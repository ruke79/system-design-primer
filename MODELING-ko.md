# 모델링 정보 위치 안내 (한국어)

> 이 저장소에서 **모델링(데이터 모델 / 클래스 모델 / 아키텍처 다이어그램)** 정보가 어디에 있는지 정리한 문서입니다.
> 본문 번역은 [README-ko.md](README-ko.md), 원문은 [README.md](README.md)를 참고하세요.
>
> 아래 표기하는 `solutions/system_design/*/README.md` 경로에는 같은 위치에 한국어 번역본 `README-ko.md`가 함께 있습니다.
> 줄 번호는 원문과 번역본이 1:1로 대응하므로 어느 쪽을 봐도 동일합니다.

이 저장소의 모델링 정보는 크게 **4가지 종류**로, 서로 다른 위치에 흩어져 있습니다.

| 종류 | 위치 | 파일 형식 |
|---|---|---|
| ① 모델링 **이론/원칙** | `README.md` (번역: `README-ko.md`) | Markdown |
| ② **데이터 모델**(스키마, 로그 포맷, 캐시 레이아웃) | `solutions/system_design/*/README.md` | Markdown 내 코드 블록 |
| ③ **클래스 모델**(객체지향 설계) | `solutions/object_oriented_design/*/` | `.py` + `.ipynb` |
| ④ **아키텍처 다이어그램**(편집 가능한 원본) | `solutions/system_design/*/*.graffle`, `*.png` | OmniGraffle / PNG |

---

## ① 모델링 이론이 있는 곳 — `README.md`

데이터 모델링을 어떻게 할지 결정하는 데 필요한 **원칙**은 본문의 다음 섹션에 있습니다.
(괄호 안은 원문 `README.md` 기준 줄 번호)

