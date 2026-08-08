# Mint.com 설계하기

> 한국어 번역본입니다. 원문: [README.md](README.md) | 전체 문서: [README-ko.md](../../../README-ko.md)

*참고: 이 문서는 중복을 피하기 위해 [시스템 설계 주제](https://github.com/donnemartin/system-design-primer#index-of-system-design-topics)의 관련 항목으로 직접 링크합니다. 일반적인 논점, 트레이드오프, 대안은 링크된 내용을 참고하세요.*

## 1단계: 유스케이스와 제약 조건 정리하기

> 요구사항을 수집하고 문제 범위를 정합니다.
> 유스케이스와 제약 조건을 명확히 하기 위해 질문하세요.
> 가정에 대해 논의하세요.

명확히 답해줄 면접관이 없으므로, 여기서는 몇 가지 유스케이스와 제약 조건을 직접 정의하겠습니다.

### 유스케이스

#### 다음 유스케이스만 다루도록 문제 범위를 좁힙니다

* **사용자**가 금융 계좌를 연결함
* **서비스**가 계좌에서 거래 내역을 추출함
    * 매일 갱신
    * 거래를 분류(categorize)함
        * 사용자가 수동으로 카테고리를 덮어쓸 수 있음
        * 자동 재분류는 없음
    * 카테고리별 월간 지출을 분석함
* **서비스**가 예산을 추천함
    * 사용자가 예산을 직접 설정할 수 있음
    * 예산에 근접하거나 초과하면 알림을 보냄
* **서비스**는 고가용성을 가짐

#### 범위에서 제외

* **서비스**가 추가적인 로깅과 분석을 수행함

### 제약 조건과 가정

#### 가정 정리

* 트래픽은 고르게 분포하지 않음
* 계좌의 자동 일간 갱신은 지난 30일간 활동한 사용자에게만 적용됨
* 금융 계좌 추가나 삭제는 비교적 드묾
* 예산 알림은 즉각적일 필요 없음
* 사용자 1,000만 명
    * 사용자당 예산 카테고리 10개 = 예산 항목 1억 개
    * 카테고리 예시:
        * 주거 = $1,000
        * 식비 = $200
        * 주유 = $100
    * 거래 카테고리를 판단하는 데 판매자(seller) 정보를 사용
        * 판매자 5만 곳
* 금융 계좌 3,000만 개
* 월 50억 건의 거래
* 월 5억 건의 읽기 요청
* 쓰기 대 읽기 비율 10:1
    * 쓰기가 많음. 사용자는 매일 거래하지만 사이트를 매일 방문하는 사람은 적음

#### 사용량 계산

**봉투 뒷면 계산을 해야 하는지 면접관에게 확인하세요.**

* 거래당 크기:
    * `user_id` - 8바이트
    * `created_at` - 5바이트
    * `seller` - 32바이트
    * `amount` - 5바이트
    * 합계: 약 50바이트
* 월 250 GB의 신규 거래 데이터
    * 거래당 50바이트 * 월 50억 건의 거래
    * 3년간 9 TB의 신규 거래 데이터
    * 대부분이 기존 거래 갱신이 아니라 신규 거래라고 가정
* 평균 초당 2,000건의 거래
* 평균 초당 200건의 읽기 요청

편리한 환산 가이드:

* 월 250만 초
* 초당 1건 = 월 250만 건
* 초당 40건 = 월 1억 건
* 초당 400건 = 월 10억 건

## 2단계: 상위 수준 설계 만들기

> 중요한 구성 요소를 모두 담아 상위 수준 설계를 그립니다.

![Imgur](http://i.imgur.com/E8klrBh.png)

## 3단계: 핵심 컴포넌트 설계하기

> 각 핵심 컴포넌트를 깊이 파고듭니다.

### 유스케이스: 사용자가 금융 계좌를 연결한다

1,000만 명의 사용자 정보는 [관계형 데이터베이스](https://github.com/donnemartin/system-design-primer#relational-database-management-system-rdbms)에 저장할 수 있습니다. [SQL과 NoSQL 선택의 유스케이스와 트레이드오프](https://github.com/donnemartin/system-design-primer#sql-or-nosql)를 논의해야 합니다.

* **클라이언트**가 [리버스 프록시](https://github.com/donnemartin/system-design-primer#reverse-proxy-web-server)로 동작하는 **웹 서버**에 요청을 보냄
* **웹 서버**가 요청을 **계좌 API** 서버로 전달함
* **계좌 API** 서버가 새로 입력된 계좌 정보로 **SQL 데이터베이스**의 `accounts` 테이블을 갱신함

**코드를 얼마나 작성해야 하는지 면접관에게 확인하세요.**

`accounts` 테이블은 다음과 같은 구조를 가질 수 있습니다.

```
id int NOT NULL AUTO_INCREMENT
created_at datetime NOT NULL
last_update datetime NOT NULL
account_url varchar(255) NOT NULL
account_login varchar(32) NOT NULL
account_password_hash char(64) NOT NULL
user_id int NOT NULL
PRIMARY KEY(id)
FOREIGN KEY(user_id) REFERENCES users(id)
```

조회 속도를 높이고(전체 테이블 스캔 대신 로그 시간) 데이터를 메모리에 유지하기 위해 `id`, `user_id`, `created_at`에 [인덱스](https://github.com/donnemartin/system-design-primer#use-good-indices)를 생성합니다. 메모리에서 1 MB를 순차적으로 읽는 데 약 250마이크로초가 걸리는 반면, SSD에서는 4배, 디스크에서는 80배 더 오래 걸립니다.<sup><a href=https://github.com/donnemartin/system-design-primer#latency-numbers-every-programmer-should-know>1</a></sup>

공개 [**REST API**](https://github.com/donnemartin/system-design-primer#representational-state-transfer-rest)를 사용합니다.

```
$ curl -X POST --data '{ "user_id": "foo", "account_url": "bar", \
    "account_login": "baz", "account_password": "qux" }' \
    https://mint.com/api/v1/account
```

내부 통신에는 [원격 프로시저 호출(RPC)](https://github.com/donnemartin/system-design-primer#remote-procedure-call-rpc)을 사용할 수 있습니다.

다음으로 서비스가 계좌에서 거래 내역을 추출합니다.

### 유스케이스: 서비스가 계좌에서 거래 내역을 추출한다

다음의 경우에 계좌에서 정보를 추출해야 합니다.

* 사용자가 계좌를 처음 연결할 때
* 사용자가 수동으로 계좌를 새로고침할 때
* 지난 30일간 활동한 사용자에 대해 매일 자동으로

데이터 흐름:

* **클라이언트**가 **웹 서버**에 요청을 보냄
* **웹 서버**가 요청을 **계좌 API** 서버로 전달함
* **계좌 API** 서버가 [Amazon SQS](https://aws.amazon.com/sqs/)나 [RabbitMQ](https://www.rabbitmq.com/) 같은 **큐**에 작업을 등록함
    * 거래 추출은 시간이 걸릴 수 있으므로 [큐를 이용한 비동기 처리](https://github.com/donnemartin/system-design-primer#asynchronism)가 바람직합니다. 다만 복잡성이 추가됩니다.
* **거래 추출 서비스**는 다음을 수행함
    * **큐**에서 작업을 가져와 해당 계좌의 거래 내역을 금융기관에서 추출하고, 결과를 원본 로그 파일로 **오브젝트 스토어**에 저장
    * **카테고리 서비스**를 사용해 각 거래를 분류
    * **예산 서비스**를 사용해 카테고리별 월간 지출 합계를 계산
        * **예산 서비스**는 **알림 서비스**를 사용해 사용자가 예산에 근접하거나 초과했는지 알림
    * 분류된 거래로 **SQL 데이터베이스**의 `transactions` 테이블을 갱신
    * 카테고리별 월간 지출 합계로 **SQL 데이터베이스**의 `monthly_spending` 테이블을 갱신
    * **알림 서비스**를 통해 거래 처리가 완료되었음을 사용자에게 알림
        * **큐**(그림에는 없음)를 사용해 비동기적으로 알림을 전송

`transactions` 테이블은 다음과 같은 구조를 가질 수 있습니다.

```
id int NOT NULL AUTO_INCREMENT
created_at datetime NOT NULL
seller varchar(32) NOT NULL
amount decimal NOT NULL
user_id int NOT NULL
PRIMARY KEY(id)
FOREIGN KEY(user_id) REFERENCES users(id)
```

`id`, `user_id`, `created_at`에 [인덱스](https://github.com/donnemartin/system-design-primer#use-good-indices)를 생성합니다.

`monthly_spending` 테이블은 다음과 같은 구조를 가질 수 있습니다.

```
id int NOT NULL AUTO_INCREMENT
month_year date NOT NULL
category varchar(32)
amount decimal NOT NULL
user_id int NOT NULL
PRIMARY KEY(id)
FOREIGN KEY(user_id) REFERENCES users(id)
```

`id`와 `user_id`에 [인덱스](https://github.com/donnemartin/system-design-primer#use-good-indices)를 생성합니다.

#### 카테고리 서비스

**카테고리 서비스**를 위해, 가장 인기 있는 판매자들로 판매자→카테고리 딕셔너리를 미리 채워둘 수 있습니다. 판매자를 5만 곳으로 추정하고 각 항목이 255바이트 미만이라고 추정하면, 이 딕셔너리는 메모리를 약 12 MB만 차지합니다.

**코드를 얼마나 작성해야 하는지 면접관에게 확인하세요.**

```python
class DefaultCategories(Enum):

    HOUSING = 0
    FOOD = 1
    GAS = 2
    SHOPPING = 3
    ...

seller_category_map = {}
seller_category_map['Exxon'] = DefaultCategories.GAS
seller_category_map['Target'] = DefaultCategories.SHOPPING
...
```

초기에 맵에 채워지지 않은 판매자에 대해서는, 사용자가 제공한 수동 카테고리 덮어쓰기를 평가하는 크라우드소싱 방식을 사용할 수 있습니다. 힙(heap)을 사용하면 판매자별 최다 수동 덮어쓰기를 O(1) 시간에 빠르게 조회할 수 있습니다.

```python
class Categorizer(object):

    def __init__(self, seller_category_map, seller_category_crowd_overrides_map):
        self.seller_category_map = seller_category_map
        self.seller_category_crowd_overrides_map = \
            seller_category_crowd_overrides_map

    def categorize(self, transaction):
        if transaction.seller in self.seller_category_map:
            return self.seller_category_map[transaction.seller]
        elif transaction.seller in self.seller_category_crowd_overrides_map:
            self.seller_category_map[transaction.seller] = \
                self.seller_category_crowd_overrides_map[transaction.seller].peek_min()
            return self.seller_category_map[transaction.seller]
        return None
```

Transaction 구현:

```python
class Transaction(object):

    def __init__(self, created_at, seller, amount):
        self.created_at = created_at
        self.seller = seller
        self.amount = amount
```

### 유스케이스: 서비스가 예산을 추천한다

우선, 소득 구간에 따라 카테고리별 금액을 배분하는 일반적인 예산 템플릿을 사용할 수 있습니다. 이 방식을 쓰면 제약 조건에서 언급한 1억 개의 예산 항목을 모두 저장할 필요 없이, 사용자가 덮어쓴 항목만 저장하면 됩니다. 사용자가 예산 카테고리를 덮어쓰면 그 값을 `TABLE budget_overrides`에 저장할 수 있습니다.

```python
class Budget(object):

    def __init__(self, income):
        self.income = income
        self.categories_to_budget_map = self.create_budget_template()

    def create_budget_template(self):
        return {
            DefaultCategories.HOUSING: self.income * .4,
            DefaultCategories.FOOD: self.income * .2,
            DefaultCategories.GAS: self.income * .1,
            DefaultCategories.SHOPPING: self.income * .2,
            ...
        }

    def override_category_budget(self, category, amount):
        self.categories_to_budget_map[category] = amount
```

**예산 서비스**에서는 `transactions` 테이블에 SQL 질의를 실행해 `monthly_spending` 집계 테이블을 생성할 수 있습니다. 사용자는 보통 한 달에 여러 건의 거래를 하므로, `monthly_spending` 테이블의 행 수는 전체 50억 건의 거래보다 훨씬 적을 것입니다.

대안으로, 원본 거래 파일에 대해 **MapReduce** 작업을 실행해 다음을 수행할 수 있습니다.

* 각 거래를 분류
* 카테고리별 월간 지출 합계를 생성

거래 파일에 대해 분석을 실행하면 데이터베이스 부하를 크게 줄일 수 있습니다.

사용자가 카테고리를 갱신하면 **예산 서비스**를 호출해 분석을 다시 실행할 수 있습니다.

**코드를 얼마나 작성해야 하는지 면접관에게 확인하세요.**

로그 파일 형식 예시(탭 구분):

```
user_id   timestamp   seller  amount
```

**MapReduce** 구현:

```python
class SpendingByCategory(MRJob):

    def __init__(self, categorizer):
        self.categorizer = categorizer
        self.current_year_month = calc_current_year_month()
        ...

    def calc_current_year_month(self):
        """Return the current year and month."""
        ...

    def extract_year_month(self, timestamp):
        """Return the year and month portions of the timestamp."""
        ...

    def handle_budget_notifications(self, key, total):
        """Call notification API if nearing or exceeded budget."""
        ...

    def mapper(self, _, line):
        """Parse each log line, extract and transform relevant lines.

        Argument line will be of the form:

        user_id   timestamp   seller  amount

        Using the categorizer to convert seller to category,
        emit key value pairs of the form:

        (user_id, 2016-01, shopping), 25
        (user_id, 2016-01, shopping), 100
        (user_id, 2016-01, gas), 50
        """
        user_id, timestamp, seller, amount = line.split('\t')
        category = self.categorizer.categorize(seller)
        period = self.extract_year_month(timestamp)
        if period == self.current_year_month:
            yield (user_id, period, category), amount

    def reducer(self, key, value):
        """Sum values for each key.

        (user_id, 2016-01, shopping), 125
        (user_id, 2016-01, gas), 50
        """
        total = sum(values)
        yield key, sum(values)
```

## 4단계: 설계 확장하기

> 제약 조건을 고려해 병목을 찾아내고 해결합니다.

![Imgur](http://i.imgur.com/V5q57vU.png)

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
* [비동기](https://github.com/donnemartin/system-design-primer#asynchronism)
* [일관성 패턴](https://github.com/donnemartin/system-design-primer#consistency-patterns)
* [가용성 패턴](https://github.com/donnemartin/system-design-primer#availability-patterns)

유스케이스를 하나 추가합니다. **사용자**가 요약과 거래 내역을 조회한다.

사용자 세션, 카테고리별 집계 통계, 최근 거래 내역은 Redis나 Memcached 같은 **메모리 캐시**에 둘 수 있습니다.

* **클라이언트**가 **웹 서버**에 읽기 요청을 보냄
* **웹 서버**가 요청을 **읽기 API** 서버로 전달함
    * 정적 콘텐츠는 S3 같은 **오브젝트 스토어**에서 제공하며 **CDN**에 캐시됨
* **읽기 API** 서버는 다음을 수행함
    * **메모리 캐시**에서 콘텐츠를 확인
        * URL이 **메모리 캐시**에 있으면 캐시된 내용을 반환
        * 없으면
            * URL이 **SQL 데이터베이스**에 있으면 내용을 가져옴
                * 그 내용으로 **메모리 캐시**를 갱신

트레이드오프와 대안은 [캐시를 갱신하는 시점](https://github.com/donnemartin/system-design-primer#when-to-update-the-cache)을 참고하세요. 위에서 설명한 방식은 [캐시 어사이드](https://github.com/donnemartin/system-design-primer#cache-aside)입니다.

`monthly_spending` 집계 테이블을 **SQL 데이터베이스**에 두는 대신, Amazon Redshift나 Google BigQuery 같은 데이터 웨어하우징 솔루션으로 별도의 **분석 데이터베이스**를 만들 수 있습니다.

데이터베이스에는 한 달치 `transactions` 데이터만 저장하고 나머지는 데이터 웨어하우스나 **오브젝트 스토어**에 저장하고 싶을 수 있습니다. Amazon S3 같은 **오브젝트 스토어**는 월 250 GB의 신규 콘텐츠라는 제약 조건을 여유롭게 감당할 수 있습니다.

*평균* 초당 200건의 읽기 요청(피크에는 더 높음)을 처리하기 위해, 인기 콘텐츠 트래픽은 데이터베이스가 아니라 **메모리 캐시**가 처리해야 합니다. **메모리 캐시**는 고르지 않게 분포한 트래픽과 트래픽 급증을 처리하는 데도 유용합니다. **SQL 읽기 복제본**은 쓰기 복제에 매여 있지만 않다면 캐시 미스를 감당할 수 있어야 합니다.

*평균* 초당 2,000건의 거래 쓰기(피크에는 더 높음)는 단일 **SQL 쓰기 마스터-슬레이브**에는 부담스러울 수 있습니다. 추가적인 SQL 확장 패턴을 적용해야 할 수 있습니다.

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
