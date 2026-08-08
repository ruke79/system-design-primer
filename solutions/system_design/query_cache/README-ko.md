# 최근 웹 서버 질의 결과를 저장하는 키-값 캐시 설계하기

> 한국어 번역본입니다. 원문: [README.md](README.md) | 전체 문서: [README-ko.md](../../../README-ko.md)

*참고: 이 문서는 중복을 피하기 위해 [시스템 설계 주제](https://github.com/donnemartin/system-design-primer#index-of-system-design-topics)의 관련 항목으로 직접 링크합니다. 일반적인 논점, 트레이드오프, 대안은 링크된 내용을 참고하세요.*

## 1단계: 유스케이스와 제약 조건 정리하기

> 요구사항을 수집하고 문제 범위를 정합니다.
> 유스케이스와 제약 조건을 명확히 하기 위해 질문하세요.
> 가정에 대해 논의하세요.

명확히 답해줄 면접관이 없으므로, 여기서는 몇 가지 유스케이스와 제약 조건을 직접 정의하겠습니다.

### 유스케이스

#### 다음 유스케이스만 다루도록 문제 범위를 좁힙니다

* **사용자**가 검색 요청을 보내 캐시 히트가 발생함
* **사용자**가 검색 요청을 보내 캐시 미스가 발생함
* **서비스**는 고가용성을 가짐

### 제약 조건과 가정

#### 가정 정리

* 트래픽은 고르게 분포하지 않음
    * 인기 있는 질의는 거의 항상 캐시에 있어야 함
    * 만료/갱신 방법을 결정해야 함
* 캐시에서 응답하려면 조회가 빨라야 함
* 머신 간 지연이 낮음
* 캐시의 메모리가 제한적임
    * 무엇을 유지하고 무엇을 제거할지 결정해야 함
    * 수백만 건의 질의를 캐시해야 함
* 사용자 1,000만 명
* 월 100억 건의 질의

#### 사용량 계산

**봉투 뒷면 계산을 해야 하는지 면접관에게 확인하세요.**

* 캐시는 키: 질의, 값: 결과의 순서 있는 목록을 저장함
    * `query` - 50바이트
    * `title` - 20바이트
    * `snippet` - 200바이트
    * 합계: 270바이트
* 100억 건의 질의가 모두 고유하고 모두 저장된다면 월 2.7 TB의 캐시 데이터
    * 검색당 270바이트 * 월 100억 건의 검색
    * 가정상 메모리가 제한적이므로 내용을 어떻게 만료시킬지 결정해야 함
* 초당 4,000건의 요청

편리한 환산 가이드:

* 월 250만 초
* 초당 1건 = 월 250만 건
* 초당 40건 = 월 1억 건
* 초당 400건 = 월 10억 건

## 2단계: 상위 수준 설계 만들기

> 중요한 구성 요소를 모두 담아 상위 수준 설계를 그립니다.

![Imgur](http://i.imgur.com/KqZ3dSx.png)

## 3단계: 핵심 컴포넌트 설계하기

> 각 핵심 컴포넌트를 깊이 파고듭니다.

### 유스케이스: 사용자가 요청을 보내 캐시 히트가 발생한다

인기 있는 질의는 Redis나 Memcached 같은 **메모리 캐시**에서 처리하여 읽기 지연을 줄이고 **역색인 서비스**와 **문서 서비스**의 과부하를 막을 수 있습니다. 메모리에서 1 MB를 순차적으로 읽는 데 약 250마이크로초가 걸리는 반면, SSD에서는 4배, 디스크에서는 80배 더 오래 걸립니다.<sup><a href=https://github.com/donnemartin/system-design-primer#latency-numbers-every-programmer-should-know>1</a></sup>

캐시 용량이 제한적이므로, 오래된 항목을 만료시키기 위해 LRU(least recently used) 방식을 사용합니다.

* **클라이언트**가 [리버스 프록시](https://github.com/donnemartin/system-design-primer#reverse-proxy-web-server)로 동작하는 **웹 서버**에 요청을 보냄
* **웹 서버**가 요청을 **질의 API** 서버로 전달함
* **질의 API** 서버는 다음을 수행함
    * 질의를 파싱함
        * 마크업 제거
        * 텍스트를 용어(term)로 분리
        * 오타 교정
        * 대소문자 정규화
        * 질의를 불리언 연산으로 변환
    * **메모리 캐시**에서 질의에 일치하는 콘텐츠를 확인함
        * **메모리 캐시**에 히트가 있으면 **메모리 캐시**는 다음을 수행함
            * 캐시된 항목의 위치를 LRU 목록의 맨 앞으로 갱신
            * 캐시된 내용을 반환
        * 없으면 **질의 API**가 다음을 수행함
            * **역색인 서비스**를 사용해 질의에 일치하는 문서를 찾음
                * **역색인 서비스**는 일치하는 결과에 순위를 매기고 상위 항목을 반환함
            * **문서 서비스**를 사용해 제목과 스니펫을 반환함
            * 해당 내용으로 **메모리 캐시**를 갱신하고, 항목을 LRU 목록의 맨 앞에 배치함

#### 캐시 구현

캐시는 이중 연결 리스트를 사용할 수 있습니다. 새 항목은 head에 추가되고, 만료될 항목은 tail에서 제거됩니다. 각 연결 리스트 노드에 빠르게 접근하기 위해 해시 테이블을 사용합니다.

**코드를 얼마나 작성해야 하는지 면접관에게 확인하세요.**

**질의 API 서버** 구현:

```python
class QueryApi(object):

    def __init__(self, memory_cache, reverse_index_service):
        self.memory_cache = memory_cache
        self.reverse_index_service = reverse_index_service

    def parse_query(self, query):
        """Remove markup, break text into terms, deal with typos,
        normalize capitalization, convert to use boolean operations.
        """
        ...

    def process_query(self, query):
        query = self.parse_query(query)
        results = self.memory_cache.get(query)
        if results is None:
            results = self.reverse_index_service.process_search(query)
            self.memory_cache.set(query, results)
        return results
```

**Node** 구현:

```python
class Node(object):

    def __init__(self, query, results):
        self.query = query
        self.results = results
```

**LinkedList** 구현:

```python
class LinkedList(object):

    def __init__(self):
        self.head = None
        self.tail = None

    def move_to_front(self, node):
        ...

    def append_to_front(self, node):
        ...

    def remove_from_tail(self):
        ...
```

**Cache** 구현:

```python
class Cache(object):

    def __init__(self, MAX_SIZE):
        self.MAX_SIZE = MAX_SIZE
        self.size = 0
        self.lookup = {}  # key: query, value: node
        self.linked_list = LinkedList()

    def get(self, query)
        """Get the stored query result from the cache.

        Accessing a node updates its position to the front of the LRU list.
        """
        node = self.lookup[query]
        if node is None:
            return None
        self.linked_list.move_to_front(node)
        return node.results

    def set(self, results, query):
        """Set the result for the given query key in the cache.

        When updating an entry, updates its position to the front of the LRU list.
        If the entry is new and the cache is at capacity, removes the oldest entry
        before the new entry is added.
        """
        node = self.lookup[query]
        if node is not None:
            # Key exists in cache, update the value
            node.results = results
            self.linked_list.move_to_front(node)
        else:
            # Key does not exist in cache
            if self.size == self.MAX_SIZE:
                # Remove the oldest entry from the linked list and lookup
                self.lookup.pop(self.linked_list.tail.query, None)
                self.linked_list.remove_from_tail()
            else:
                self.size += 1
            # Add the new key and value
            new_node = Node(query, results)
            self.linked_list.append_to_front(new_node)
            self.lookup[query] = new_node
```

#### 캐시를 언제 갱신할 것인가

다음의 경우 캐시를 갱신해야 합니다.

* 페이지 내용이 변경될 때
* 페이지가 제거되거나 새 페이지가 추가될 때
* 페이지 랭크가 변경될 때

이런 경우를 처리하는 가장 단순한 방법은, 캐시된 항목이 갱신되기 전까지 캐시에 머무를 수 있는 최대 시간을 설정하는 것입니다. 보통 TTL(time to live)이라고 부릅니다.

트레이드오프와 대안은 [캐시를 갱신하는 시점](https://github.com/donnemartin/system-design-primer#when-to-update-the-cache)을 참고하세요. 위에서 설명한 방식은 [캐시 어사이드](https://github.com/donnemartin/system-design-primer#cache-aside)입니다.

## 4단계: 설계 확장하기

> 제약 조건을 고려해 병목을 찾아내고 해결합니다.

![Imgur](http://i.imgur.com/4j99mhe.png)

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
* [일관성 패턴](https://github.com/donnemartin/system-design-primer#consistency-patterns)
* [가용성 패턴](https://github.com/donnemartin/system-design-primer#availability-patterns)

### 메모리 캐시를 여러 머신으로 확장하기

많은 요청 부하와 대량의 메모리 요구를 감당하기 위해 수평으로 확장합니다. **메모리 캐시** 클러스터에 데이터를 저장하는 방법은 크게 세 가지입니다.

* **캐시 클러스터의 각 머신이 자기만의 캐시를 가짐** - 단순하지만 캐시 적중률이 낮아질 가능성이 큽니다.
* **캐시 클러스터의 각 머신이 캐시의 복사본을 가짐** - 단순하지만 메모리를 비효율적으로 사용합니다.
* **캐시를 클러스터의 모든 머신에 [샤딩](https://github.com/donnemartin/system-design-primer#sharding)함** - 더 복잡하지만 아마도 가장 좋은 선택입니다. `machine = hash(query)`처럼 해싱을 사용해 어떤 머신이 해당 질의의 캐시 결과를 가지고 있을지 결정할 수 있습니다. [일관된 해싱](https://github.com/donnemartin/system-design-primer#under-development)을 사용하는 것이 좋습니다.

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
