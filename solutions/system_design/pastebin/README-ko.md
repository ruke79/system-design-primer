# Pastebin.com(또는 Bit.ly) 설계하기

> 한국어 번역본입니다. 원문: [README.md](README.md) | 전체 문서: [README-ko.md](../../../README-ko.md)

*참고: 이 문서는 중복을 피하기 위해 [시스템 설계 주제](https://github.com/donnemartin/system-design-primer#index-of-system-design-topics)의 관련 항목으로 직접 링크합니다. 일반적인 논점, 트레이드오프, 대안은 링크된 내용을 참고하세요.*

**Bit.ly 설계하기**도 비슷한 문제입니다. 다만 pastebin은 원래의 축약되지 않은 URL 대신 붙여넣은 내용(paste contents)을 저장해야 한다는 점이 다릅니다.

## 1단계: 유스케이스와 제약 조건 정리하기

> 요구사항을 수집하고 문제 범위를 정합니다.
> 유스케이스와 제약 조건을 명확히 하기 위해 질문하세요.
> 가정에 대해 논의하세요.

명확히 답해줄 면접관이 없으므로, 여기서는 몇 가지 유스케이스와 제약 조건을 직접 정의하겠습니다.

### 유스케이스

#### 다음 유스케이스만 다루도록 문제 범위를 좁힙니다

* **사용자**가 텍스트 블록을 입력하면 무작위로 생성된 링크를 받음
    * 만료
        * 기본 설정은 만료되지 않음
        * 선택적으로 시간 기반 만료를 설정할 수 있음
* **사용자**가 paste의 URL을 입력하면 내용을 조회함
* **사용자**는 익명임
* **서비스**가 페이지 분석 데이터를 추적함
    * 월별 방문 통계
* **서비스**가 만료된 paste를 삭제함
* **서비스**는 고가용성을 가짐

#### 범위에서 제외

* **사용자**가 계정을 등록함
    * **사용자**가 이메일을 인증함
* **사용자**가 등록된 계정으로 로그인함
    * **사용자**가 문서를 편집함
* **사용자**가 공개 범위를 설정할 수 있음
* **사용자**가 단축 링크를 직접 지정할 수 있음

### 제약 조건과 가정

#### 가정 정리

* 트래픽은 고르게 분포하지 않음
* 단축 링크를 따라가는 것은 빨라야 함
* paste는 텍스트만 지원
* 페이지 조회 분석은 실시간일 필요 없음
* 사용자 1,000만 명
* 월 paste 쓰기 1,000만 건
* 월 paste 읽기 1억 건
* 읽기 대 쓰기 비율 10:1

#### 사용량 계산

**봉투 뒷면 계산을 해야 하는지 면접관에게 확인하세요.**

* paste당 크기
    * paste당 콘텐츠 1 KB
    * `shortlink` - 7바이트
    * `expiration_length_in_minutes` - 4바이트
    * `created_at` - 5바이트
    * `paste_path` - 255바이트
    * 합계 = 약 1.27 KB
* 월 12.7 GB의 신규 paste 콘텐츠
    * paste당 1.27 KB * 월 1,000만 paste
    * 3년간 약 450 GB의 신규 paste 콘텐츠
    * 3년간 3억 6,000만 개의 단축 링크
    * 대부분이 기존 paste 갱신이 아니라 신규 paste라고 가정
* 평균 초당 4건의 paste 쓰기
* 평균 초당 40건의 읽기 요청

편리한 환산 가이드:

* 월 250만 초
* 초당 1건 = 월 250만 건
* 초당 40건 = 월 1억 건
* 초당 400건 = 월 10억 건

## 2단계: 상위 수준 설계 만들기

> 중요한 구성 요소를 모두 담아 상위 수준 설계를 그립니다.

![Imgur](http://i.imgur.com/BKsBnmG.png)

## 3단계: 핵심 컴포넌트 설계하기

> 각 핵심 컴포넌트를 깊이 파고듭니다.

### 유스케이스: 사용자가 텍스트 블록을 입력하면 무작위로 생성된 링크를 받는다

[관계형 데이터베이스](https://github.com/donnemartin/system-design-primer#relational-database-management-system-rdbms)를 거대한 해시 테이블처럼 사용하여, 생성된 URL을 paste 파일이 있는 파일 서버와 경로에 매핑할 수 있습니다.

파일 서버를 직접 관리하는 대신 Amazon S3 같은 관리형 **오브젝트 스토어**나 [NoSQL 문서 저장소](https://github.com/donnemartin/system-design-primer#document-store)를 사용할 수도 있습니다.

관계형 데이터베이스를 거대한 해시 테이블로 쓰는 대신 [NoSQL 키-값 저장소](https://github.com/donnemartin/system-design-primer#key-value-store)를 사용하는 방법도 있습니다. [SQL과 NoSQL 선택의 트레이드오프](https://github.com/donnemartin/system-design-primer#sql-or-nosql)를 논의해야 합니다. 아래 설명은 관계형 데이터베이스 방식을 사용합니다.

* **클라이언트**가 [리버스 프록시](https://github.com/donnemartin/system-design-primer#reverse-proxy-web-server)로 동작하는 **웹 서버**에 paste 생성 요청을 보냄
* **웹 서버**가 요청을 **쓰기 API** 서버로 전달함
* **쓰기 API** 서버는 다음을 수행함
    * 고유한 URL을 생성
        * **SQL 데이터베이스**에서 중복을 조회해 URL이 고유한지 확인
        * 고유하지 않으면 다른 URL을 생성
        * 사용자 지정 URL을 지원한다면 사용자가 제공한 값을 사용(마찬가지로 중복 확인)
    * **SQL 데이터베이스**의 `pastes` 테이블에 저장
    * paste 데이터를 **오브젝트 스토어**에 저장
    * URL을 반환

**코드를 얼마나 작성해야 하는지 면접관에게 확인하세요.**

`pastes` 테이블은 다음과 같은 구조를 가질 수 있습니다.

```
shortlink char(7) NOT NULL
expiration_length_in_minutes int NOT NULL
created_at datetime NOT NULL
paste_path varchar(255) NOT NULL
PRIMARY KEY(shortlink)
```

기본 키를 `shortlink` 칼럼 기준으로 설정하면 데이터베이스가 고유성을 강제하는 데 사용하는 [인덱스](https://github.com/donnemartin/system-design-primer#use-good-indices)가 생성됩니다. 조회 속도를 높이고(전체 테이블 스캔 대신 로그 시간) 데이터를 메모리에 유지하기 위해 `created_at`에도 인덱스를 추가로 만듭니다. 메모리에서 1 MB를 순차적으로 읽는 데 약 250마이크로초가 걸리는 반면, SSD에서는 4배, 디스크에서는 80배 더 오래 걸립니다.<sup><a href=https://github.com/donnemartin/system-design-primer#latency-numbers-every-programmer-should-know>1</a></sup>

고유한 URL을 생성하는 방법은 다음과 같습니다.

* 사용자의 ip_address + timestamp에 대한 [**MD5**](https://en.wikipedia.org/wiki/MD5) 해시를 구함
    * MD5는 128비트 해시 값을 만들어내는 널리 쓰이는 해시 함수임
    * MD5는 균일하게 분포함
    * 대안으로 무작위 생성 데이터의 MD5 해시를 구할 수도 있음
* MD5 해시를 [**Base 62**](https://www.kerstner.at/2012/07/shortening-strings-using-base-62-encoding/)로 인코딩함
    * Base 62는 `[a-zA-Z0-9]`로 인코딩하므로 URL에 적합하며 특수 문자 이스케이프가 필요 없음
    * 원본 입력에 대한 해시 결과는 하나뿐이고 Base 62는 결정적임(무작위성이 없음)
    * Base 64도 널리 쓰이는 인코딩이지만 `+`와 `/` 문자 때문에 URL에서는 문제가 됨
    * 아래 [Base 62 의사코드](http://stackoverflow.com/questions/742013/how-to-code-a-url-shortener)는 자릿수 k(=7)에 대해 O(k) 시간에 동작함

```python
def base_encode(num, base=62):
    digits = []
    while num > 0
      remainder = modulo(num, base)
      digits.push(remainder)
      num = divide(num, base)
    digits = digits.reverse
```

* 출력의 앞 7글자를 취하면 62^7가지 값이 가능하므로, 3년간 3억 6,000만 개의 단축 링크라는 제약 조건을 충분히 감당할 수 있습니다.

```python
url = base_encode(md5(ip_address+timestamp))[:URL_LENGTH]
```

공개 [**REST API**](https://github.com/donnemartin/system-design-primer#representational-state-transfer-rest)를 사용합니다.

```
$ curl -X POST --data '{ "expiration_length_in_minutes": "60", \
    "paste_contents": "Hello World!" }' https://pastebin.com/api/v1/paste
```

응답:

```
{
    "shortlink": "foobar"
}
```

내부 통신에는 [원격 프로시저 호출(RPC)](https://github.com/donnemartin/system-design-primer#remote-procedure-call-rpc)을 사용할 수 있습니다.

### 유스케이스: 사용자가 paste의 URL을 입력하면 내용을 조회한다

* **클라이언트**가 **웹 서버**에 paste 조회 요청을 보냄
* **웹 서버**가 요청을 **읽기 API** 서버로 전달함
* **읽기 API** 서버는 다음을 수행함
    * 생성된 URL을 **SQL 데이터베이스**에서 확인
        * URL이 **SQL 데이터베이스**에 있으면 **오브젝트 스토어**에서 paste 내용을 가져옴
        * 없으면 사용자에게 오류 메시지를 반환

REST API:

```
$ curl https://pastebin.com/api/v1/paste?shortlink=foobar
```

응답:

```
{
    "paste_contents": "Hello World"
    "created_at": "YYYY-MM-DD HH:MM:SS"
    "expiration_length_in_minutes": "60"
}
```

### 유스케이스: 서비스가 페이지 분석 데이터를 추적한다

실시간 분석이 요구사항이 아니므로, **웹 서버** 로그를 **MapReduce**로 처리해 조회 수를 집계하면 됩니다.

**코드를 얼마나 작성해야 하는지 면접관에게 확인하세요.**

```python
class HitCounts(MRJob):

    def extract_url(self, line):
        """Extract the generated url from the log line."""
        ...

    def extract_year_month(self, line):
        """Return the year and month portions of the timestamp."""
        ...

    def mapper(self, _, line):
        """Parse each log line, extract and transform relevant lines.

        Emit key value pairs of the form:

        (2016-01, url0), 1
        (2016-01, url0), 1
        (2016-01, url1), 1
        """
        url = self.extract_url(line)
        period = self.extract_year_month(line)
        yield (period, url), 1

    def reducer(self, key, values):
        """Sum values for each key.

        (2016-01, url0), 2
        (2016-01, url1), 1
        """
        yield key, sum(values)
```

### 유스케이스: 서비스가 만료된 paste를 삭제한다

만료된 paste를 삭제하려면 **SQL 데이터베이스**에서 만료 타임스탬프가 현재 타임스탬프보다 오래된 모든 항목을 스캔하면 됩니다. 그런 다음 만료된 항목을 테이블에서 삭제(또는 만료로 표시)합니다.

## 4단계: 설계 확장하기

> 제약 조건을 고려해 병목을 찾아내고 해결합니다.

![Imgur](http://i.imgur.com/4edXG0T.png)

**중요: 초기 설계에서 최종 설계로 곧장 건너뛰지 마세요!**

다음을 반복적으로 수행하겠다고 말하세요. 1) **벤치마크/부하 테스트**, 2) 병목 **프로파일링**, 3) 대안과 트레이드오프를 평가하며 병목 해결, 4) 반복. 초기 설계를 반복적으로 확장하는 예시는 [AWS에서 수백만 사용자까지 확장되는 시스템 설계하기](../scaling_aws/README-ko.md)를 참고하세요.

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

Amazon S3 같은 **오브젝트 스토어**는 월 12.7 GB의 신규 콘텐츠라는 제약 조건을 여유롭게 감당할 수 있습니다.

*평균* 초당 40건의 읽기 요청(피크에는 더 높음)을 처리하기 위해, 인기 콘텐츠 트래픽은 데이터베이스가 아니라 **메모리 캐시**가 처리해야 합니다. **메모리 캐시**는 고르지 않게 분포한 트래픽과 트래픽 급증을 처리하는 데도 유용합니다. **SQL 읽기 복제본**은 쓰기 복제에 매여 있지만 않다면 캐시 미스를 감당할 수 있어야 합니다.

*평균* 초당 4건의 paste 쓰기(피크에는 더 높음)는 단일 **SQL 쓰기 마스터-슬레이브**로 처리 가능해야 합니다. 그렇지 않다면 추가적인 SQL 확장 패턴을 적용해야 합니다.

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