| 주제 | 위치 |
|---|---|
| 관계형 DB / ACID | `README.md:816` — [관계형 데이터베이스 관리 시스템(RDBMS)](README-ko.md#관계형-데이터베이스-관리-시스템rdbms) |
| 마스터-슬레이브 / 마스터-마스터 복제 | `README.md:829`, `README.md:844` |
| 페더레이션(기능적 분할) | `README.md:874` |
| 샤딩(수평 분할) | `README.md:895` |
| **비정규화** — 읽기 성능을 위한 모델링 | `README.md:923` |
| **SQL 튜닝** — 스키마 설계, 인덱스 설계 | `README.md:941` |
| NoSQL 4종 모델 추상화 | `README.md:991` |
| ├ 키-값 저장소 (추상화: 해시 테이블) | `README.md:1003` |
| ├ 문서 저장소 (추상화: 값이 문서인 키-값 저장소) | `README.md:1020` |
| ├ 와이드 칼럼 저장소 (추상화: 중첩 맵) | `README.md:1039` |
| └ 그래프 DB (추상화: 그래프) | `README.md:1062` |
| SQL이냐 NoSQL이냐 — 모델 선택 기준 | `README.md:1090` |

특히 **`#### 스키마를 조밀하게 만들기`**(`README.md:952`)와 **`##### 좋은 인덱스 사용하기`**(`README.md:964`)가
실제 테이블 모델링 시 바로 적용할 수 있는 체크리스트입니다.

---

## ② 데이터 모델(스키마)이 있는 곳 — 시스템 설계 해답

각 해답 문서의 **`Step 3: Design core components`** 절 안에 구체적인 스키마/데이터 구조가 코드 블록으로 들어 있습니다.

### Pastebin — 관계형 테이블 스키마 (가장 전형적인 DDL 예시)

* **위치**: `solutions/system_design/pastebin/README.md:109-117`
* `pastes` 테이블: `shortlink`(PK, char(7)), `expiration_length_in_minutes`, `created_at`, `paste_path`
* 이어지는 `README.md:119`에 **PK가 곧 유니크 인덱스가 되는 이유**와 `created_at` 보조 인덱스 근거 설명
* 분석용 집계 모델은 `README.md:191` "Service tracks analytics of pages" 절, MapReduce 코드는 `pastebin/pastebin.py`

### Twitter — 캐시(Redis) 메모리 레이아웃 모델

* **위치**: `solutions/system_design/twitter/README.md:126-131`
* 홈 타임라인을 Redis 리스트로 표현: 트윗 1건당 `tweet_id`(8B) + `user_id`(8B) + `meta`(1B) = 17바이트
* 관계형 DB(사용자 타임라인) vs NoSQL/메모리 캐시(홈 타임라인) 분리 근거는 `twitter/README.md:104-107`

### Mint — 도메인 클래스 모델 + 카테고리 분류 모델

* **위치**: `solutions/system_design/mint/README.md:186-250`
* `DefaultCategories(Enum)`, `Categorizer`, `Transaction`, `Budget` 클래스 정의
* 동일 코드 원본: `solutions/system_design/mint/mint_snippets.py`
* 집계(카테고리별 지출) MapReduce 모델: `mint/README.md:277`, 원본 `mint/mint_mapreduce.py`

### Social Graph — 그래프 자료구조 모델 (모델링 관점에서 가장 밀도 높음)

* **위치**: `solutions/system_design/social_graph/README.md:66-200`
* `Graph`(최단 경로 탐색), `Person`, `LookupService`, `PersonServer`, `UserGraphService` 클래스
* 샤딩된 그래프에서 사람↔서버 매핑을 어떻게 모델링하는지가 핵심
* 동일 코드 원본: `solutions/system_design/social_graph/social_graph_snippets.py`

### Sales Rank — 로그 스키마 + 집계 테이블 모델

* **위치**: `solutions/system_design/sales_rank/README.md:87-96`
* 탭 구분 로그 레코드 스키마: `timestamp, product_id, category_id, qty, total_price, seller_id, buyer_id`
* 집계 결과 테이블 `sales_rank` 및 다단계 MapReduce 모델: `sales_rank/README.md:100-200`
* 원본 코드: `solutions/system_design/sales_rank/sales_rank_mapreduce.py`

### Query Cache — LRU 캐시 자료구조 모델

* **위치**: `solutions/system_design/query_cache/README.md:92-210`
* `QueryApi`, `Node`, `LinkedList`, `Cache` — 해시맵 + 이중 연결 리스트 조합 모델
* 캐시 갱신 시점 모델: `query_cache/README.md:199`
* 원본 코드: `solutions/system_design/query_cache/query_cache_snippets.py`

### Web Crawler — 문서/링크 저장 모델

* **위치**: `solutions/system_design/web_crawler/README.md:104-210`
* `PagesDataStore`, `Page`, `Crawler` 클래스 + 중복 URL 제거(`RemoveDuplicateUrls`) 모델
* 원본 코드: `web_crawler/web_crawler_snippets.py`, `web_crawler/web_crawler_mapreduce.py`

### Scaling AWS — 스키마가 아닌 **배포 토폴로지 모델**

* **위치**: `solutions/system_design/scaling_aws/README.md:101-110` ("Start with SQL, consider NoSQL")
* 사용자 규모별 단계(`Users+` ~ `Users+++++`)에 따른 구성 변화가 `README.md:140-340`에 단계별로 기술
* 각 단계 다이어그램: `scaling_aws/scaling_aws_1.png` ~ `scaling_aws_7.png`

---

## ③ 클래스 모델(객체지향 설계)이 있는 곳

`solutions/object_oriented_design/` 아래 6개 디렉터리이며, 각 디렉터리는 동일한 3개 파일 구조를 가집니다.

* `<name>.py` — 클래스 정의 원본 (**모델을 보려면 여기부터**)
* `<name>.ipynb` — 설명이 포함된 주피터 노트북 (README에서 링크하는 대상)
* `__init__.py`

| 문제 | 경로 | 주요 클래스 |
|---|---|---|
| 콜센터 | `solutions/object_oriented_design/call_center/call_center.py` | `Rank(Enum)`, `Employee`(추상), `Operator`/`Supervisor`/`Director`, `CallState(Enum)`, `Call`, `CallCenter` |
| 카드 덱 | `.../deck_of_cards/deck_of_cards.py` | `Suit(Enum)`, `Card`(추상), `BlackJackCard`, `Hand`, `BlackJackHand`, `Deck` |
| 해시 테이블 | `.../hash_table/hash_map.py` | `Item`, `HashTable` |
| LRU 캐시 | `.../lru_cache/lru_cache.py` | `Node`, `LinkedList`, `Cache` |
| 온라인 채팅 | `.../online_chat/online_chat.py` | `UserService`, `User`, `Chat`(추상), `PrivateChat`/`GroupChat`, `Message`, `AddRequest`, `RequestStatus(Enum)` |
| 주차장 | `.../parking_lot/parking_lot.py` | `VehicleSize(Enum)`, `Vehicle`(추상), `Motorcycle`/`Car`/`Bus`, `ParkingLot`, `Level`, `ParkingSpot` |

> 상속 계층(추상 클래스 → 구현 클래스)과 Enum 사용이 이 저장소 객체 모델링의 일관된 패턴입니다.
> 별도의 UML 다이어그램 파일은 제공되지 않으며, **코드 자체가 클래스 모델**입니다.

---

## ④ 아키텍처 다이어그램 원본이 있는 곳

다이어그램은 두 형태로 저장되어 있습니다.

* **`.graffle`** — [OmniGraffle](https://www.omnigroup.com/omnigraffle) 편집용 원본 (macOS 전용 도구)
* **`.png`** — 문서에 삽입되는 렌더링 결과

| 시스템 | 편집 원본 | 렌더링 |
|---|---|---|
| Pastebin | `pastebin.graffle`, `pastebin_basic.graffle` | `pastebin.png`, `pastebin_basic.png` |
| Twitter | `twitter.graffle`, `twitter_basic.graffle` | `twitter.png`, `twitter_basic.png` |
| Web Crawler | `web_crawler.graffle`, `web_crawler_basic.graffle` | `web_crawler.png`, `web_crawler_basic.png` |
| Mint | `mint.graffle`, `mint_basic.graffle` | `mint.png`, `mint_basic.png` |
| Social Graph | `social_graph.graffle`, `social_graph_basic.graffle` | `social_graph.png`, `social_graph_basic.png` |
| Query Cache | `query_cache.graffle`, `query_cache_basic.graffle` | `query_cache.png`, `query_cache_basic.png` |
| Sales Rank | `sales_rank.graffle`, `sales_rank_basic.graffle` | `sales_rank.png`, `sales_rank_basic.png` |
| Scaling AWS | `scaling_aws.graffle` | `scaling_aws_1.png` ~ `scaling_aws_7.png` |

* 모든 경로 접두사는 `solutions/system_design/<시스템명>/` 입니다.
* `*_basic` 은 Step 2(상위 수준 설계), 접미사 없는 쪽은 Step 4(확장 후 설계)에 해당합니다.
* 새 문제를 추가할 때 쓰는 **빈 템플릿**: `solutions/system_design/template/template.graffle`
* README 본문에 삽입되는 개념도(캐시 패턴, CAP, 복제 등)는 최상위 `images/` 디렉터리에 있습니다.

---

## 빠르게 찾는 요령

```bash
# 관계형 스키마(DDL)가 있는 곳
grep -rn "PRIMARY KEY" --include=README.md solutions/

# 모든 클래스 모델 정의 위치
grep -rn "^class " solutions/

# 편집 가능한 다이어그램 원본 목록
find solutions -name "*.graffle"
```

---

*원저작물: [donnemartin/system-design-primer](https://github.com/donnemartin/system-design-primer), CC BY 4.0. 본 문서는 해당 저장소의 구조를 한국어로 안내하기 위해 작성된 부속 문서입니다.*
