# 아마존의 카테고리별 판매 순위 기능 설계하기

> 한국어 번역본입니다. 원문: [README.md](README.md) | 전체 문서: [README-ko.md](../../../README-ko.md)

*참고: 이 문서는 중복을 피하기 위해 [시스템 설계 주제](https://github.com/donnemartin/system-design-primer#index-of-system-design-topics)의 관련 항목으로 직접 링크합니다. 일반적인 논점, 트레이드오프, 대안은 링크된 내용을 참고하세요.*

## 1단계: 유스케이스와 제약 조건 정리하기

> 요구사항을 수집하고 문제 범위를 정합니다.
> 유스케이스와 제약 조건을 명확히 하기 위해 질문하세요.
> 가정에 대해 논의하세요.

명확히 답해줄 면접관이 없으므로, 여기서는 몇 가지 유스케이스와 제약 조건을 직접 정의하겠습니다.

### 유스케이스

#### 다음 유스케이스만 다루도록 문제 범위를 좁힙니다

* **서비스**가 지난주 카테고리별 인기 상품을 계산함
* **사용자**가 지난주 카테고리별 인기 상품을 조회함
* **서비스**는 고가용성을 가짐

#### 범위에서 제외

* 일반적인 이커머스 사이트 전반
    * 판매 순위 계산에 필요한 컴포넌트만 설계함

### 제약 조건과 가정

#### 가정 정리

* 트래픽은 고르게 분포하지 않음
* 상품은 여러 카테고리에 속할 수 있음
* 상품은 카테고리를 변경할 수 없음
* `foo/bar/baz` 같은 하위 카테고리는 없음
* 결과는 매시간 갱신되어야 함
    * 더 인기 있는 상품은 더 자주 갱신해야 할 수 있음
* 상품 1,000만 개
* 카테고리 1,000개
* 월 10억 건의 거래
* 월 1,000억 건의 읽기 요청
* 읽기 대 쓰기 비율 100:1

#### 사용량 계산

**봉투 뒷면 계산을 해야 하는지 면접관에게 확인하세요.**

* 거래당 크기:
    * `created_at` - 5바이트
    * `product_id` - 8바이트
    * `category_id` - 4바이트
    * `seller_id` - 8바이트
    * `buyer_id` - 8바이트
    * `quantity` - 4바이트
    * `total_price` - 5바이트
    * 합계: 약 40바이트
* 월 40 GB의 신규 거래 데이터
    * 거래당 40바이트 * 월 10억 건의 거래
    * 3년간 1.44 TB의 신규 거래 데이터
    * 대부분이 기존 거래 갱신이 아니라 신규 거래라고 가정
* 평균 초당 400건의 거래
* 평균 초당 40,000건의 읽기 요청

편리한 환산 가이드:

* 월 250만 초
* 초당 1건 = 월 250만 건
* 초당 40건 = 월 1억 건
* 초당 400건 = 월 10억 건

## 2단계: 상위 수준 설계 만들기

> 중요한 구성 요소를 모두 담아 상위 수준 설계를 그립니다.

![Imgur](http://i.imgur.com/vwMa1Qu.png)

## 3단계: 핵심 컴포넌트 설계하기

> 각 핵심 컴포넌트를 깊이 파고듭니다.

### 유스케이스: 서비스가 지난주 카테고리별 인기 상품을 계산한다

분산 파일 시스템을 직접 관리하는 대신, **판매 API** 서버의 원본 로그 파일을 Amazon S3 같은 관리형 **오브젝트 스토어**에 저장할 수 있습니다.

**코드를 얼마나 작성해야 하는지 면접관에게 확인하세요.**

로그 항목 예시는 다음과 같다고 가정합니다(탭 구분).

```
timestamp   product_id  category_id    qty     total_price   seller_id    buyer_id
t1          product1    category1      2       20.00         1            1
t2          product1    category2      2       20.00         2            2
t2          product1    category2      1       10.00         2            3
t3          product2    category1      3        7.00         3            4
t4          product3    category2      7        2.00         4            5
t5          product4    category1      1        5.00         5            6
...
```

**판매 순위 서비스**는 **판매 API** 서버 로그 파일을 입력으로 하는 **MapReduce**를 사용하고, 결과를 **SQL 데이터베이스**의 집계 테이블 `sales_rank`에 기록할 수 있습니다. [SQL과 NoSQL 선택의 유스케이스와 트레이드오프](https://github.com/donnemartin/system-design-primer#sql-or-nosql)를 논의해야 합니다.

다단계 **MapReduce**를 사용합니다.

* **1단계** - 데이터를 `(category, product_id), sum(quantity)` 형태로 변환
* **2단계** - 분산 정렬 수행

```python
class SalesRanker(MRJob):

    def within_past_week(self, timestamp):
        """Return True if timestamp is within past week, False otherwise."""
        ...

    def mapper(self, _ line):
        """Parse each log line, extract and transform relevant lines.

        Emit key value pairs of the form:

        (category1, product1), 2
        (category2, product1), 2
        (category2, product1), 1
        (category1, product2), 3
        (category2, product3), 7
        (category1, product4), 1
        """
        timestamp, product_id, category_id, quantity, total_price, seller_id, \
            buyer_id = line.split('\t')
        if self.within_past_week(timestamp):
            yield (category_id, product_id), quantity

    def reducer(self, key, value):
        """Sum values for each key.

        (category1, product1), 2
        (category2, product1), 3
        (category1, product2), 3
        (category2, product3), 7
        (category1, product4), 1
        """
        yield key, sum(values)

    def mapper_sort(self, key, value):
        """Construct key to ensure proper sorting.

        Transform key and value to the form:

        (category1, 2), product1
        (category2, 3), product1
        (category1, 3), product2
        (category2, 7), product3
        (category1, 1), product4

        The shuffle/sort step of MapReduce will then do a
        distributed sort on the keys, resulting in:

        (category1, 1), product4
        (category1, 2), product1
        (category1, 3), product2
        (category2, 3), product1
        (category2, 7), product3
        """
        category_id, product_id = key
        quantity = value
        yield (category_id, quantity), product_id

    def reducer_identity(self, key, value):
        yield key, value

    def steps(self):
        """Run the map and reduce steps."""
        return [
            self.mr(mapper=self.mapper,
                    reducer=self.reducer),
            self.mr(mapper=self.mapper_sort,
                    reducer=self.reducer_identity),
        ]
```

결과는 다음과 같이 정렬된 목록이 되며, 이를 `sales_rank` 테이블에 삽입할 수 있습니다.

```
(category1, 1), product4
(category1, 2), product1
(category1, 3), product2
(category2, 3), product1
(category2, 7), product3
```

`sales_rank` 테이블은 다음과 같은 구조를 가질 수 있습니다.

```
id int NOT NULL AUTO_INCREMENT
category_id int NOT NULL
total_sold int NOT NULL
product_id int NOT NULL
PRIMARY KEY(id)
FOREIGN KEY(category_id) REFERENCES Categories(id)
FOREIGN KEY(product_id) REFERENCES Products(id)
```

조회 속도를 높이고(전체 테이블 스캔 대신 로그 시간) 데이터를 메모리에 유지하기 위해 `id`, `category_id`, `product_id`에 [인덱스](https://github.com/donnemartin/system-design-primer#use-good-indices)를 생성합니다. 메모리에서 1 MB를 순차적으로 읽는 데 약 250마이크로초가 걸리는 반면, SSD에서는 4배, 디스크에서는 80배 더 오래 걸립니다.<sup><a href=https://github.com/donnemartin/system-design-primer#latency-numbers-every-programmer-should-know>1</a></sup>

### 유스케이스: 사용자가 지난주 카테고리별 인기 상품을 조회한다

* **클라이언트**가 [리버스 프록시](https://github.com/donnemartin/system-design-primer#reverse-proxy-web-server)로 동작하는 **웹 서버**에 요청을 보냄
* **웹 서버**가 요청을 **읽기 API** 서버로 전달함
* **읽기 API** 서버가 **SQL 데이터베이스**의 `sales_rank` 테이블을 읽음

공개 [**REST API**](https://github.com/donnemartin/system-design-primer#representational-state-transfer-rest)를 사용합니다.

```
$ curl https://amazon.com/api/v1/popular?category_id=1234
```

응답:

```
{
    "id": "100",
    "category_id": "1234",
    "total_sold": "100000",
    "product_id": "50",
},
{
    "id": "53",
    "category_id": "1234",
    "total_sold": "90000",
    "product_id": "200",
},
{
    "id": "75",
    "category_id": "1234",
    "total_sold": "80000",
    "product_id": "3",
},
```

내부 통신에는 [원격 프로시저 호출(RPC)](https://github.com/donnemartin/system-design-primer#remote-procedure-call-rpc)을 사용할 수 있습니다.

## 4단계: 설계 확장하기

> 제약 조건을 고려해 병목을 찾아내고 해결합니다.

![Imgur](http://i.imgur.com/MzExP06.png)

**중요: 초기 설계에서 최종 설계로 곧장 건너뛰지 마세요!**

다음을 수행하겠다고 말하세요. 1) **벤치마크/부하 테스트**, 2) 병목 **프로파일링**, 3) 대안과 트레이드오프를 평가하며 병목 해결, 4) 반복. 초기 설계를 반복적으로 확장하는 예시는 [AWS에서 수백만 사용자까지 확장되는 시스템 설계하기](../scaling_aws/README-ko.md)를 참고하세요.

초기 설계에서 어떤 병목을 만날 수 있고 각각을 어떻게 해결할지 논의하는 것이 중요합니다. 예를 들어 여러 대의 **웹 서버**와 함께 **로드 밸런서**를 추가하면 어떤 문제가 해결될까요? **CDN**은요? **마스터-슬레이브 복제본**은요? 각각의 대안과 **트레이드오프**는 무엇일까요?

설계를 완성하고 확장성 문제를 해결하기 위해 몇 가지 컴포넌트를 추가합니다. 복잡해지지 않도록 내부 로드 밸런서는 표시하지 않았습니다.

*논의 중복을 피하기 위해*, 주요 논점·트레이드오프·대안은 다음 [시스템 설계 주제](https://github.com/donnemartin/system-design-primer#index-of-system-design-topics)를 참고하세요.

* [DNS](https://github.com/donnemartin/system-design-primer#domain-name-system)
* [CDN](https://github.com/donnemartin/system-design-primer#content-delivery-network)
* [로드 밸런서](https://github.com/donnemartin/system-design-primer#load-balancer)
* [수평 확장](https://github.com/donnemartin/system-design-primer#horizontal-scaling)
* [웹 서버(리버스 프록시)](https://github.com/donnemartin/system-design-primer#reverse-proxy-web-server)
* [API 서버(애플리케이션 계층)](https://github.com/donnemartin/system-design-primer#application-layer)
* [캐시](https://github.com/donnemartin/system-design-primer#cache)
* [관계형 데이터베이스 관리 시스템(RDBMS)](https://github.com/donnemartin/system-design-primer#relational-database-management-system-rdbms)
* [SQL 쓰기 마스터-슬레이브 장애 조치](https://github.com/donnemartin/system-design-primer#fail-over)
* [마스터-슬레이브 복제](https://github.com/donnemartin/system-design-primer#master-slave-replication)
* [일관성 패턴](https://github.com/donnemartin/system-design-primer#consistency-patterns)
* [가용성 패턴](https://github.com/donnemartin/system-design-primer#availability-patterns)

**분석 데이터베이스**는 Amazon Redshift나 Google BigQuery 같은 데이터 웨어하우징 솔루션을 사용할 수 있습니다.

데이터베이스에는 제한된 기간의 데이터만 저장하고 나머지는 데이터 웨어하우스나 **오브젝트 스토어**에 저장하고 싶을 수 있습니다. Amazon S3 같은 **오브젝트 스토어**는 월 40 GB의 신규 콘텐츠라는 제약 조건을 여유롭게 감당할 수 있습니다.

*평균* 초당 40,000건의 읽기 요청(피크에는 더 높음)을 처리하기 위해, 인기 콘텐츠(와 그 판매 순위) 트래픽은 데이터베이스가 아니라 **메모리 캐시**가 처리해야 합니다. **메모리 캐시**는 고르지 않게 분포한 트래픽과 트래픽 급증을 처리하는 데도 유용합니다. 읽기 양이 많으므로 **SQL 읽기 복제본**만으로는 캐시 미스를 감당하지 못할 수 있습니다. 추가적인 SQL 확장 패턴을 적용해야 할 가능성이 높습니다.

*평균* 초당 400건의 쓰기(피크에는 더 높음)는 단일 **SQL 쓰기 마스터-슬레이브**에는 부담스러울 수 있어, 이 역시 추가적인 확장 기법이 필요함을 시사합니다.

SQL 확장 패턴에는 다음이 있습니다.

* [페더레이션](https://github.com/donnemartin/system-design-primer#federation)
* [샤딩](https://github.com/donnemartin/system-design-primer#sharding)
* [비정규화](https://github.com/donnemartin/system-design-primer#denormalization)
* [SQL 튜닝](https://github.com/donnemartin/system-design-primer#sql-tuning)

일부 데이터를 **NoSQL 데이터베이스**로 옮기는 것도 고려해야 합니다.

## 추가로 논의할 만한 주제

> 문제 범위와 남은 시간에 따라 더 깊이 파고들 수 있는 주제입니다.

#### NoSQL

* [키-값 저장소](https://github.com/donnemartin/system-design-primer#key-value-store)
* [문서 저장소](https://github.com/donnemartin/system-design-primer#document-store)
* [와이드 칼럼 저장소](https://github.com/donnemartin/system-design-primer#wide-column-store)
* [그래프 데이터베이스](https://github.com/donnemartin/system-design-primer#graph-database)
* [SQL vs NoSQL](https://github.com/donnemartin/system-design-primer#sql-or-nosql)

### 캐싱

* 어디에 캐시할 것인가
    * [클라이언트 캐싱](https://github.com/donnemartin/system-design-primer#client-caching)
    * [CDN 캐싱](https://github.com/donnemartin/system-design-primer#cdn-caching)
    * [웹 서버 캐싱](https://github.com/donnemartin/system-design-primer#web-server-caching)
    * [데이터베이스 캐싱](https://github.com/donnemartin/system-design-primer#database-caching)
    * [애플리케이션 캐싱](https://github.com/donnemartin/system-design-primer#application-caching)
* 무엇을 캐시할 것인가
    * [데이터베이스 쿼리 수준의 캐싱](https://github.com/donnemartin/system-design-primer#caching-at-the-database-query-level)
    * [객체 수준의 캐싱](https://github.com/donnemartin/system-design-primer#caching-at-the-object-level)
* 언제 캐시를 갱신할 것인가
    * [캐시 어사이드](https://github.com/donnemartin/system-design-primer#cache-aside)
    * [라이트 스루](https://github.com/donnemartin/system-design-primer#write-through)
    * [라이트 비하인드(write-back)](https://github.com/donnemartin/system-design-primer#write-behind-write-back)
    * [리프레시 어헤드](https://github.com/donnemartin/system-design-primer#refresh-ahead)

### 비동기와 마이크로서비스

* [메시지 큐](https://github.com/donnemartin/system-design-primer#message-queues)
* [태스크 큐](https://github.com/donnemartin/system-design-primer#task-queues)
* [배압](https://github.com/donnemartin/system-design-primer#back-pressure)
* [마이크로서비스](https://github.com/donnemartin/system-design-primer#microservices)

### 통신

* 트레이드오프 논의:
    * 클라이언트와의 외부 통신 - [REST를 따르는 HTTP API](https://github.com/donnemartin/system-design-primer#representational-state-transfer-rest)
    * 내부 통신 - [RPC](https://github.com/donnemartin/system-design-primer#remote-procedure-call-rpc)
* [서비스 디스커버리](https://github.com/donnemartin/system-design-primer#service-discovery)

### 보안

[보안 섹션](https://github.com/donnemartin/system-design-primer#security)을 참고하세요.

### 지연 시간 수치

[모든 프로그래머가 알아야 할 지연 시간 수치](https://github.com/donnemartin/system-design-primer#latency-numbers-every-programmer-should-know)를 참고하세요.

### 지속적으로 할 일

* 병목이 나타날 때마다 해결할 수 있도록 시스템 벤치마킹과 모니터링을 계속하세요.
* 확장은 반복적인 과정입니다.
