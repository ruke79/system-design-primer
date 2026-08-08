# 트위터 타임라인과 검색 설계하기

> 한국어 번역본입니다. 원문: [README.md](README.md) | 전체 문서: [README-ko.md](../../../README-ko.md)

*참고: 이 문서는 중복을 피하기 위해 [시스템 설계 주제](https://github.com/donnemartin/system-design-primer#index-of-system-design-topics)의 관련 항목으로 직접 링크합니다. 일반적인 논점, 트레이드오프, 대안은 링크된 내용을 참고하세요.*

**페이스북 피드 설계하기**와 **페이스북 검색 설계하기**도 비슷한 문제입니다.

## 1단계: 유스케이스와 제약 조건 정리하기

> 요구사항을 수집하고 문제 범위를 정합니다.
> 유스케이스와 제약 조건을 명확히 하기 위해 질문하세요.
> 가정에 대해 논의하세요.

명확히 답해줄 면접관이 없으므로, 여기서는 몇 가지 유스케이스와 제약 조건을 직접 정의하겠습니다.

### 유스케이스

#### 다음 유스케이스만 다루도록 문제 범위를 좁힙니다

* **사용자**가 트윗을 작성함
    * **서비스**가 팔로워에게 트윗을 푸시하고, 푸시 알림과 이메일을 전송함
* **사용자**가 사용자 타임라인(자신의 활동)을 조회함
* **사용자**가 홈 타임라인(팔로우하는 사람들의 활동)을 조회함
* **사용자**가 키워드를 검색함
* **서비스**는 고가용성을 가짐

#### 범위에서 제외

* **서비스**가 트위터 파이어호스와 다른 스트림으로 트윗을 푸시함
* **서비스**가 사용자의 공개 설정에 따라 트윗을 걸러냄
    * 답장 대상을 팔로우하지 않는 경우 @reply 숨기기
    * '리트윗 숨기기' 설정 준수
* 분석(analytics)

### 제약 조건과 가정

#### 가정 정리

일반

* 트래픽은 고르게 분포하지 않음
* 트윗 작성은 빨라야 함
    * 팔로워가 수백만 명이 아니라면 모든 팔로워에게 트윗을 팬아웃하는 것도 빨라야 함
* 활성 사용자 1억 명
* 하루 5억 트윗, 즉 월 150억 트윗
    * 트윗 1건당 평균 10건의 전달로 팬아웃됨
    * 하루에 팬아웃으로 전달되는 트윗 총 50억 건
    * 월 1,500억 건의 팬아웃 전달
* 월 2,500억 건의 읽기 요청
* 월 100억 건의 검색

타임라인

* 타임라인 조회는 빨라야 함
* 트위터는 쓰기보다 읽기가 많음
    * 트윗의 빠른 읽기에 최적화
* 트윗 수집(ingest)은 쓰기가 많음

검색

* 검색은 빨라야 함
* 검색은 읽기가 많음

#### 사용량 계산

**봉투 뒷면 계산을 해야 하는지 면접관에게 확인하세요.**

* 트윗당 크기:
    * `tweet_id` - 8바이트
    * `user_id` - 32바이트
    * `text` - 140바이트
    * `media` - 평균 10 KB
    * 합계: 약 10 KB
* 월 150 TB의 신규 트윗 콘텐츠
    * 트윗당 10 KB * 하루 5억 트윗 * 월 30일
    * 3년간 5.4 PB의 신규 트윗 콘텐츠
* 초당 10만 건의 읽기 요청
    * 월 2,500억 건의 읽기 요청 * (초당 400건 / 월 10억 건)
* 초당 6,000 트윗
    * 월 150억 트윗 * (초당 400건 / 월 10억 건)
* 초당 6만 건의 팬아웃 전달
    * 월 1,500억 건의 팬아웃 전달 * (초당 400건 / 월 10억 건)
* 초당 4,000건의 검색 요청
    * 월 100억 건의 검색 * (초당 400건 / 월 10억 건)

편리한 환산 가이드:

* 월 250만 초
* 초당 1건 = 월 250만 건
* 초당 40건 = 월 1억 건
* 초당 400건 = 월 10억 건

## 2단계: 상위 수준 설계 만들기

> 중요한 구성 요소를 모두 담아 상위 수준 설계를 그립니다.

![Imgur](http://i.imgur.com/48tEA2j.png)

## 3단계: 핵심 컴포넌트 설계하기

> 각 핵심 컴포넌트를 깊이 파고듭니다.

### 유스케이스: 사용자가 트윗을 작성한다

사용자 타임라인(자신의 활동)을 채우기 위한 본인 트윗은 [관계형 데이터베이스](https://github.com/donnemartin/system-design-primer#relational-database-management-system-rdbms)에 저장할 수 있습니다. [SQL과 NoSQL 선택의 유스케이스와 트레이드오프](https://github.com/donnemartin/system-design-primer#sql-or-nosql)를 논의해야 합니다.

트윗을 전달하고 홈 타임라인(팔로우하는 사람들의 활동)을 구성하는 것은 더 까다롭습니다. 모든 팔로워에게 트윗을 팬아웃하는 것(초당 6만 건의 팬아웃 전달)은 전통적인 [관계형 데이터베이스](https://github.com/donnemartin/system-design-primer#relational-database-management-system-rdbms)에 과부하를 일으킵니다. 아마도 **NoSQL 데이터베이스**나 **메모리 캐시**처럼 쓰기가 빠른 데이터 저장소를 선택해야 할 것입니다. 메모리에서 1 MB를 순차적으로 읽는 데 약 250마이크로초가 걸리는 반면, SSD에서는 4배, 디스크에서는 80배 더 오래 걸립니다.<sup><a href=https://github.com/donnemartin/system-design-primer#latency-numbers-every-programmer-should-know>1</a></sup>

사진이나 동영상 같은 미디어는 **오브젝트 스토어**에 저장할 수 있습니다.

* **클라이언트**가 [리버스 프록시](https://github.com/donnemartin/system-design-primer#reverse-proxy-web-server)로 동작하는 **웹 서버**에 트윗을 작성함
* **웹 서버**가 요청을 **쓰기 API** 서버로 전달함
* **쓰기 API**가 **SQL 데이터베이스**의 사용자 타임라인에 트윗을 저장함
* **쓰기 API**가 **팬아웃 서비스**에 연락하고, 팬아웃 서비스는 다음을 수행함
    * **사용자 그래프 서비스**에 질의해 **메모리 캐시**에 저장된 사용자의 팔로워를 찾음
    * *사용자의 팔로워들의 홈 타임라인*에 트윗을 **메모리 캐시**로 저장함
        * O(n) 연산: 팔로워 1,000명 = 조회와 삽입 1,000회
    * 빠른 검색을 위해 **검색 인덱스 서비스**에 트윗을 저장함
    * 미디어를 **오브젝트 스토어**에 저장함
    * **알림 서비스**를 이용해 팔로워에게 푸시 알림을 보냄
        * **큐**(그림에는 없음)를 사용해 비동기적으로 알림을 전송함

**코드를 얼마나 작성해야 하는지 면접관에게 확인하세요.**

**메모리 캐시**가 Redis라면 다음과 같은 구조의 네이티브 Redis 리스트를 사용할 수 있습니다.

```
           tweet n+2                   tweet n+1                   tweet n
| 8 bytes   8 bytes  1 byte | 8 bytes   8 bytes  1 byte | 8 bytes   8 bytes  1 byte |
| tweet_id  user_id  meta   | tweet_id  user_id  meta   | tweet_id  user_id  meta   |
```

새 트윗은 **메모리 캐시**에 저장되고, 이 캐시가 사용자의 홈 타임라인(팔로우하는 사람들의 활동)을 채웁니다.

공개 [**REST API**](https://github.com/donnemartin/system-design-primer#representational-state-transfer-rest)를 사용합니다.

```
$ curl -X POST --data '{ "user_id": "123", "auth_token": "ABC123", \
    "status": "hello world!", "media_ids": "ABC987" }' \
    https://twitter.com/api/v1/tweet
```

응답:

```
{
    "created_at": "Wed Sep 05 00:37:15 +0000 2012",
    "status": "hello world!",
    "tweet_id": "987",
    "user_id": "123",
    ...
}
```

내부 통신에는 [원격 프로시저 호출(RPC)](https://github.com/donnemartin/system-design-primer#remote-procedure-call-rpc)을 사용할 수 있습니다.

### 유스케이스: 사용자가 홈 타임라인을 조회한다

* **클라이언트**가 **웹 서버**에 홈 타임라인 요청을 보냄
* **웹 서버**가 요청을 **읽기 API** 서버로 전달함
* **읽기 API** 서버가 **타임라인 서비스**에 연락하고, 타임라인 서비스는 다음을 수행함
    * **메모리 캐시**에 저장된 타임라인 데이터(트윗 id와 사용자 id 포함)를 가져옴 - O(1)
    * **트윗 정보 서비스**에 [multiget](http://redis.io/commands/mget)으로 질의해 트윗 id에 대한 추가 정보를 얻음 - O(n)
    * **사용자 정보 서비스**에 multiget으로 질의해 사용자 id에 대한 추가 정보를 얻음 - O(n)

REST API:

```
$ curl https://twitter.com/api/v1/home_timeline?user_id=123
```

응답:

```
{
    "user_id": "456",
    "tweet_id": "123",
    "status": "foo"
},
{
    "user_id": "789",
    "tweet_id": "456",
    "status": "bar"
},
{
    "user_id": "789",
    "tweet_id": "579",
    "status": "baz"
},
```

### 유스케이스: 사용자가 사용자 타임라인을 조회한다

* **클라이언트**가 **웹 서버**에 사용자 타임라인 요청을 보냄
* **웹 서버**가 요청을 **읽기 API** 서버로 전달함
* **읽기 API**가 **SQL 데이터베이스**에서 사용자 타임라인을 가져옴

REST API는 홈 타임라인과 비슷하지만, 팔로우하는 사람들이 아니라 해당 사용자 본인의 트윗만 반환한다는 점이 다릅니다.

### 유스케이스: 사용자가 키워드를 검색한다

* **클라이언트**가 **웹 서버**에 검색 요청을 보냄
* **웹 서버**가 요청을 **검색 API** 서버로 전달함
* **검색 API**가 **검색 서비스**에 연락하고, 검색 서비스는 다음을 수행함
    * 입력 질의를 파싱/토큰화하여 무엇을 검색해야 할지 판단함
        * 마크업 제거
        * 텍스트를 용어(term)로 분리
        * 오타 교정
        * 대소문자 정규화
        * 질의를 불리언 연산으로 변환
    * 결과를 얻기 위해 **검색 클러스터**(예: [Lucene](https://lucene.apache.org/))에 질의함
        * 클러스터의 각 서버에 [스캐터 게더](https://github.com/donnemartin/system-design-primer#under-development)를 수행해 질의 결과가 있는지 확인
        * 결과를 병합·랭킹·정렬하여 반환

REST API:

```
$ curl https://twitter.com/api/v1/search?query=hello+world
```

응답은 홈 타임라인과 비슷하지만, 주어진 질의에 일치하는 트윗을 반환한다는 점이 다릅니다.

## 4단계: 설계 확장하기

> 제약 조건을 고려해 병목을 찾아내고 해결합니다.

![Imgur](http://i.imgur.com/jrUBAF7.png)

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

**팬아웃 서비스**는 잠재적인 병목입니다. 팔로워가 수백만 명인 트위터 사용자는 트윗이 팬아웃 과정을 통과하는 데 수 분이 걸릴 수 있습니다. 이는 해당 트윗에 대한 @reply와의 경쟁 상태(race condition)로 이어질 수 있는데, 응답 시점에 트윗 순서를 재정렬하여 완화할 수 있습니다.

팔로워가 매우 많은 사용자의 트윗은 팬아웃하지 않는 방법도 있습니다. 대신 팔로워가 많은 사용자의 트윗은 검색으로 찾아, 그 검색 결과를 사용자의 홈 타임라인 결과와 병합한 뒤 응답 시점에 순서를 재정렬합니다.

추가적인 최적화:

* **메모리 캐시**에는 홈 타임라인마다 수백 개의 트윗만 유지
* **메모리 캐시**에는 활성 사용자의 홈 타임라인 정보만 유지
    * 지난 30일간 활동이 없던 사용자라면 **SQL 데이터베이스**에서 타임라인을 재구성할 수 있음
        * **사용자 그래프 서비스**에 질의해 그 사용자가 누구를 팔로우하는지 확인
        * **SQL 데이터베이스**에서 트윗을 가져와 **메모리 캐시**에 추가
* **트윗 정보 서비스**에는 한 달치 트윗만 저장
* **사용자 정보 서비스**에는 활성 사용자만 저장
* **검색 클러스터**는 지연을 낮게 유지하기 위해 트윗을 메모리에 두어야 할 가능성이 큼

**SQL 데이터베이스**의 병목도 해결해야 합니다.

**메모리 캐시**가 데이터베이스 부하를 줄여주긴 하지만, **SQL 읽기 복제본**만으로 캐시 미스를 감당하기는 어려울 것입니다. 추가적인 SQL 확장 패턴을 적용해야 할 가능성이 높습니다.

쓰기 양이 많아 단일 **SQL 쓰기 마스터-슬레이브**로는 감당하기 어려우므로, 이 역시 추가적인 확장 기법이 필요함을 시사합니다.

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
