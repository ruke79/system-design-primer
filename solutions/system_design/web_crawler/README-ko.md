# 웹 크롤러 설계하기

> 한국어 번역본입니다. 원문: [README.md](README.md) | 전체 문서: [README-ko.md](../../../README-ko.md)

*참고: 이 문서는 중복을 피하기 위해 [시스템 설계 주제](https://github.com/donnemartin/system-design-primer#index-of-system-design-topics)의 관련 항목으로 직접 링크합니다. 일반적인 논점, 트레이드오프, 대안은 링크된 내용을 참고하세요.*

## 1단계: 유스케이스와 제약 조건 정리하기

> 요구사항을 수집하고 문제 범위를 정합니다.
> 유스케이스와 제약 조건을 명확히 하기 위해 질문하세요.
> 가정에 대해 논의하세요.

명확히 답해줄 면접관이 없으므로, 여기서는 몇 가지 유스케이스와 제약 조건을 직접 정의하겠습니다.

### 유스케이스

#### 다음 유스케이스만 다루도록 문제 범위를 좁힙니다

* **서비스**가 URL 목록을 크롤링함
    * 검색어를 포함한 페이지로 이어지는 단어의 역색인(reverse index)을 생성함
    * 페이지의 제목과 스니펫을 생성함
        * 제목과 스니펫은 정적이며, 검색 질의에 따라 달라지지 않음
* **사용자**가 검색어를 입력하면 크롤러가 생성한 제목과 스니펫이 포함된 관련 페이지 목록을 봄
    * 이 유스케이스는 상위 수준 컴포넌트와 상호작용만 스케치하며, 깊이 들어갈 필요는 없음
* **서비스**는 고가용성을 가짐

#### 범위에서 제외

* 검색 분석(analytics)
* 개인화된 검색 결과
* 페이지 랭크

### 제약 조건과 가정

#### 가정 정리

* 트래픽은 고르게 분포하지 않음
    * 어떤 검색은 매우 인기 있지만, 어떤 검색은 단 한 번만 실행됨
* 익명 사용자만 지원
* 검색 결과 생성은 빨라야 함
* 웹 크롤러가 무한 루프에 빠지면 안 됨
    * 그래프에 사이클이 있으면 무한 루프에 빠짐
* 크롤링할 링크 10억 개
    * 최신성을 보장하려면 페이지를 주기적으로 크롤링해야 함
    * 평균 갱신 주기는 주 1회 정도이며, 인기 사이트는 더 자주
        * 월 40억 개 링크 크롤링
    * 웹 페이지당 평균 저장 크기: 500 KB
        * 단순화를 위해 변경도 신규 페이지와 동일하게 계산
* 월 1,000억 건의 검색

기존 시스템인 [solr](http://lucene.apache.org/solr/)나 [nutch](http://nutch.apache.org/)를 쓰지 말고, 더 전통적인 시스템을 직접 다루는 연습을 하세요.

#### 사용량 계산

**봉투 뒷면 계산을 해야 하는지 면접관에게 확인하세요.**

* 월 2 PB의 페이지 콘텐츠 저장
    * 페이지당 500 KB * 월 40억 개 링크 크롤링
    * 3년간 72 PB의 페이지 콘텐츠 저장
* 초당 1,600건의 쓰기 요청
* 초당 40,000건의 검색 요청

편리한 환산 가이드:

* 월 250만 초
* 초당 1건 = 월 250만 건
* 초당 40건 = 월 1억 건
* 초당 400건 = 월 10억 건

## 2단계: 상위 수준 설계 만들기

> 중요한 구성 요소를 모두 담아 상위 수준 설계를 그립니다.

![Imgur](http://i.imgur.com/xjdAAUv.png)

## 3단계: 핵심 컴포넌트 설계하기

> 각 핵심 컴포넌트를 깊이 파고듭니다.

### 유스케이스: 서비스가 URL 목록을 크롤링한다

전체 사이트 인기도를 기준으로 순위가 매겨진 초기 `links_to_crawl` 목록이 있다고 가정합니다. 이 가정이 합리적이지 않다면, [Yahoo](https://www.yahoo.com/)나 [DMOZ](http://www.dmoz.org/)처럼 외부 콘텐츠로 링크하는 인기 사이트로 크롤러를 시드(seed)할 수 있습니다.

처리된 링크와 그 페이지 시그니처를 저장하기 위해 `crawled_links` 테이블을 사용합니다.

`links_to_crawl`과 `crawled_links`는 키-값 **NoSQL 데이터베이스**에 저장할 수 있습니다. `links_to_crawl`의 순위가 매겨진 링크에는 정렬 집합(sorted set)을 지원하는 [Redis](https://redis.io/)를 사용해 페이지 링크의 순위를 관리할 수 있습니다. [SQL과 NoSQL 선택의 유스케이스와 트레이드오프](https://github.com/donnemartin/system-design-primer#sql-or-nosql)를 논의해야 합니다.

* **크롤러 서비스**는 루프를 돌며 각 페이지 링크를 다음과 같이 처리함
    * 크롤링할 최상위 순위 페이지 링크를 가져옴
        * **NoSQL 데이터베이스**의 `crawled_links`에서 비슷한 페이지 시그니처를 가진 항목이 있는지 확인
            * 비슷한 페이지가 있으면 해당 페이지 링크의 우선순위를 낮춤
                * 이렇게 하면 사이클에 빠지는 것을 방지함
                * 계속 진행
            * 없으면 링크를 크롤링함
                * [역색인](https://en.wikipedia.org/wiki/Search_engine_indexing)을 생성하도록 **역색인 서비스** 큐에 작업을 추가
                * 정적 제목과 스니펫을 생성하도록 **문서 서비스** 큐에 작업을 추가
                * 페이지 시그니처를 생성
                * **NoSQL 데이터베이스**의 `links_to_crawl`에서 해당 링크를 제거
                * **NoSQL 데이터베이스**의 `crawled_links`에 페이지 링크와 시그니처를 삽입

**코드를 얼마나 작성해야 하는지 면접관에게 확인하세요.**

`PagesDataStore`는 **크롤러 서비스** 내부에서 **NoSQL 데이터베이스**를 사용하는 추상화입니다.

```python
class PagesDataStore(object):

    def __init__(self, db);
        self.db = db
        ...

    def add_link_to_crawl(self, url):
        """Add the given link to `links_to_crawl`."""
        ...

    def remove_link_to_crawl(self, url):
        """Remove the given link from `links_to_crawl`."""
        ...

    def reduce_priority_link_to_crawl(self, url)
        """Reduce the priority of a link in `links_to_crawl` to avoid cycles."""
        ...

    def extract_max_priority_page(self):
        """Return the highest priority link in `links_to_crawl`."""
        ...

    def insert_crawled_link(self, url, signature):
        """Add the given link to `crawled_links`."""
        ...

    def crawled_similar(self, signature):
        """Determine if we've already crawled a page matching the given signature"""
        ...
```

`Page`는 **크롤러 서비스** 내부에서 페이지와 그 콘텐츠, 자식 URL, 시그니처를 캡슐화하는 추상화입니다.

```python
class Page(object):

    def __init__(self, url, contents, child_urls, signature):
        self.url = url
        self.contents = contents
        self.child_urls = child_urls
        self.signature = signature
```

`Crawler`는 **크롤러 서비스**의 주요 클래스이며 `Page`와 `PagesDataStore`로 구성됩니다.

```python
class Crawler(object):

    def __init__(self, data_store, reverse_index_queue, doc_index_queue):
        self.data_store = data_store
        self.reverse_index_queue = reverse_index_queue
        self.doc_index_queue = doc_index_queue

    def create_signature(self, page):
        """Create signature based on url and contents."""
        ...

    def crawl_page(self, page):
        for url in page.child_urls:
            self.data_store.add_link_to_crawl(url)
        page.signature = self.create_signature(page)
        self.data_store.remove_link_to_crawl(page.url)
        self.data_store.insert_crawled_link(page.url, page.signature)

    def crawl(self):
        while True:
            page = self.data_store.extract_max_priority_page()
            if page is None:
                break
            if self.data_store.crawled_similar(page.signature):
                self.data_store.reduce_priority_link_to_crawl(page.url)
            else:
                self.crawl_page(page)
```

### 중복 처리하기

웹 크롤러가 무한 루프에 빠지지 않도록 주의해야 합니다. 무한 루프는 그래프에 사이클이 있을 때 발생합니다.

**코드를 얼마나 작성해야 하는지 면접관에게 확인하세요.**

중복 URL을 제거해야 합니다.

* 목록이 작다면 `sort | unique` 같은 방법을 쓸 수 있습니다.
* 크롤링할 링크가 10억 개라면, **MapReduce**를 사용해 빈도가 1인 항목만 출력할 수 있습니다.

```python
class RemoveDuplicateUrls(MRJob):

    def mapper(self, _, line):
        yield line, 1

    def reducer(self, key, values):
        total = sum(values)
        if total == 1:
            yield key, total
```

중복 콘텐츠를 탐지하는 것은 더 복잡합니다. 페이지 콘텐츠를 기반으로 시그니처를 생성한 뒤 두 시그니처의 유사도를 비교할 수 있습니다. 사용할 만한 알고리즘으로 [자카드 지수](https://en.wikipedia.org/wiki/Jaccard_index)와 [코사인 유사도](https://en.wikipedia.org/wiki/Cosine_similarity)가 있습니다.

### 크롤링 결과를 언제 갱신할지 결정하기

최신성을 보장하려면 페이지를 주기적으로 크롤링해야 합니다. 크롤링 결과에 페이지를 마지막으로 크롤링한 시각을 나타내는 `timestamp` 필드를 둘 수 있습니다. 기본 기간(예: 일주일)이 지나면 모든 페이지를 갱신합니다. 자주 갱신되거나 인기 있는 사이트는 더 짧은 주기로 갱신할 수 있습니다.

분석까지 깊이 다루지는 않겠지만, 데이터 마이닝을 통해 특정 페이지가 갱신되기까지의 평균 시간을 파악하고, 그 통계를 이용해 재크롤링 주기를 결정할 수 있습니다.

웹마스터가 크롤링 빈도를 제어할 수 있도록 `Robots.txt` 파일을 지원하는 것도 선택할 수 있습니다.

### 유스케이스: 사용자가 검색어를 입력하면 제목과 스니펫이 포함된 관련 페이지 목록을 본다

* **클라이언트**가 [리버스 프록시](https://github.com/donnemartin/system-design-primer#reverse-proxy-web-server)로 동작하는 **웹 서버**에 요청을 보냄
* **웹 서버**가 요청을 **질의 API** 서버로 전달함
* **질의 API** 서버는 다음을 수행함
    * 질의를 파싱함
        * 마크업 제거
        * 텍스트를 용어(term)로 분리
        * 오타 교정
        * 대소문자 정규화
        * 질의를 불리언 연산으로 변환
    * **역색인 서비스**를 사용해 질의에 일치하는 문서를 찾음
        * **역색인 서비스**는 일치하는 결과에 순위를 매기고 상위 항목을 반환함
    * **문서 서비스**를 사용해 제목과 스니펫을 반환함

공개 [**REST API**](https://github.com/donnemartin/system-design-primer#representational-state-transfer-rest)를 사용합니다.

```
$ curl https://search.com/api/v1/search?query=hello+world
```

응답:

```
{
    "title": "foo's title",
    "snippet": "foo's snippet",
    "link": "https://foo.com",
},
{
    "title": "bar's title",
    "snippet": "bar's snippet",
    "link": "https://bar.com",
},
{
    "title": "baz's title",
    "snippet": "baz's snippet",
    "link": "https://baz.com",
},
```

내부 통신에는 [원격 프로시저 호출(RPC)](https://github.com/donnemartin/system-design-primer#remote-procedure-call-rpc)을 사용할 수 있습니다.

## 4단계: 설계 확장하기

> 제약 조건을 고려해 병목을 찾아내고 해결합니다.

![Imgur](http://i.imgur.com/bWxPtQA.png)

**중요: 초기 설계에서 최종 설계로 곧장 건너뛰지 마세요!**

다음을 수행하겠다고 말하세요. 1) **벤치마크/부하 테스트**, 2) 병목 **프로파일링**, 3) 대안과 트레이드오프를 평가하며 병목 해결, 4) 반복. 초기 설계를 반복적으로 확장하는 예시는 [AWS에서 수백만 사용자까지 확장되는 시스템 설계하기](../scaling_aws/README-ko.md)를 참고하세요.

초기 설계에서 어떤 병목을 만날 수 있고 각각을 어떻게 해결할지 논의하는 것이 중요합니다. 예를 들어 여러 대의 **웹 서버**와 함께 **로드 밸런서**를 추가하면 어떤 문제가 해결될까요? **CDN**은요? **마스터-슬레이브 복제본**은요? 각각의 대안과 **트레이드오프**는 무엇일까요?

설계를 완성하고 확장성 문제를 해결하기 위해 몇 가지 컴포넌트를 추가합니다. 복잡해지지 않도록 내부 로드 밸런서는 표시하지 않았습니다.

*논의 중복을 피하기 위해*, 주요 논점·트레이드오프·대안은 다음 [시스템 설계 주제](https://github.com/donnemartin/system-design-primer#index-of-system-design-topics)를 참고하세요.

* [DNS](https://github.com/donnemartin/system-design-primer#domain-name-system)
* [로드 밸런서](https://github.com/donnemartin/system-design-primer#load-balancer)
* [수평 확장](https://github.com/donnemartin/system-design-primer#horizontal-scaling)
* [웹 서버(리버스 프록시)](https://github.com/donnemartin/system-design-primer#reverse-proxy-web-server)
* [API 서버(애플리케이션 계층)](https://github.com/donnemartin/system-design-primer#application-layer)
* [캐시](https://github.com/donnemartin/system-design-primer#cache)
* [NoSQL](https://github.com/donnemartin/system-design-primer#nosql)
* [일관성 패턴](https://github.com/donnemartin/system-design-primer#consistency-patterns)
* [가용성 패턴](https://github.com/donnemartin/system-design-primer#availability-patterns)

어떤 검색은 매우 인기 있지만 어떤 검색은 단 한 번만 실행됩니다. 인기 있는 질의는 Redis나 Memcached 같은 **메모리 캐시**에서 처리하여 응답 시간을 줄이고 **역색인 서비스**와 **문서 서비스**의 과부하를 막을 수 있습니다. **메모리 캐시**는 고르지 않게 분포한 트래픽과 트래픽 급증을 처리하는 데도 유용합니다. 메모리에서 1 MB를 순차적으로 읽는 데 약 250마이크로초가 걸리는 반면, SSD에서는 4배, 디스크에서는 80배 더 오래 걸립니다.<sup><a href=https://github.com/donnemartin/system-design-primer#latency-numbers-every-programmer-should-know>1</a></sup>

**크롤링 서비스**에 대한 다른 최적화는 다음과 같습니다.

* 데이터 크기와 요청 부하를 감당하려면 **역색인 서비스**와 **문서 서비스**는 샤딩과 페더레이션을 적극적으로 활용해야 할 것입니다.
* DNS 조회가 병목이 될 수 있으므로, **크롤러 서비스**는 주기적으로 갱신되는 자체 DNS 조회 정보를 유지할 수 있습니다.
* **크롤러 서비스**는 여러 연결을 동시에 열어 두는 [커넥션 풀링](https://en.wikipedia.org/wiki/Connection_pool)으로 성능을 높이고 메모리 사용량을 줄일 수 있습니다.
    * [UDP](https://github.com/donnemartin/system-design-primer#user-datagram-protocol-udp)로 전환하는 것도 성능 향상에 도움이 될 수 있습니다.
* 웹 크롤링은 대역폭을 많이 사용하므로, 높은 처리량을 유지할 수 있는 충분한 대역폭을 확보하세요.

## 추가로 논의할 만한 주제

> 문제 범위와 남은 시간에 따라 더 깊이 파고들 수 있는 주제입니다.

### SQL 확장 패턴

* [읽기 복제본](https://github.com/donnemartin/system-design-primer#master-slave-replication)
* [페더레이션](https://github.com/donnemartin/system-design-primer#federation)
* [샤딩](https://github.com/donnemartin/system-design-primer#sharding)
* [비정규화](https://github.com/donnemartin/system-design-primer#denormalization)
* [SQL 튜닝](https://github.com/donnemartin/system-design-primer#sql-tuning)

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
