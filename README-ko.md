*[English](README.md) ∙ [日本語](README-ja.md) ∙ [简体中文](README-zh-Hans.md) ∙ [繁體中文](README-zh-TW.md) ∙ [한국어](README-ko.md) | [Add Translation](https://github.com/donnemartin/system-design-primer/issues/28)*

> 이 문서는 [The System Design Primer](https://github.com/donnemartin/system-design-primer)(원저자: [Donne Martin](https://github.com/donnemartin), CC BY 4.0)의 한국어 번역본입니다.
> 원문: [README.md](README.md)

# The System Design Primer (시스템 설계 입문서)

<p align="center">
  <img src="images/jj3A5N8.png">
  <br/>
</p>

## 동기

> 대규모 시스템을 설계하는 방법을 배웁니다.
>
> 시스템 설계 면접을 준비합니다.

### 대규모 시스템 설계 방법 배우기

확장 가능한(scalable) 시스템을 설계하는 방법을 익히면 더 나은 엔지니어가 될 수 있습니다.

시스템 설계는 범위가 매우 넓은 주제입니다. 시스템 설계 원칙에 대한 **방대한 양의 자료가 웹 곳곳에 흩어져** 있습니다.

이 저장소는 대규모 시스템을 만드는 방법을 배울 수 있도록 자료를 **체계적으로 모아 정리**한 것입니다.

### 오픈 소스 커뮤니티에서 배우기

이 프로젝트는 지속적으로 갱신되는 오픈 소스 프로젝트입니다.

[기여](#기여하기)는 언제나 환영합니다!

### 시스템 설계 면접 준비하기

코딩 면접뿐 아니라, 많은 기술 회사에서 시스템 설계는 **기술 면접 과정의 필수 요소**입니다.

**자주 나오는 시스템 설계 면접 문제를 연습**하고, 토론·코드·다이어그램으로 구성된 **예시 해답과 비교**해 보세요.

면접 준비를 위한 추가 주제:

* [학습 가이드](#학습-가이드)
* [시스템 설계 면접 문제에 접근하는 방법](#시스템-설계-면접-문제에-접근하는-방법)
* [시스템 설계 면접 문제와 **해답**](#시스템-설계-면접-문제와-해답)
* [객체지향 설계 면접 문제와 **해답**](#객체지향-설계-면접-문제와-해답)
* [추가 시스템 설계 면접 문제](#추가-시스템-설계-면접-문제)

## Anki 플래시카드

<p align="center">
  <img src="images/zdCAkB3.png">
  <br/>
</p>

제공되는 [Anki 플래시카드 덱](https://apps.ankiweb.net/)은 간격 반복(spaced repetition) 학습을 통해 핵심 시스템 설계 개념을 오래 기억하도록 도와줍니다.

* [시스템 설계 덱](https://github.com/donnemartin/system-design-primer/tree/master/resources/flash_cards/System%20Design.apkg)
* [시스템 설계 연습문제 덱](https://github.com/donnemartin/system-design-primer/tree/master/resources/flash_cards/System%20Design%20Exercises.apkg)
* [객체지향 설계 연습문제 덱](https://github.com/donnemartin/system-design-primer/tree/master/resources/flash_cards/OO%20Design.apkg)

이동 중에 학습하기 좋습니다.

### 코딩 학습 자료: Interactive Coding Challenges

[**코딩 면접**](https://github.com/donnemartin/interactive-coding-challenges) 준비 자료를 찾고 있나요?

<p align="center">
  <img src="images/b4YtAEN.png">
  <br/>
</p>

자매 저장소인 [**Interactive Coding Challenges**](https://github.com/donnemartin/interactive-coding-challenges)를 확인해 보세요. 여기에는 추가 Anki 덱이 포함되어 있습니다.

* [코딩 덱](https://github.com/donnemartin/interactive-coding-challenges/tree/master/anki_cards/Coding.apkg)

## 기여하기

> 커뮤니티에서 배우세요.

다음과 같은 도움을 주는 풀 리퀘스트를 자유롭게 보내주세요.

* 오류 수정
* 기존 섹션 개선
* 새로운 섹션 추가
* [번역](https://github.com/donnemartin/system-design-primer/issues/28)

다듬어야 할 내용은 [개발 중](#개발-중) 섹션에 정리되어 있습니다.

[기여 가이드라인](CONTRIBUTING.md)을 참고하세요.

## 시스템 설계 주제 색인

> 다양한 시스템 설계 주제의 장단점을 포함한 요약입니다. **모든 것은 트레이드오프입니다.**
>
> 각 섹션에는 더 깊이 있는 자료로 향하는 링크가 있습니다.

<p align="center">
  <img src="images/jrUBAF7.png">
  <br/>
</p>

* [시스템 설계 주제: 여기서 시작하세요](#시스템-설계-주제-여기서-시작하세요)
    * [1단계: 확장성 강의 영상 보기](#1단계-확장성-강의-영상-보기)
    * [2단계: 확장성 아티클 읽기](#2단계-확장성-아티클-읽기)
    * [다음 단계](#다음-단계)
* [성능 vs 확장성](#성능-vs-확장성)
* [지연 시간 vs 처리량](#지연-시간-vs-처리량)
* [가용성 vs 일관성](#가용성-vs-일관성)
    * [CAP 정리](#cap-정리)
        * [CP - 일관성과 분할 내성](#cp---일관성과-분할-내성)
        * [AP - 가용성과 분할 내성](#ap---가용성과-분할-내성)
* [일관성 패턴](#일관성-패턴)
    * [약한 일관성](#약한-일관성)
    * [최종적 일관성](#최종적-일관성)
    * [강한 일관성](#강한-일관성)
* [가용성 패턴](#가용성-패턴)
    * [장애 조치(Fail-over)](#장애-조치fail-over)
    * [복제(Replication)](#복제replication)
    * [숫자로 보는 가용성](#숫자로-보는-가용성)
* [도메인 네임 시스템](#도메인-네임-시스템)
* [콘텐츠 전송 네트워크(CDN)](#콘텐츠-전송-네트워크cdn)
    * [푸시 CDN](#푸시-cdn)
    * [풀 CDN](#풀-cdn)
* [로드 밸런서](#로드-밸런서)
    * [액티브-패시브](#액티브-패시브)
    * [액티브-액티브](#액티브-액티브)
    * [레이어 4 로드 밸런싱](#레이어-4-로드-밸런싱)
    * [레이어 7 로드 밸런싱](#레이어-7-로드-밸런싱)
    * [수평 확장](#수평-확장)
* [리버스 프록시(웹 서버)](#리버스-프록시웹-서버)
    * [로드 밸런서 vs 리버스 프록시](#로드-밸런서-vs-리버스-프록시)
* [애플리케이션 계층](#애플리케이션-계층)
    * [마이크로서비스](#마이크로서비스)
    * [서비스 디스커버리](#서비스-디스커버리)
* [데이터베이스](#데이터베이스)
    * [관계형 데이터베이스 관리 시스템(RDBMS)](#관계형-데이터베이스-관리-시스템rdbms)
        * [마스터-슬레이브 복제](#마스터-슬레이브-복제)
        * [마스터-마스터 복제](#마스터-마스터-복제)
        * [페더레이션](#페더레이션)
        * [샤딩](#샤딩)
        * [비정규화](#비정규화)
        * [SQL 튜닝](#sql-튜닝)
    * [NoSQL](#nosql)
        * [키-값 저장소](#키-값-저장소)
        * [문서 저장소](#문서-저장소)
        * [와이드 칼럼 저장소](#와이드-칼럼-저장소)
        * [그래프 데이터베이스](#그래프-데이터베이스)
    * [SQL이냐 NoSQL이냐](#sql이냐-nosql이냐)
* [캐시](#캐시)
    * [클라이언트 캐싱](#클라이언트-캐싱)
    * [CDN 캐싱](#cdn-캐싱)
    * [웹 서버 캐싱](#웹-서버-캐싱)
    * [데이터베이스 캐싱](#데이터베이스-캐싱)
    * [애플리케이션 캐싱](#애플리케이션-캐싱)
    * [데이터베이스 쿼리 수준의 캐싱](#데이터베이스-쿼리-수준의-캐싱)
    * [객체 수준의 캐싱](#객체-수준의-캐싱)
    * [캐시를 갱신하는 시점](#캐시를-갱신하는-시점)
        * [캐시 어사이드(Cache-aside)](#캐시-어사이드cache-aside)
        * [라이트 스루(Write-through)](#라이트-스루write-through)
        * [라이트 비하인드(Write-behind, write-back)](#라이트-비하인드write-behind-write-back)
        * [리프레시 어헤드(Refresh-ahead)](#리프레시-어헤드refresh-ahead)
* [비동기](#비동기)
    * [메시지 큐](#메시지-큐)
    * [태스크 큐](#태스크-큐)
    * [배압(Back pressure)](#배압back-pressure)
* [통신](#통신)
    * [전송 제어 프로토콜(TCP)](#전송-제어-프로토콜tcp)
    * [사용자 데이터그램 프로토콜(UDP)](#사용자-데이터그램-프로토콜udp)
    * [원격 프로시저 호출(RPC)](#원격-프로시저-호출rpc)
    * [REST(Representational state transfer)](#restrepresentational-state-transfer)
* [보안](#보안)
* [부록](#부록)
    * [2의 거듭제곱 표](#2의-거듭제곱-표)
    * [모든 프로그래머가 알아야 할 지연 시간 수치](#모든-프로그래머가-알아야-할-지연-시간-수치)
    * [추가 시스템 설계 면접 문제](#추가-시스템-설계-면접-문제)
    * [실제 아키텍처 사례](#실제-아키텍처-사례)
    * [기업별 아키텍처](#기업별-아키텍처)
    * [기업 엔지니어링 블로그](#기업-엔지니어링-블로그)
* [개발 중](#개발-중)
* [크레딧](#크레딧)
* [연락처](#연락처)
* [라이선스](#라이선스)

## 학습 가이드

> 면접까지 남은 기간(짧음, 보통, 김)에 따라 검토할 주제를 제안합니다.

![Imgur](images/OfVllex.png)

**Q: 면접을 위해 여기 있는 내용을 전부 알아야 하나요?**

**A: 아니요, 면접 준비를 위해 여기 있는 모든 것을 알 필요는 없습니다.**

면접에서 무엇을 질문받을지는 다음과 같은 변수에 따라 달라집니다.

* 경력이 얼마나 되는지
* 기술적 배경이 무엇인지
* 어떤 직무에 지원하는지
* 어떤 회사와 면접을 보는지
* 운

일반적으로 경력이 많은 지원자일수록 시스템 설계에 대해 더 많이 알 것으로 기대됩니다. 아키텍트나 팀 리드는 개별 기여자(IC)보다 더 많은 것을 요구받을 수 있습니다. 최상위 기술 회사는 한 번 이상의 설계 면접 라운드를 진행할 가능성이 높습니다.

넓게 시작한 뒤 몇 가지 영역을 깊게 파고드세요. 다양한 핵심 시스템 설계 주제를 조금씩이라도 알아두면 도움이 됩니다. 아래 가이드는 남은 기간, 경력, 지원 직무, 지원 회사에 맞춰 조정하세요.

* **기간이 짧을 때** - 시스템 설계 주제의 **폭**을 목표로 하세요. 면접 문제 **일부**를 풀며 연습하세요.
* **기간이 보통일 때** - 시스템 설계 주제의 **폭**과 **어느 정도의 깊이**를 목표로 하세요. 면접 문제 **다수**를 풀며 연습하세요.
* **기간이 길 때** - 시스템 설계 주제의 **폭**과 **더 깊은 깊이**를 목표로 하세요. 면접 문제 **대부분**을 풀며 연습하세요.

| | 짧음 | 보통 | 김 |
|---|---|---|---|
| 시스템이 어떻게 동작하는지 폭넓게 이해하기 위해 [시스템 설계 주제](#시스템-설계-주제-색인) 읽기 | :+1: | :+1: | :+1: |
| 지원하는 회사의 [기업 엔지니어링 블로그](#기업-엔지니어링-블로그) 글 몇 편 읽기 | :+1: | :+1: | :+1: |
| [실제 아키텍처 사례](#실제-아키텍처-사례) 몇 가지 읽기 | :+1: | :+1: | :+1: |
| [시스템 설계 면접 문제에 접근하는 방법](#시스템-설계-면접-문제에-접근하는-방법) 검토하기 | :+1: | :+1: | :+1: |
| [시스템 설계 면접 문제와 해답](#시스템-설계-면접-문제와-해답) 풀어보기 | 일부 | 다수 | 대부분 |
| [객체지향 설계 면접 문제와 해답](#객체지향-설계-면접-문제와-해답) 풀어보기 | 일부 | 다수 | 대부분 |
| [추가 시스템 설계 면접 문제](#추가-시스템-설계-면접-문제) 검토하기 | 일부 | 다수 | 대부분 |

## 시스템 설계 면접 문제에 접근하는 방법

> 시스템 설계 면접 문제를 다루는 방법입니다.

시스템 설계 면접은 **정해진 답이 없는 열린 대화**입니다. 그 대화를 이끄는 것은 지원자의 몫입니다.

다음 단계를 활용해 논의를 이끌 수 있습니다. 이 과정을 몸에 익히려면 [시스템 설계 면접 문제와 해답](#시스템-설계-면접-문제와-해답) 섹션을 아래 단계에 따라 풀어보세요.

### 1단계: 유스케이스, 제약 조건, 가정 정리하기

요구사항을 수집하고 문제 범위를 정합니다. 유스케이스와 제약 조건을 명확히 하기 위해 질문하세요. 가정에 대해 논의하세요.

* 누가 사용하나요?
* 어떻게 사용하나요?
* 사용자는 몇 명인가요?
* 시스템은 무엇을 하나요?
* 시스템의 입력과 출력은 무엇인가요?
* 처리해야 할 데이터 양은 어느 정도인가요?
* 초당 요청 수는 얼마나 예상되나요?
* 예상되는 읽기 대 쓰기 비율은 얼마인가요?

### 2단계: 상위 수준 설계 만들기

중요한 구성 요소를 모두 담아 상위 수준 설계를 그립니다.

* 주요 컴포넌트와 연결 관계를 스케치하기
* 자신의 아이디어를 정당화하기

### 3단계: 핵심 컴포넌트 설계하기

각 핵심 컴포넌트를 깊이 파고듭니다. 예를 들어 [URL 단축 서비스 설계](solutions/system_design/pastebin/README-ko.md)를 요청받았다면 다음을 논의합니다.

* 전체 URL의 해시를 생성하고 저장하기
    * [MD5](solutions/system_design/pastebin/README-ko.md)와 [Base62](solutions/system_design/pastebin/README-ko.md)
    * 해시 충돌
    * SQL이냐 NoSQL이냐
    * 데이터베이스 스키마
* 해시된 URL을 전체 URL로 변환하기
    * 데이터베이스 조회
* API와 객체지향 설계

### 4단계: 설계 확장하기

제약 조건을 고려해 병목을 찾아내고 해결합니다. 예를 들어 확장성 문제를 해결하기 위해 다음이 필요한가요?

* 로드 밸런서
* 수평 확장
* 캐싱
* 데이터베이스 샤딩

가능한 해법과 트레이드오프를 논의하세요. 모든 것은 트레이드오프입니다. [확장 가능한 시스템 설계 원칙](#시스템-설계-주제-색인)을 활용해 병목을 해결하세요.

### 봉투 뒷면 계산(Back-of-the-envelope calculations)

손으로 간단한 추정을 해보라는 요청을 받을 수 있습니다. 다음 자료는 [부록](#부록)을 참고하세요.

* [봉투 뒷면 계산 활용하기](http://highscalability.com/blog/2011/1/26/google-pro-tip-use-back-of-the-envelope-calculations-to-choo.html)
* [2의 거듭제곱 표](#2의-거듭제곱-표)
* [모든 프로그래머가 알아야 할 지연 시간 수치](#모든-프로그래머가-알아야-할-지연-시간-수치)

### 출처 및 더 읽을거리

무엇을 기대해야 할지 감을 잡으려면 다음 링크를 확인하세요.

* [How to ace a systems design interview](https://web.archive.org/web/20210505130322/https://www.palantir.com/2011/10/how-to-rock-a-systems-design-interview/)
* [The system design interview](http://www.hiredintech.com/system-design)
* [Intro to Architecture and Systems Design Interviews](https://www.youtube.com/watch?v=ZgdS0EUmn70)
* [System design template](https://leetcode.com/discuss/career/229177/My-System-Design-Template)

## 시스템 설계 면접 문제와 해답

> 자주 나오는 시스템 설계 면접 문제와 예시 토론, 코드, 다이어그램입니다.
>
> 해답은 `solutions/` 폴더의 내용으로 연결됩니다.

| 문제 | |
|---|---|
| Pastebin.com(또는 Bit.ly) 설계하기 | [해답](solutions/system_design/pastebin/README-ko.md) |
| 트위터 타임라인과 검색(또는 페이스북 피드와 검색) 설계하기 | [해답](solutions/system_design/twitter/README-ko.md) |
| 웹 크롤러 설계하기 | [해답](solutions/system_design/web_crawler/README-ko.md) |
| Mint.com 설계하기 | [해답](solutions/system_design/mint/README-ko.md) |
| 소셜 네트워크를 위한 자료구조 설계하기 | [해답](solutions/system_design/social_graph/README-ko.md) |
| 검색 엔진을 위한 키-값 저장소 설계하기 | [해답](solutions/system_design/query_cache/README-ko.md) |
| 아마존의 카테고리별 판매 순위 기능 설계하기 | [해답](solutions/system_design/sales_rank/README-ko.md) |
| AWS에서 수백만 사용자까지 확장되는 시스템 설계하기 | [해답](solutions/system_design/scaling_aws/README-ko.md) |
| 시스템 설계 문제 추가하기 | [기여하기](#기여하기) |

### Pastebin.com(또는 Bit.ly) 설계하기

[문제와 해답 보기](solutions/system_design/pastebin/README-ko.md)

![Imgur](images/4edXG0T.png)

### 트위터 타임라인과 검색(또는 페이스북 피드와 검색) 설계하기

[문제와 해답 보기](solutions/system_design/twitter/README-ko.md)

![Imgur](images/jrUBAF7.png)

### 웹 크롤러 설계하기

[문제와 해답 보기](solutions/system_design/web_crawler/README-ko.md)

![Imgur](images/bWxPtQA.png)

### Mint.com 설계하기

[문제와 해답 보기](solutions/system_design/mint/README-ko.md)

![Imgur](images/V5q57vU.png)

### 소셜 네트워크를 위한 자료구조 설계하기

[문제와 해답 보기](solutions/system_design/social_graph/README-ko.md)

![Imgur](images/cdCv5g7.png)

### 검색 엔진을 위한 키-값 저장소 설계하기

[문제와 해답 보기](solutions/system_design/query_cache/README-ko.md)

![Imgur](images/4j99mhe.png)

### 아마존의 카테고리별 판매 순위 기능 설계하기

[문제와 해답 보기](solutions/system_design/sales_rank/README-ko.md)

![Imgur](images/MzExP06.png)

### AWS에서 수백만 사용자까지 확장되는 시스템 설계하기

[문제와 해답 보기](solutions/system_design/scaling_aws/README-ko.md)

![Imgur](images/jj3A5N8.png)

## 객체지향 설계 면접 문제와 해답

> 자주 나오는 객체지향 설계 면접 문제와 예시 토론, 코드, 다이어그램입니다.
>
> 해답은 `solutions/` 폴더의 내용으로 연결됩니다.

>**참고: 이 섹션은 개발 중입니다.**

| 문제 | |
|---|---|
| 해시 맵 설계하기 | [해답](solutions/object_oriented_design/hash_table/hash_map.ipynb)  |
| LRU(Least Recently Used) 캐시 설계하기 | [해답](solutions/object_oriented_design/lru_cache/lru_cache.ipynb)  |
| 콜센터 설계하기 | [해답](solutions/object_oriented_design/call_center/call_center.ipynb)  |
| 카드 덱 설계하기 | [해답](solutions/object_oriented_design/deck_of_cards/deck_of_cards.ipynb)  |
| 주차장 설계하기 | [해답](solutions/object_oriented_design/parking_lot/parking_lot.ipynb)  |
| 채팅 서버 설계하기 | [해답](solutions/object_oriented_design/online_chat/online_chat.ipynb)  |
| 원형 배열 설계하기 | [기여하기](#기여하기)  |
| 객체지향 설계 문제 추가하기 | [기여하기](#기여하기) |

## 시스템 설계 주제: 여기서 시작하세요

시스템 설계가 처음이신가요?

우선 흔히 쓰이는 원칙들이 무엇인지, 어떻게 사용되는지, 장단점은 무엇인지 기본적인 이해가 필요합니다.

### 1단계: 확장성 강의 영상 보기

[하버드 확장성 강의](https://www.youtube.com/watch?v=-W9F__D3oY4)

* 다루는 주제:
    * 수직 확장(Vertical scaling)
    * 수평 확장(Horizontal scaling)
    * 캐싱
    * 로드 밸런싱
    * 데이터베이스 복제
    * 데이터베이스 파티셔닝

### 2단계: 확장성 아티클 읽기

[Scalability](https://web.archive.org/web/20221030091841/http://www.lecloud.net/tagged/scalability/chrono)

* 다루는 주제:
    * [클론](https://web.archive.org/web/20220530193911/https://www.lecloud.net/post/7295452622/scalability-for-dummies-part-1-clones)
    * [데이터베이스](https://web.archive.org/web/20220602114024/https://www.lecloud.net/post/7994751381/scalability-for-dummies-part-2-database)
    * [캐시](https://web.archive.org/web/20230126233752/https://www.lecloud.net/post/9246290032/scalability-for-dummies-part-3-cache)
    * [비동기](https://web.archive.org/web/20220926171507/https://www.lecloud.net/post/9699762917/scalability-for-dummies-part-4-asynchronism)

### 다음 단계

다음으로 상위 수준의 트레이드오프를 살펴봅니다.

* **성능** vs **확장성**
* **지연 시간** vs **처리량**
* **가용성** vs **일관성**

**모든 것은 트레이드오프**라는 점을 기억하세요.

그다음에는 DNS, CDN, 로드 밸런서 같은 더 구체적인 주제로 들어갑니다.

## 성능 vs 확장성

추가한 자원에 비례해 **성능**이 향상된다면 그 서비스는 **확장 가능(scalable)**합니다. 일반적으로 성능 향상이란 더 많은 작업 단위를 처리하는 것을 의미하지만, 데이터셋이 커질 때처럼 더 큰 작업 단위를 감당하는 것을 뜻하기도 합니다.<sup><a href=http://www.allthingsdistributed.com/2006/03/a_word_on_scalability.html>1</a></sup>

성능과 확장성을 구분하는 또 다른 관점:

* **성능** 문제가 있다면, 사용자 한 명에게도 시스템이 느립니다.
* **확장성** 문제가 있다면, 사용자 한 명에게는 빠르지만 부하가 높아지면 느려집니다.

### 출처 및 더 읽을거리

* [A word on scalability](http://www.allthingsdistributed.com/2006/03/a_word_on_scalability.html)
* [Scalability, availability, stability, patterns](http://www.slideshare.net/jboner/scalability-availability-stability-patterns/)

## 지연 시간 vs 처리량

**지연 시간(Latency)**은 어떤 동작을 수행하거나 결과를 만들어내는 데 걸리는 시간입니다.

**처리량(Throughput)**은 단위 시간당 그러한 동작 또는 결과의 개수입니다.

일반적으로 **허용 가능한 지연 시간**을 유지하면서 **최대 처리량**을 목표로 해야 합니다.

### 출처 및 더 읽을거리

* [Understanding latency vs throughput](https://community.cadence.com/cadence_blogs_8/b/fv/posts/understanding-latency-vs-throughput)

## 가용성 vs 일관성

### CAP 정리

<p align="center">
  <img src="images/bgLMI2u.png">
  <br/>
  <i><a href="https://robertgreiner.com/cap-theorem-revisited">출처: CAP theorem revisited</a></i>
</p>

분산 컴퓨터 시스템에서는 다음 보장 중 두 가지만 지원할 수 있습니다.

* **일관성(Consistency)** - 모든 읽기는 가장 최근의 쓰기 결과를 받거나 오류를 받습니다.
* **가용성(Availability)** - 모든 요청은 응답을 받지만, 그 응답이 가장 최신 정보를 담고 있다는 보장은 없습니다.
* **분할 내성(Partition Tolerance)** - 네트워크 장애로 인해 임의의 분할이 발생해도 시스템은 계속 동작합니다.

*네트워크는 신뢰할 수 없으므로 분할 내성은 반드시 지원해야 합니다. 따라서 일관성과 가용성 사이에서 소프트웨어적인 트레이드오프를 선택하게 됩니다.*

#### CP - 일관성과 분할 내성

분할된 노드의 응답을 기다리다가 타임아웃 오류가 발생할 수 있습니다. 비즈니스 요구사항이 원자적(atomic) 읽기·쓰기를 필요로 한다면 CP가 좋은 선택입니다.

#### AP - 가용성과 분할 내성

응답은 어느 노드에서든 가장 즉시 사용 가능한 버전의 데이터를 반환하며, 이는 최신이 아닐 수 있습니다. 분할이 해소된 후 쓰기가 전파되는 데 시간이 걸릴 수 있습니다.

비즈니스가 [최종적 일관성](#최종적-일관성)을 허용할 수 있거나, 외부 오류에도 시스템이 계속 동작해야 한다면 AP가 좋은 선택입니다.

### 출처 및 더 읽을거리

* [CAP theorem revisited](https://robertgreiner.com/cap-theorem-revisited/)
* [A plain english introduction to CAP theorem](http://ksat.me/a-plain-english-introduction-to-cap-theorem)
* [CAP FAQ](https://github.com/henryr/cap-faq)
* [The CAP theorem](https://www.youtube.com/watch?v=k-Yaq8AHlFA)

## 일관성 패턴

같은 데이터의 복사본이 여러 개 있을 때, 클라이언트가 일관된 데이터 뷰를 볼 수 있도록 이들을 어떻게 동기화할지 선택해야 합니다. [CAP 정리](#cap-정리)에서의 일관성 정의를 떠올려 보세요. 모든 읽기는 가장 최근의 쓰기 결과를 받거나 오류를 받습니다.

### 약한 일관성

쓰기 이후의 읽기가 그 결과를 볼 수도, 보지 못할 수도 있습니다. 최선 노력(best effort) 방식입니다.

memcached 같은 시스템에서 볼 수 있습니다. 약한 일관성은 VoIP, 영상 채팅, 실시간 멀티플레이어 게임 같은 실시간 유스케이스에 적합합니다. 예를 들어 통화 중 몇 초간 수신이 끊겼다가 다시 연결되면, 끊긴 동안 상대가 말한 내용은 들리지 않습니다.

### 최종적 일관성

쓰기 이후의 읽기는 결국(보통 수 밀리초 이내) 그 결과를 보게 됩니다. 데이터는 비동기적으로 복제됩니다.

DNS나 이메일 같은 시스템에서 볼 수 있습니다. 최종적 일관성은 고가용성 시스템에 적합합니다.

### 강한 일관성

쓰기 이후의 읽기는 그 결과를 봅니다. 데이터는 동기적으로 복제됩니다.

파일 시스템과 RDBMS에서 볼 수 있습니다. 강한 일관성은 트랜잭션이 필요한 시스템에 적합합니다.

### 출처 및 더 읽을거리

* [Transactions across data centers](http://snarfed.org/transactions_across_datacenters_io.html)

## 가용성 패턴

고가용성을 지원하기 위한 두 가지 상호 보완적인 패턴이 있습니다. **장애 조치(fail-over)**와 **복제(replication)**입니다.

### 장애 조치(Fail-over)

#### 액티브-패시브

액티브-패시브 장애 조치에서는 액티브 서버와 대기 중인 패시브 서버 사이에 하트비트가 오갑니다. 하트비트가 끊기면 패시브 서버가 액티브 서버의 IP 주소를 넘겨받아 서비스를 재개합니다.

다운타임의 길이는 패시브 서버가 이미 '핫(hot)' 대기 상태로 실행 중인지, 아니면 '콜드(cold)' 대기 상태에서 부팅해야 하는지에 따라 결정됩니다. 트래픽은 액티브 서버만 처리합니다.

액티브-패시브 장애 조치는 마스터-슬레이브 장애 조치라고도 합니다.

#### 액티브-액티브

액티브-액티브에서는 두 서버 모두 트래픽을 처리하며 부하를 나눠 갖습니다.

서버가 외부에 공개되어 있다면 DNS가 두 서버의 공인 IP를 모두 알아야 합니다. 내부용이라면 애플리케이션 로직이 두 서버를 모두 알고 있어야 합니다.

액티브-액티브 장애 조치는 마스터-마스터 장애 조치라고도 합니다.

### 단점: 장애 조치

* 장애 조치는 하드웨어와 복잡성을 추가로 요구합니다.
* 새로 쓰인 데이터가 패시브 서버로 복제되기 전에 액티브 시스템이 죽으면 데이터가 유실될 수 있습니다.

### 복제(Replication)

#### 마스터-슬레이브와 마스터-마스터

이 주제는 [데이터베이스](#데이터베이스) 섹션에서 더 자세히 다룹니다.

* [마스터-슬레이브 복제](#마스터-슬레이브-복제)
* [마스터-마스터 복제](#마스터-마스터-복제)

### 숫자로 보는 가용성

가용성은 흔히 서비스가 사용 가능한 시간의 비율, 즉 업타임(또는 다운타임)으로 정량화합니다. 일반적으로 9의 개수로 측정하며, 99.99% 가용성을 갖는 서비스는 "나인 네 개(four 9s)"라고 표현합니다.

#### 99.9% 가용성 - 나인 세 개

| 기간            | 허용 가능한 다운타임 |
|---------------------|--------------------|
| 연간 다운타임   | 8시간 45분 57초       |
| 월간 다운타임  | 43분 49.7초          |
| 주간 다운타임   | 10분 4.8초           |
| 일간 다운타임    | 1분 26.4초           |

#### 99.99% 가용성 - 나인 네 개

| 기간            | 허용 가능한 다운타임 |
|---------------------|--------------------|
| 연간 다운타임   | 52분 35.7초        |
| 월간 다운타임  | 4분 23초             |
| 주간 다운타임   | 1분 5초              |
| 일간 다운타임    | 8.6초               |

#### 병렬 구성 vs 직렬 구성에서의 가용성

서비스가 장애가 발생할 수 있는 여러 컴포넌트로 이루어져 있다면, 전체 가용성은 컴포넌트가 직렬로 연결되어 있는지 병렬로 연결되어 있는지에 따라 달라집니다.

###### 직렬 구성

가용성이 100% 미만인 두 컴포넌트가 직렬로 연결되면 전체 가용성은 낮아집니다.

```
가용성 (전체) = 가용성 (Foo) * 가용성 (Bar)
```

`Foo`와 `Bar`가 각각 99.9% 가용성을 갖는다면, 직렬 구성에서의 전체 가용성은 99.8%입니다.

###### 병렬 구성

가용성이 100% 미만인 두 컴포넌트가 병렬로 연결되면 전체 가용성은 높아집니다.

```
가용성 (전체) = 1 - (1 - 가용성 (Foo)) * (1 - 가용성 (Bar))
```

`Foo`와 `Bar`가 각각 99.9% 가용성을 갖는다면, 병렬 구성에서의 전체 가용성은 99.9999%입니다.

## 도메인 네임 시스템

<p align="center">
  <img src="images/IOyLj4i.jpg">
  <br/>
  <i><a href=http://www.slideshare.net/srikrupa5/dns-security-presentation-issa>출처: DNS security presentation</a></i>
</p>

도메인 네임 시스템(DNS)은 www.example.com 같은 도메인 이름을 IP 주소로 변환합니다.

DNS는 계층 구조이며, 최상위에 소수의 권한 있는(authoritative) 서버가 있습니다. 조회를 할 때 어떤 DNS 서버에 접속할지는 라우터나 ISP가 알려줍니다. 하위 DNS 서버는 매핑을 캐시하는데, DNS 전파 지연으로 인해 이 값이 오래된 것일 수 있습니다. DNS 결과는 브라우저나 OS에서도 [TTL(time to live)](https://en.wikipedia.org/wiki/Time_to_live)로 정해진 기간 동안 캐시될 수 있습니다.

* **NS 레코드(name server)** - 도메인/서브도메인의 DNS 서버를 지정합니다.
* **MX 레코드(mail exchange)** - 메시지를 수신할 메일 서버를 지정합니다.
* **A 레코드(address)** - 이름을 IP 주소로 연결합니다.
* **CNAME(canonical)** - 이름을 다른 이름이나 `CNAME`(example.com → www.example.com), 또는 `A` 레코드로 연결합니다.

[CloudFlare](https://www.cloudflare.com/dns/)나 [Route 53](https://aws.amazon.com/route53/) 같은 서비스는 관리형 DNS 서비스를 제공합니다. 일부 DNS 서비스는 다양한 방식으로 트래픽을 라우팅할 수 있습니다.

* [가중치 라운드 로빈](https://www.jscape.com/blog/load-balancing-algorithms)
    * 점검 중인 서버로 트래픽이 가지 않도록 방지
    * 크기가 서로 다른 클러스터 간 부하 분산
    * A/B 테스트
* [지연 시간 기반](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy-latency.html)
* [지리적 위치 기반](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy-geo.html)

### 단점: DNS

* DNS 서버에 접근하는 데 약간의 지연이 발생합니다. 다만 위에서 설명한 캐싱으로 완화됩니다.
* DNS 서버 관리는 복잡할 수 있으며, 일반적으로 [정부, ISP, 대기업](http://superuser.com/questions/472695/who-controls-the-dns-servers/472729)이 관리합니다.
* DNS 서비스는 최근 [DDoS 공격](http://dyn.com/blog/dyn-analysis-summary-of-friday-october-21-attack/)의 표적이 되어, 사용자가 트위터의 IP 주소를 모르는 한 트위터 같은 사이트에 접속하지 못하는 일이 있었습니다.

### 출처 및 더 읽을거리

* [DNS architecture](https://technet.microsoft.com/en-us/library/dd197427(v=ws.10).aspx)
* [Wikipedia](https://en.wikipedia.org/wiki/Domain_Name_System)
* [DNS articles](https://support.dnsimple.com/categories/dns/)

## 콘텐츠 전송 네트워크(CDN)

<p align="center">
  <img src="images/h9TAuGI.jpg">
  <br/>
  <i><a href=https://www.creative-artworks.eu/why-use-a-content-delivery-network-cdn/>출처: Why use a CDN</a></i>
</p>

콘텐츠 전송 네트워크(CDN)는 전 세계에 분산된 프록시 서버 네트워크로, 사용자와 가까운 위치에서 콘텐츠를 제공합니다. 일반적으로 HTML/CSS/JS, 사진, 동영상 같은 정적 파일을 CDN에서 제공하지만, 아마존의 CloudFront처럼 동적 콘텐츠를 지원하는 CDN도 있습니다. 사이트의 DNS 조회 결과가 클라이언트에게 어떤 서버에 접속할지 알려줍니다.

CDN에서 콘텐츠를 제공하면 두 가지 방식으로 성능이 크게 향상됩니다.

* 사용자가 자신과 가까운 데이터 센터에서 콘텐츠를 받습니다.
* CDN이 처리하는 요청은 내 서버가 처리하지 않아도 됩니다.

### 푸시 CDN

푸시 CDN은 서버에서 변경이 일어날 때마다 새 콘텐츠를 전달받습니다. 콘텐츠 제공에 대한 책임을 전적으로 지고, CDN에 직접 업로드하며 URL을 CDN을 가리키도록 변경해야 합니다. 콘텐츠가 언제 만료되고 언제 갱신되는지 설정할 수 있습니다. 콘텐츠는 새로 생기거나 변경되었을 때만 업로드되므로 트래픽은 최소화되지만 저장 공간은 최대로 사용됩니다.

트래픽이 적거나 콘텐츠가 자주 갱신되지 않는 사이트에 푸시 CDN이 적합합니다. 일정 간격으로 다시 가져오는 대신, 콘텐츠를 CDN에 한 번만 올려두면 됩니다.

### 풀 CDN

풀 CDN은 첫 번째 사용자가 콘텐츠를 요청할 때 서버에서 새 콘텐츠를 가져옵니다. 콘텐츠는 서버에 그대로 두고 URL만 CDN을 가리키도록 변경합니다. 콘텐츠가 CDN에 캐시되기 전까지는 요청이 느립니다.

[TTL(time-to-live)](https://en.wikipedia.org/wiki/Time_to_live)이 콘텐츠가 얼마나 오래 캐시될지 결정합니다. 풀 CDN은 CDN의 저장 공간을 최소화하지만, 실제로 변경되지 않았는데도 파일이 만료되어 다시 가져오면 불필요한 트래픽이 생길 수 있습니다.

트래픽이 많은 사이트에는 풀 CDN이 적합합니다. 최근에 요청된 콘텐츠만 CDN에 남으므로 트래픽이 더 고르게 분산됩니다.

### 단점: CDN

* 트래픽에 따라 CDN 비용이 상당할 수 있습니다. 다만 CDN을 쓰지 않았을 때 발생할 추가 비용과 견주어 판단해야 합니다.
* TTL이 만료되기 전에 콘텐츠가 갱신되면 오래된 콘텐츠가 제공될 수 있습니다.
* 정적 콘텐츠의 URL을 CDN을 가리키도록 변경해야 합니다.

### 출처 및 더 읽을거리

* [Globally distributed content delivery](https://figshare.com/articles/Globally_distributed_content_delivery/6605972)
* [The differences between push and pull CDNs](https://www.geeksforgeeks.org/system-design/pull-cdn-vs-push-cdn/)
* [Wikipedia](https://en.wikipedia.org/wiki/Content_delivery_network)

## 로드 밸런서

<p align="center">
  <img src="images/h81n9iK.png">
  <br/>
  <i><a href=http://horicky.blogspot.com/2010/10/scalable-system-design-patterns.html>출처: Scalable system design patterns</a></i>
</p>

로드 밸런서는 들어오는 클라이언트 요청을 애플리케이션 서버, 데이터베이스 같은 컴퓨팅 자원으로 분배합니다. 각 경우에 로드 밸런서는 컴퓨팅 자원의 응답을 해당 클라이언트에게 돌려줍니다. 로드 밸런서는 다음에 효과적입니다.

* 비정상 서버로 요청이 가는 것을 방지
* 자원 과부하 방지
* 단일 장애점(SPOF) 제거에 도움

로드 밸런서는 하드웨어(비쌈)로 구현하거나 HAProxy 같은 소프트웨어로 구현할 수 있습니다.

추가적인 이점:

* **SSL 종료(termination)** - 들어오는 요청을 복호화하고 서버 응답을 암호화하여, 백엔드 서버가 비용이 큰 이 연산을 하지 않아도 되게 합니다.
    * 각 서버에 [X.509 인증서](https://en.wikipedia.org/wiki/X.509)를 설치할 필요가 없어집니다.
* **세션 지속성(Session persistence)** - 웹 앱이 세션을 추적하지 않는 경우, 쿠키를 발급해 특정 클라이언트의 요청을 같은 인스턴스로 라우팅합니다.

장애에 대비해 [액티브-패시브](#액티브-패시브) 또는 [액티브-액티브](#액티브-액티브) 모드로 로드 밸런서를 여러 대 구성하는 것이 일반적입니다.

로드 밸런서는 다음을 비롯한 다양한 기준으로 트래픽을 라우팅할 수 있습니다.

* 무작위(Random)
* 최소 부하(Least loaded)
* 세션/쿠키
* [라운드 로빈 또는 가중치 라운드 로빈](https://www.g33kinfo.com/info/round-robin-vs-weighted-round-robin-lb)
* [레이어 4](#레이어-4-로드-밸런싱)
* [레이어 7](#레이어-7-로드-밸런싱)

### 레이어 4 로드 밸런싱

레이어 4 로드 밸런서는 [전송 계층](#통신) 정보를 보고 요청을 어떻게 분배할지 결정합니다. 일반적으로 헤더의 출발지·목적지 IP 주소와 포트를 사용하며, 패킷의 내용은 보지 않습니다. 레이어 4 로드 밸런서는 [NAT(Network Address Translation)](https://web.archive.org/web/20240117134735/https://www.nginx.com/resources/glossary/layer-4-load-balancing/)를 수행하며 업스트림 서버와 네트워크 패킷을 주고받습니다.

### 레이어 7 로드 밸런싱

레이어 7 로드 밸런서는 [애플리케이션 계층](#통신)을 보고 요청을 어떻게 분배할지 결정합니다. 여기에는 헤더, 메시지, 쿠키의 내용이 포함될 수 있습니다. 레이어 7 로드 밸런서는 네트워크 트래픽을 종료시키고, 메시지를 읽고, 로드 밸런싱 결정을 내린 뒤, 선택한 서버로 연결을 엽니다. 예를 들어 동영상 트래픽은 동영상을 호스팅하는 서버로 보내고, 더 민감한 결제 트래픽은 보안이 강화된 서버로 보낼 수 있습니다.

유연성을 포기하는 대신, 레이어 4 로드 밸런싱은 레이어 7보다 시간과 컴퓨팅 자원을 덜 씁니다. 다만 최신 범용 하드웨어에서는 성능 차이가 미미할 수 있습니다.

### 수평 확장

로드 밸런서는 수평 확장에도 도움이 되어 성능과 가용성을 높입니다. 범용 장비를 사용해 확장(scale out)하는 것이, 더 비싼 하드웨어로 단일 서버를 키우는 **수직 확장(Vertical Scaling)**보다 비용 효율적이고 가용성도 높습니다. 또한 전문화된 엔터프라이즈 시스템보다 범용 하드웨어를 다루는 인재를 채용하기가 더 쉽습니다.

#### 단점: 수평 확장

* 수평 확장은 복잡성을 키우고 서버 복제를 수반합니다.
    * 서버는 무상태(stateless)여야 합니다. 세션이나 프로필 사진 같은 사용자 관련 데이터를 담고 있으면 안 됩니다.
    * 세션은 [데이터베이스](#데이터베이스)(SQL, NoSQL)나 영속적인 [캐시](#캐시)(Redis, Memcached) 같은 중앙 저장소에 보관할 수 있습니다.
* 업스트림 서버가 늘어나면 캐시나 데이터베이스 같은 다운스트림 서버가 더 많은 동시 연결을 처리해야 합니다.

### 단점: 로드 밸런서

* 자원이 부족하거나 설정이 잘못되면 로드 밸런서 자체가 성능 병목이 될 수 있습니다.
* 단일 장애점을 없애려고 로드 밸런서를 도입하면 복잡성이 증가합니다.
* 로드 밸런서가 한 대뿐이면 그 자체가 단일 장애점이며, 여러 대를 구성하면 복잡성이 더 커집니다.

### 출처 및 더 읽을거리

* [NGINX architecture](https://www.nginx.com/blog/inside-nginx-how-we-designed-for-performance-scale/)
* [HAProxy architecture guide](http://www.haproxy.org/download/1.2/doc/architecture.txt)
* [Scalability](https://web.archive.org/web/20220530193911/https://www.lecloud.net/post/7295452622/scalability-for-dummies-part-1-clones)
* [Wikipedia](https://en.wikipedia.org/wiki/Load_balancing_(computing))
* [Layer 4 load balancing](https://www.nginx.com/resources/glossary/layer-4-load-balancing/)
* [Layer 7 load balancing](https://www.nginx.com/resources/glossary/layer-7-load-balancing/)
* [ELB listener config](http://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-listener-config.html)

## 리버스 프록시(웹 서버)

<p align="center">
  <img src="images/n41Azff.png">
  <br/>
  <i><a href=https://upload.wikimedia.org/wikipedia/commons/6/67/Reverse_proxy_h2g2bob.svg>출처: Wikipedia</a></i>
  <br/>
</p>

리버스 프록시는 내부 서비스를 한곳에 모으고 외부에 통합된 인터페이스를 제공하는 웹 서버입니다. 클라이언트의 요청은 이를 처리할 수 있는 서버로 전달되고, 리버스 프록시가 그 서버의 응답을 클라이언트에게 돌려줍니다.

추가적인 이점:

* **보안 강화** - 백엔드 서버 정보 은닉, IP 블랙리스트, 클라이언트별 연결 수 제한
* **확장성과 유연성 향상** - 클라이언트는 리버스 프록시의 IP만 보므로 서버를 확장하거나 설정을 변경하기 쉽습니다.
* **SSL 종료** - 들어오는 요청을 복호화하고 서버 응답을 암호화하여 백엔드 서버가 비용이 큰 연산을 하지 않아도 되게 합니다.
    * 각 서버에 [X.509 인증서](https://en.wikipedia.org/wiki/X.509)를 설치할 필요가 없어집니다.
* **압축** - 서버 응답을 압축합니다.
* **캐싱** - 캐시된 요청에 대한 응답을 반환합니다.
* **정적 콘텐츠** - 정적 콘텐츠를 직접 제공합니다.
    * HTML/CSS/JS
    * 사진
    * 동영상
    * 기타

### 로드 밸런서 vs 리버스 프록시

* 로드 밸런서는 서버가 여러 대일 때 유용합니다. 보통 같은 기능을 수행하는 서버 집합으로 트래픽을 라우팅합니다.
* 리버스 프록시는 웹 서버나 애플리케이션 서버가 한 대뿐일 때도 앞서 설명한 이점을 제공하므로 유용할 수 있습니다.
* NGINX, HAProxy 같은 솔루션은 레이어 7 리버스 프록시와 로드 밸런싱을 모두 지원합니다.

### 단점: 리버스 프록시

* 리버스 프록시를 도입하면 복잡성이 증가합니다.
* 리버스 프록시가 한 대뿐이면 단일 장애점이며, 여러 대([장애 조치](https://en.wikipedia.org/wiki/Failover))를 구성하면 복잡성이 더 커집니다.

### 출처 및 더 읽을거리

* [Reverse proxy vs load balancer](https://www.nginx.com/resources/glossary/reverse-proxy-vs-load-balancer/)
* [NGINX architecture](https://www.nginx.com/blog/inside-nginx-how-we-designed-for-performance-scale/)
* [HAProxy architecture guide](http://www.haproxy.org/download/1.2/doc/architecture.txt)
* [Wikipedia](https://en.wikipedia.org/wiki/Reverse_proxy)

## 애플리케이션 계층

<p align="center">
  <img src="images/yB5SYwm.png">
  <br/>
  <i><a href=http://lethain.com/introduction-to-architecting-systems-for-scale/#platform_layer>출처: Intro to architecting systems for scale</a></i>
</p>

웹 계층을 애플리케이션 계층(플랫폼 계층이라고도 함)과 분리하면 두 계층을 독립적으로 확장하고 설정할 수 있습니다. 새 API를 추가할 때 웹 서버를 반드시 늘리지 않고도 애플리케이션 서버만 추가하면 됩니다. **단일 책임 원칙**은 작고 자율적인 서비스들이 협력하는 구조를 권장합니다. 작은 서비스를 다루는 작은 팀은 빠른 성장을 더 공격적으로 계획할 수 있습니다.

애플리케이션 계층의 워커는 [비동기](#비동기) 처리를 가능하게 하기도 합니다.

### 마이크로서비스

이 논의와 관련된 것이 [마이크로서비스](https://en.wikipedia.org/wiki/Microservices)입니다. 마이크로서비스는 독립적으로 배포 가능한 작고 모듈화된 서비스들의 모음이라고 설명할 수 있습니다. 각 서비스는 고유한 프로세스로 실행되며, 잘 정의된 경량 메커니즘을 통해 통신하여 하나의 비즈니스 목표를 수행합니다. <sup><a href=https://smartbear.com/learn/api-design/what-are-microservices>1</a></sup>

예를 들어 핀터레스트라면 사용자 프로필, 팔로워, 피드, 검색, 사진 업로드 등의 마이크로서비스를 가질 수 있습니다.

### 서비스 디스커버리

[Consul](https://www.consul.io/docs/index.html), [Etcd](https://coreos.com/etcd/docs/latest), [Zookeeper](http://www.slideshare.net/sauravhaloi/introduction-to-apache-zookeeper) 같은 시스템은 등록된 이름, 주소, 포트를 추적하여 서비스들이 서로를 찾을 수 있게 도와줍니다. [헬스 체크](https://www.consul.io/intro/getting-started/checks.html)는 서비스 상태를 확인하는 데 쓰이며 보통 [HTTP](#하이퍼텍스트-전송-프로토콜http) 엔드포인트로 수행합니다. Consul과 Etcd에는 설정 값이나 공유 데이터를 저장하기 좋은 [키-값 저장소](#키-값-저장소)가 내장되어 있습니다.

### 단점: 애플리케이션 계층

* 느슨하게 결합된 서비스로 애플리케이션 계층을 구성하면 (모놀리식 시스템과 비교해) 아키텍처, 운영, 프로세스 관점에서 다른 접근이 필요합니다.
* 마이크로서비스는 배포와 운영 측면에서 복잡성을 더할 수 있습니다.

### 출처 및 더 읽을거리

* [Intro to architecting systems for scale](http://lethain.com/introduction-to-architecting-systems-for-scale)
* [Crack the system design interview](http://www.puncsky.com/blog/2016-02-13-crack-the-system-design-interview)
* [Service oriented architecture](https://en.wikipedia.org/wiki/Service-oriented_architecture)
* [Introduction to Zookeeper](http://www.slideshare.net/sauravhaloi/introduction-to-apache-zookeeper)
* [Here's what you need to know about building microservices](https://cloudncode.wordpress.com/2016/07/22/msa-getting-started/)

## 데이터베이스

<p align="center">
  <img src="images/Xkm5CXz.png">
  <br/>
  <i><a href=https://www.youtube.com/watch?v=kKjm4ehYiMs>출처: Scaling up to your first 10 million users</a></i>
</p>

### 관계형 데이터베이스 관리 시스템(RDBMS)

SQL 같은 관계형 데이터베이스는 테이블로 조직된 데이터 항목의 모음입니다.

**ACID**는 관계형 데이터베이스 [트랜잭션](https://en.wikipedia.org/wiki/Database_transaction)의 속성 집합입니다.

* **원자성(Atomicity)** - 각 트랜잭션은 전부 수행되거나 전혀 수행되지 않습니다.
* **일관성(Consistency)** - 모든 트랜잭션은 데이터베이스를 하나의 유효한 상태에서 다른 유효한 상태로 옮깁니다.
* **격리성(Isolation)** - 트랜잭션을 동시에 실행한 결과가 순차적으로 실행한 결과와 같습니다.
* **지속성(Durability)** - 한 번 커밋된 트랜잭션은 그대로 유지됩니다.

관계형 데이터베이스를 확장하는 기법은 여러 가지가 있습니다. **마스터-슬레이브 복제**, **마스터-마스터 복제**, **페더레이션**, **샤딩**, **비정규화**, **SQL 튜닝**입니다.

#### 마스터-슬레이브 복제

마스터가 읽기와 쓰기를 모두 처리하고, 쓰기를 하나 이상의 슬레이브로 복제합니다. 슬레이브는 읽기만 처리합니다. 슬레이브는 트리 형태로 다른 슬레이브에 다시 복제할 수도 있습니다. 마스터가 오프라인이 되면, 슬레이브 중 하나가 마스터로 승격되거나 새 마스터가 준비될 때까지 시스템은 읽기 전용 모드로 계속 동작할 수 있습니다.

<p align="center">
  <img src="images/C9ioGtn.png">
  <br/>
  <i><a href=http://www.slideshare.net/jboner/scalability-availability-stability-patterns/>출처: Scalability, availability, stability, patterns</a></i>
</p>

##### 단점: 마스터-슬레이브 복제

* 슬레이브를 마스터로 승격시키기 위한 추가 로직이 필요합니다.
* 마스터-슬레이브와 마스터-마스터 **양쪽 모두**에 해당하는 내용은 [단점: 복제](#단점-복제)를 참고하세요.

#### 마스터-마스터 복제

두 마스터 모두 읽기와 쓰기를 처리하며 쓰기에 대해 서로 협조합니다. 둘 중 하나가 죽어도 시스템은 읽기와 쓰기를 계속 수행할 수 있습니다.

<p align="center">
  <img src="images/krAHLGg.png">
  <br/>
  <i><a href=http://www.slideshare.net/jboner/scalability-availability-stability-patterns/>출처: Scalability, availability, stability, patterns</a></i>
</p>

##### 단점: 마스터-마스터 복제

* 어디에 쓸지 결정하려면 로드 밸런서가 필요하거나 애플리케이션 로직을 변경해야 합니다.
* 대부분의 마스터-마스터 시스템은 일관성이 느슨하거나(ACID 위반) 동기화로 인해 쓰기 지연이 커집니다.
* 쓰기 노드가 늘고 지연이 커질수록 충돌 해결이 더 중요한 문제가 됩니다.
* 마스터-슬레이브와 마스터-마스터 **양쪽 모두**에 해당하는 내용은 [단점: 복제](#단점-복제)를 참고하세요.

##### 단점: 복제

* 새로 쓰인 데이터가 다른 노드로 복제되기 전에 마스터가 죽으면 데이터가 유실될 수 있습니다.
* 쓰기는 읽기 복제본에서 재실행됩니다. 쓰기가 많으면 읽기 복제본이 쓰기 재실행에 매여 읽기를 그만큼 처리하지 못합니다.
* 읽기 슬레이브가 많을수록 복제해야 할 양도 많아져 복제 지연이 커집니다.
* 일부 시스템에서는 마스터에 쓸 때 여러 스레드를 띄워 병렬로 쓰지만, 읽기 복제본은 단일 스레드로 순차적으로만 쓸 수 있습니다.
* 복제는 하드웨어와 복잡성을 추가로 요구합니다.

##### 출처 및 더 읽을거리: 복제

* [Scalability, availability, stability, patterns](http://www.slideshare.net/jboner/scalability-availability-stability-patterns/)
* [Multi-master replication](https://en.wikipedia.org/wiki/Multi-master_replication)

#### 페더레이션

<p align="center">
  <img src="images/U3qV33e.png">
  <br/>
  <i><a href=https://www.youtube.com/watch?v=kKjm4ehYiMs>출처: Scaling up to your first 10 million users</a></i>
</p>

페더레이션(또는 기능적 파티셔닝)은 데이터베이스를 기능별로 나눕니다. 예를 들어 하나의 거대한 데이터베이스 대신 **포럼**, **사용자**, **상품** 세 개의 데이터베이스를 두면, 각 데이터베이스의 읽기·쓰기 트래픽이 줄고 따라서 복제 지연도 줄어듭니다. 데이터베이스가 작아지면 메모리에 더 많은 데이터가 들어가고, 캐시 지역성이 좋아져 캐시 적중률이 올라갑니다. 쓰기를 직렬화하는 중앙 마스터가 하나도 없으므로 병렬로 쓸 수 있어 처리량이 증가합니다.

##### 단점: 페더레이션

* 스키마상 거대한 함수나 테이블이 필요하다면 페더레이션은 효과적이지 않습니다.
* 어느 데이터베이스에서 읽고 쓸지 결정하도록 애플리케이션 로직을 수정해야 합니다.
* 두 데이터베이스의 데이터를 조인하려면 [서버 링크](http://stackoverflow.com/questions/5145637/querying-data-by-joining-two-tables-in-two-database-on-different-servers)가 필요해 더 복잡합니다.
* 페더레이션은 하드웨어와 복잡성을 추가로 요구합니다.

##### 출처 및 더 읽을거리: 페더레이션

* [Scaling up to your first 10 million users](https://www.youtube.com/watch?v=kKjm4ehYiMs)

#### 샤딩

<p align="center">
  <img src="images/wU8x5Id.png">
  <br/>
  <i><a href=http://www.slideshare.net/jboner/scalability-availability-stability-patterns/>출처: Scalability, availability, stability, patterns</a></i>
</p>

샤딩은 각 데이터베이스가 데이터의 일부만 관리하도록 데이터를 여러 데이터베이스에 분산합니다. 사용자 데이터베이스를 예로 들면, 사용자 수가 늘어남에 따라 클러스터에 샤드를 추가합니다.

[페더레이션](#페더레이션)의 장점과 비슷하게, 샤딩은 읽기·쓰기 트래픽과 복제량을 줄이고 캐시 적중률을 높입니다. 인덱스 크기도 줄어들어 일반적으로 쿼리가 빨라지고 성능이 향상됩니다. 샤드 하나가 죽어도 나머지 샤드는 계속 동작합니다. 다만 데이터 유실을 막으려면 어떤 형태로든 복제를 추가하는 것이 좋습니다. 페더레이션처럼 쓰기를 직렬화하는 중앙 마스터가 없으므로 병렬로 쓸 수 있어 처리량이 증가합니다.

사용자 테이블을 샤딩하는 흔한 방법은 사용자의 성(last name) 첫 글자나 지리적 위치를 기준으로 나누는 것입니다.

##### 단점: 샤딩

* 샤드와 함께 동작하도록 애플리케이션 로직을 수정해야 하며, 이로 인해 SQL 쿼리가 복잡해질 수 있습니다.
* 샤드 간 데이터 분포가 치우칠 수 있습니다. 예를 들어 헤비 유저들이 한 샤드에 몰리면 그 샤드의 부하가 다른 샤드보다 커집니다.
    * 재분배(rebalancing)는 복잡성을 더합니다. [일관된 해싱(consistent hashing)](http://www.paperplanes.de/2011/12/9/the-magic-of-consistent-hashing.html) 기반의 샤딩 함수를 쓰면 이동해야 할 데이터 양을 줄일 수 있습니다.
* 여러 샤드에 걸친 데이터 조인은 더 복잡합니다.
* 샤딩은 하드웨어와 복잡성을 추가로 요구합니다.

##### 출처 및 더 읽을거리: 샤딩

* [The coming of the shard](http://highscalability.com/blog/2009/8/6/an-unorthodox-approach-to-database-design-the-coming-of-the.html)
* [Shard database architecture](https://en.wikipedia.org/wiki/Shard_(database_architecture))
* [Consistent hashing](http://www.paperplanes.de/2011/12/9/the-magic-of-consistent-hashing.html)

#### 비정규화

비정규화는 쓰기 성능을 일부 희생해 읽기 성능을 향상시키려는 기법입니다. 비용이 큰 조인을 피하기 위해 데이터의 중복 사본을 여러 테이블에 씁니다. [PostgreSQL](https://en.wikipedia.org/wiki/PostgreSQL), 오라클 같은 일부 RDBMS는 [구체화된 뷰(materialized view)](https://en.wikipedia.org/wiki/Materialized_view)를 지원하여 중복 정보를 저장하고 일관성을 유지하는 작업을 대신해 줍니다.

[페더레이션](#페더레이션)이나 [샤딩](#샤딩) 같은 기법으로 데이터가 분산되고 나면, 데이터 센터에 걸친 조인 관리는 복잡성을 더욱 키웁니다. 비정규화는 이런 복잡한 조인의 필요성을 없앨 수 있습니다.

대부분의 시스템에서 읽기는 쓰기보다 100:1, 심지어 1000:1로 많습니다. 복잡한 데이터베이스 조인을 유발하는 읽기는 디스크 연산에 상당한 시간을 소모하여 매우 비쌀 수 있습니다.

##### 단점: 비정규화

* 데이터가 중복됩니다.
* 제약 조건(constraint)으로 중복 사본의 동기화를 도울 수 있지만, 이는 데이터베이스 설계의 복잡성을 높입니다.
* 쓰기 부하가 큰 상황에서는 비정규화된 데이터베이스가 정규화된 것보다 성능이 나쁠 수 있습니다.

###### 출처 및 더 읽을거리: 비정규화

* [Denormalization](https://en.wikipedia.org/wiki/Denormalization)

#### SQL 튜닝

SQL 튜닝은 범위가 넓은 주제이며, 참고서로 쓸 만한 [책](https://www.amazon.com/s/ref=nb_sb_noss_2?url=search-alias%3Daps&field-keywords=sql+tuning)도 많이 나와 있습니다.

병목을 재현하고 찾아내려면 **벤치마크**와 **프로파일링**이 중요합니다.

* **벤치마크** - [ab](http://httpd.apache.org/docs/2.2/programs/ab.html) 같은 도구로 고부하 상황을 시뮬레이션합니다.
* **프로파일링** - [슬로 쿼리 로그](http://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html) 같은 도구를 켜서 성능 문제를 추적합니다.

벤치마킹과 프로파일링을 하면 다음과 같은 최적화 방향을 찾을 수 있습니다.

##### 스키마를 조밀하게 만들기

* MySQL은 빠른 접근을 위해 연속된 블록 단위로 디스크에 기록합니다.
* 길이가 고정된 필드에는 `VARCHAR` 대신 `CHAR`를 사용하세요.
    * `CHAR`는 사실상 빠른 임의 접근을 가능하게 하지만, `VARCHAR`는 다음 문자열로 넘어가기 전에 문자열의 끝을 찾아야 합니다.
* 블로그 게시글처럼 큰 텍스트 블록에는 `TEXT`를 사용하세요. `TEXT`는 불리언 검색도 지원합니다. `TEXT` 필드를 쓰면 텍스트 블록의 위치를 가리키는 포인터가 디스크에 저장됩니다.
* 2^32(약 40억)까지의 큰 수에는 `INT`를 사용하세요.
* 부동소수점 표현 오차를 피하려면 통화에는 `DECIMAL`을 사용하세요.
* 큰 `BLOB`을 저장하지 말고, 객체를 가져올 위치를 저장하세요.
* `VARCHAR(255)`는 8비트 수로 셀 수 있는 최대 문자 수로, 일부 RDBMS에서 1바이트를 최대한 활용합니다.
* 가능한 곳에는 `NOT NULL` 제약을 설정해 [검색 성능을 개선](http://stackoverflow.com/questions/1017239/how-do-null-values-affect-performance-in-a-database-search)하세요.

##### 좋은 인덱스 사용하기

* 조회에 쓰이는 칼럼(`SELECT`, `GROUP BY`, `ORDER BY`, `JOIN`)은 인덱스가 있으면 더 빨라질 수 있습니다.
* 인덱스는 보통 자가 균형 [B-트리](https://en.wikipedia.org/wiki/B-tree)로 표현되며, 데이터를 정렬된 상태로 유지하고 검색·순차 접근·삽입·삭제를 로그 시간에 수행합니다.
* 인덱스를 두면 데이터를 메모리에 유지하게 되어 더 많은 공간이 필요합니다.
* 인덱스도 갱신해야 하므로 쓰기가 느려질 수 있습니다.
* 대량의 데이터를 적재할 때는 인덱스를 껐다가 데이터를 넣고 인덱스를 다시 만드는 편이 빠를 수 있습니다.

##### 비용이 큰 조인 피하기

* 성능이 요구된다면 [비정규화](#비정규화)하세요.

##### 테이블 파티셔닝

* 자주 접근되는 부분(핫 스팟)을 별도 테이블로 분리해 메모리에 유지되도록 합니다.

##### 쿼리 캐시 튜닝

* 경우에 따라 [쿼리 캐시](https://dev.mysql.com/doc/refman/5.7/en/query-cache.html)가 오히려 [성능 문제](https://www.percona.com/blog/2016/10/12/mysql-5-7-performance-tuning-immediately-after-installation/)를 일으킬 수 있습니다.

##### 출처 및 더 읽을거리: SQL 튜닝

* [Tips for optimizing MySQL queries](http://aiddroid.com/10-tips-optimizing-mysql-queries-dont-suck/)
* [Is there a good reason i see VARCHAR(255) used so often?](http://stackoverflow.com/questions/1217466/is-there-a-good-reason-i-see-varchar255-used-so-often-as-opposed-to-another-l)
* [How do null values affect performance?](http://stackoverflow.com/questions/1017239/how-do-null-values-affect-performance-in-a-database-search)
* [Slow query log](http://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html)

### NoSQL

NoSQL은 **키-값 저장소**, **문서 저장소**, **와이드 칼럼 저장소**, **그래프 데이터베이스** 형태로 표현되는 데이터 항목의 모음입니다. 데이터는 비정규화되며, 조인은 일반적으로 애플리케이션 코드에서 수행합니다. 대부분의 NoSQL 저장소는 진정한 ACID 트랜잭션을 지원하지 않고 [최종적 일관성](#최종적-일관성)을 택합니다.

NoSQL 데이터베이스의 속성을 설명할 때 **BASE**를 자주 사용합니다. [CAP 정리](#cap-정리)와 비교하면 BASE는 일관성보다 가용성을 택합니다.

* **Basically available(기본적 가용성)** - 시스템은 가용성을 보장합니다.
* **Soft state(소프트 상태)** - 입력이 없어도 시스템의 상태는 시간이 지나며 변할 수 있습니다.
* **Eventual consistency(최종적 일관성)** - 그 기간 동안 입력이 없다면, 시스템은 일정 시간이 지난 뒤 일관된 상태가 됩니다.

[SQL이냐 NoSQL이냐](#sql이냐-nosql이냐)를 고르는 것에 더해, 어떤 종류의 NoSQL 데이터베이스가 자신의 유스케이스에 가장 잘 맞는지 이해하면 도움이 됩니다. 다음 절에서 **키-값 저장소**, **문서 저장소**, **와이드 칼럼 저장소**, **그래프 데이터베이스**를 살펴봅니다.

#### 키-값 저장소

> 추상화: 해시 테이블

키-값 저장소는 일반적으로 O(1) 읽기·쓰기를 제공하며 메모리나 SSD를 기반으로 하는 경우가 많습니다. 데이터 저장소가 키를 [사전식 순서](https://en.wikipedia.org/wiki/Lexicographical_order)로 유지하면 키 범위를 효율적으로 조회할 수 있습니다. 키-값 저장소는 값과 함께 메타데이터를 저장할 수 있습니다.

키-값 저장소는 높은 성능을 제공하며, 단순한 데이터 모델이나 인메모리 캐시 계층처럼 빠르게 변하는 데이터에 자주 쓰입니다. 제공하는 연산이 제한적이므로, 추가 연산이 필요하면 그 복잡성은 애플리케이션 계층으로 옮겨갑니다.

키-값 저장소는 문서 저장소나 경우에 따라 그래프 데이터베이스 같은 더 복잡한 시스템의 기반이 됩니다.

##### 출처 및 더 읽을거리: 키-값 저장소

* [Key-value database](https://en.wikipedia.org/wiki/Key-value_database)
* [Disadvantages of key-value stores](http://stackoverflow.com/questions/4056093/what-are-the-disadvantages-of-using-a-key-value-table-over-nullable-columns-or)
* [Redis architecture](http://qnimate.com/overview-of-redis-architecture/)
* [Memcached architecture](https://adayinthelifeof.nl/2011/02/06/memcache-internals/)

#### 문서 저장소

> 추상화: 값으로 문서를 저장하는 키-값 저장소

문서 저장소는 문서(XML, JSON, 바이너리 등)를 중심으로 하며, 하나의 문서가 특정 객체에 대한 모든 정보를 담습니다. 문서 저장소는 문서 내부 구조를 기준으로 질의할 수 있는 API나 질의 언어를 제공합니다. *참고: 많은 키-값 저장소가 값의 메타데이터를 다루는 기능을 포함하고 있어 두 저장소 유형의 경계가 흐려지고 있습니다.*

내부 구현에 따라 문서는 컬렉션, 태그, 메타데이터, 디렉터리로 조직됩니다. 문서를 묶거나 그룹화할 수 있지만, 문서마다 완전히 다른 필드를 가질 수도 있습니다.

[MongoDB](https://www.mongodb.com/mongodb-architecture)나 [CouchDB](https://blog.couchdb.org/2016/08/01/couchdb-2-0-architecture/) 같은 일부 문서 저장소는 복잡한 질의를 수행하기 위한 SQL 유사 언어도 제공합니다. [DynamoDB](http://www.read.seas.harvard.edu/~kohler/class/cs239-w08/decandia07dynamo.pdf)는 키-값과 문서를 모두 지원합니다.

문서 저장소는 높은 유연성을 제공하며 가끔씩 변경되는 데이터를 다루는 데 자주 쓰입니다.

##### 출처 및 더 읽을거리: 문서 저장소

* [Document-oriented database](https://en.wikipedia.org/wiki/Document-oriented_database)
* [MongoDB architecture](https://www.mongodb.com/mongodb-architecture)
* [CouchDB architecture](https://blog.couchdb.org/2016/08/01/couchdb-2-0-architecture/)
* [Elasticsearch architecture](https://www.elastic.co/blog/found-elasticsearch-from-the-bottom-up)

#### 와이드 칼럼 저장소

<p align="center">
  <img src="images/n16iOGk.png">
  <br/>
  <i><a href=http://blog.grio.com/2015/11/sql-nosql-a-brief-history.html>출처: SQL & NoSQL, a brief history</a></i>
</p>

> 추상화: 중첩 맵 `ColumnFamily<RowKey, Columns<ColKey, Value, Timestamp>>`

와이드 칼럼 저장소의 기본 데이터 단위는 칼럼(이름/값 쌍)입니다. 칼럼은 칼럼 패밀리(SQL 테이블에 대응)로 묶을 수 있습니다. 슈퍼 칼럼 패밀리는 칼럼 패밀리를 다시 묶습니다. 로우 키로 각 칼럼에 독립적으로 접근할 수 있으며, 같은 로우 키를 가진 칼럼들이 하나의 행을 이룹니다. 각 값에는 버전 관리와 충돌 해결을 위한 타임스탬프가 포함됩니다.

구글이 최초의 와이드 칼럼 저장소로 [Bigtable](http://www.read.seas.harvard.edu/~kohler/class/cs239-w08/chang06bigtable.pdf)을 발표했고, 이는 하둡 생태계에서 자주 쓰이는 오픈소스 [HBase](https://www.edureka.co/blog/hbase-architecture/)와 페이스북의 [Cassandra](http://docs.datastax.com/en/cassandra/3.0/cassandra/architecture/archIntro.html)에 영향을 주었습니다. BigTable, HBase, Cassandra 같은 저장소는 키를 사전식 순서로 유지하여 선택적인 키 범위를 효율적으로 조회할 수 있습니다.

와이드 칼럼 저장소는 높은 가용성과 높은 확장성을 제공합니다. 매우 큰 데이터셋에 자주 쓰입니다.

##### 출처 및 더 읽을거리: 와이드 칼럼 저장소

* [SQL & NoSQL, a brief history](http://blog.grio.com/2015/11/sql-nosql-a-brief-history.html)
* [Bigtable architecture](http://www.read.seas.harvard.edu/~kohler/class/cs239-w08/chang06bigtable.pdf)
* [HBase architecture](https://www.edureka.co/blog/hbase-architecture/)
* [Cassandra architecture](http://docs.datastax.com/en/cassandra/3.0/cassandra/architecture/archIntro.html)

#### 그래프 데이터베이스

<p align="center">
  <img src="images/fNcl65g.png">
  <br/>
  <i><a href=https://en.wikipedia.org/wiki/File:GraphDatabase_PropertyGraph.png>출처: Graph database</a></i>
</p>

> 추상화: 그래프

그래프 데이터베이스에서 각 노드는 레코드이고, 각 간선(arc)은 두 노드 사이의 관계입니다. 그래프 데이터베이스는 외래 키가 많거나 다대다 관계가 많은 복잡한 관계를 표현하는 데 최적화되어 있습니다.

그래프 데이터베이스는 소셜 네트워크처럼 관계가 복잡한 데이터 모델에서 높은 성능을 제공합니다. 비교적 새로운 기술이라 아직 널리 쓰이지는 않으며, 개발 도구나 자료를 찾기가 더 어려울 수 있습니다. 많은 그래프 DB는 [REST API](#restrepresentational-state-transfer)로만 접근할 수 있습니다.

##### 출처 및 더 읽을거리: 그래프

* [Graph database](https://en.wikipedia.org/wiki/Graph_database)
* [Neo4j](https://neo4j.com/)
* [FlockDB](https://blog.twitter.com/2010/introducing-flockdb)

#### 출처 및 더 읽을거리: NoSQL

* [Explanation of base terminology](http://stackoverflow.com/questions/3342497/explanation-of-base-terminology)
* [NoSQL databases a survey and decision guidance](https://medium.com/baqend-blog/nosql-databases-a-survey-and-decision-guidance-ea7823a822d#.wskogqenq)
* [Scalability](https://web.archive.org/web/20220602114024/https://www.lecloud.net/post/7994751381/scalability-for-dummies-part-2-database)
* [Introduction to NoSQL](https://www.youtube.com/watch?v=qI_g07C_Q5I)
* [NoSQL patterns](http://horicky.blogspot.com/2009/11/nosql-patterns.html)

### SQL이냐 NoSQL이냐

<p align="center">
  <img src="images/wXGqG5f.png">
  <br/>
  <i><a href=https://www.infoq.com/articles/Transition-RDBMS-NoSQL/>출처: Transitioning from RDBMS to NoSQL</a></i>
</p>

**SQL**을 선택하는 이유:

* 구조화된 데이터
* 엄격한 스키마
* 관계형 데이터
* 복잡한 조인이 필요함
* 트랜잭션
* 확장에 대한 명확한 패턴
* 더 성숙함: 개발자, 커뮤니티, 코드, 도구 등
* 인덱스를 통한 조회가 매우 빠름

**NoSQL**을 선택하는 이유:

* 반정형 데이터
* 동적이거나 유연한 스키마
* 비관계형 데이터
* 복잡한 조인이 필요 없음
* 수 TB(또는 PB) 규모의 데이터 저장
* 매우 데이터 집약적인 워크로드
* IOPS에 대한 매우 높은 처리량

NoSQL에 잘 맞는 데이터 예:

* 클릭스트림과 로그 데이터의 빠른 수집
* 리더보드나 점수 데이터
* 장바구니처럼 임시적인 데이터
* 자주 접근되는('핫') 테이블
* 메타데이터/조회 테이블

##### 출처 및 더 읽을거리: SQL이냐 NoSQL이냐

* [Scaling up to your first 10 million users](https://www.youtube.com/watch?v=kKjm4ehYiMs)
* [SQL vs NoSQL differences](https://www.sitepoint.com/sql-vs-nosql-differences/)

## 캐시

<p align="center">
  <img src="images/Q6z24La.png">
  <br/>
  <i><a href=http://horicky.blogspot.com/2010/10/scalable-system-design-patterns.html>출처: Scalable system design patterns</a></i>
</p>

캐싱은 페이지 로드 시간을 개선하고 서버와 데이터베이스의 부하를 줄여줍니다. 이 모델에서 디스패처는 실제 실행을 아끼기 위해, 해당 요청이 이전에 있었는지 먼저 조회하고 이전 결과를 찾아 반환하려 시도합니다.

데이터베이스는 파티션 전반에 읽기·쓰기가 고르게 분포될 때 이득을 봅니다. 인기 있는 항목은 이 분포를 치우치게 하여 병목을 유발합니다. 데이터베이스 앞에 캐시를 두면 고르지 않은 부하와 트래픽 급증을 흡수하는 데 도움이 됩니다.

### 클라이언트 캐싱

캐시는 클라이언트 측(OS 또는 브라우저), [서버 측](#리버스-프록시웹-서버), 또는 별도의 캐시 계층에 위치할 수 있습니다.

### CDN 캐싱

[CDN](#콘텐츠-전송-네트워크cdn)도 캐시의 한 종류로 봅니다.

### 웹 서버 캐싱

[리버스 프록시](#리버스-프록시웹-서버)와 [Varnish](https://www.varnish-cache.org/) 같은 캐시는 정적·동적 콘텐츠를 직접 제공할 수 있습니다. 웹 서버도 요청을 캐시하여 애플리케이션 서버에 접속하지 않고 응답을 반환할 수 있습니다.

### 데이터베이스 캐싱

데이터베이스는 보통 일반적인 유스케이스에 최적화된 기본 설정으로 어느 정도의 캐싱을 포함합니다. 이 설정을 특정 사용 패턴에 맞게 조정하면 성능을 더 높일 수 있습니다.

### 애플리케이션 캐싱

Memcached, Redis 같은 인메모리 캐시는 애플리케이션과 데이터 저장소 사이에 위치한 키-값 저장소입니다. 데이터를 RAM에 보관하므로, 데이터를 디스크에 저장하는 일반적인 데이터베이스보다 훨씬 빠릅니다. RAM은 디스크보다 용량이 제한적이므로 [LRU(least recently used)](https://en.wikipedia.org/wiki/Cache_replacement_policies#Least_recently_used_(LRU)) 같은 [캐시 무효화](https://en.wikipedia.org/wiki/Cache_algorithms) 알고리즘으로 '차가운' 항목을 무효화하고 '뜨거운' 데이터를 RAM에 유지합니다.

Redis에는 다음과 같은 추가 기능이 있습니다.

* 영속화(persistence) 옵션
* 정렬 집합(sorted set), 리스트 같은 내장 자료구조

캐시할 수 있는 수준은 여러 가지이며 크게 두 범주로 나뉩니다. **데이터베이스 쿼리**와 **객체**입니다.

* 행(row) 수준
* 쿼리 수준
* 완전히 구성된 직렬화 가능한 객체
* 완전히 렌더링된 HTML

일반적으로 파일 기반 캐싱은 피하는 것이 좋습니다. 서버 복제와 오토 스케일링이 어려워지기 때문입니다.

### 데이터베이스 쿼리 수준의 캐싱

데이터베이스에 질의할 때마다 쿼리를 해시하여 키로 삼고 결과를 캐시에 저장합니다. 이 방식은 만료 문제를 겪습니다.

* 복잡한 쿼리는 캐시된 결과를 지우기 어렵습니다.
* 테이블의 셀 하나처럼 데이터 한 조각이 바뀌면, 그 셀을 포함할 수 있는 모든 캐시된 쿼리를 지워야 합니다.

### 객체 수준의 캐싱

애플리케이션 코드에서처럼 데이터를 객체로 바라보세요. 애플리케이션이 데이터베이스의 데이터셋을 클래스 인스턴스나 자료구조로 조립하게 합니다.

* 기반 데이터가 변경되면 캐시에서 객체를 제거합니다.
* 비동기 처리가 가능해집니다. 워커가 가장 최근에 캐시된 객체를 소비하여 객체를 조립합니다.

캐시하면 좋은 것들:

* 사용자 세션
* 완전히 렌더링된 웹 페이지
* 활동 스트림(activity stream)
* 사용자 그래프 데이터

### 캐시를 갱신하는 시점

캐시에는 제한된 양의 데이터만 저장할 수 있으므로, 자신의 유스케이스에 가장 잘 맞는 캐시 갱신 전략을 정해야 합니다.

#### 캐시 어사이드(Cache-aside)

<p align="center">
  <img src="images/ONjORqk.png">
  <br/>
  <i><a href=http://www.slideshare.net/tmatyashovsky/from-cache-to-in-memory-data-grid-introduction-to-hazelcast>출처: From cache to in-memory data grid</a></i>
</p>

애플리케이션이 저장소에서 읽고 쓰는 책임을 집니다. 캐시는 저장소와 직접 상호작용하지 않습니다. 애플리케이션은 다음을 수행합니다.

* 캐시에서 항목을 찾음 → 캐시 미스 발생
* 데이터베이스에서 항목을 로드
* 캐시에 항목을 추가
* 항목을 반환

```python
def get_user(self, user_id):
    user = cache.get("user.{0}", user_id)
    if user is None:
        user = db.query("SELECT * FROM users WHERE user_id = {0}", user_id)
        if user is not None:
            key = "user.{0}".format(user_id)
            cache.set(key, json.dumps(user))
    return user
```

[Memcached](https://memcached.org/)는 일반적으로 이 방식으로 사용됩니다.

캐시에 추가된 데이터를 이후에 읽는 것은 빠릅니다. 캐시 어사이드는 지연 로딩(lazy loading)이라고도 합니다. 요청된 데이터만 캐시되므로 요청되지 않는 데이터로 캐시가 채워지는 것을 막습니다.

##### 단점: 캐시 어사이드

* 캐시 미스마다 세 번의 왕복이 발생해 눈에 띄는 지연이 생길 수 있습니다.
* 데이터베이스에서 값이 갱신되면 캐시 데이터가 오래된 것이 될 수 있습니다. TTL을 설정해 캐시 항목 갱신을 강제하거나 라이트 스루를 사용하면 완화됩니다.
* 노드가 죽으면 비어 있는 새 노드로 교체되어 지연이 증가합니다.

#### 라이트 스루(Write-through)

<p align="center">
  <img src="images/0vBc0hN.png">
  <br/>
  <i><a href=http://www.slideshare.net/jboner/scalability-availability-stability-patterns/>출처: Scalability, availability, stability, patterns</a></i>
</p>

애플리케이션은 캐시를 주 데이터 저장소처럼 사용하여 캐시에 읽고 씁니다. 데이터베이스에 읽고 쓰는 책임은 캐시가 집니다.

* 애플리케이션이 캐시에 항목을 추가/갱신
* 캐시가 데이터 저장소에 동기적으로 항목을 기록
* 반환

애플리케이션 코드:

```python
set_user(12345, {"foo":"bar"})
```

캐시 코드:

```python
def set_user(user_id, values):
    user = db.query("UPDATE Users WHERE id = {0}", user_id, values)
    cache.set(user_id, user)
```

라이트 스루는 쓰기 연산 때문에 전체적으로는 느리지만, 방금 쓴 데이터를 이후에 읽는 것은 빠릅니다. 일반적으로 사용자는 데이터를 읽을 때보다 갱신할 때 지연에 더 관대합니다. 캐시의 데이터가 오래되지 않습니다.

##### 단점: 라이트 스루

* 장애나 확장으로 새 노드가 생기면, 데이터베이스에서 해당 항목이 갱신되기 전까지 새 노드는 그 항목을 캐시하지 않습니다. 캐시 어사이드를 라이트 스루와 함께 쓰면 이 문제를 완화할 수 있습니다.
* 기록된 데이터 대부분이 한 번도 읽히지 않을 수 있는데, TTL로 최소화할 수 있습니다.

#### 라이트 비하인드(Write-behind, write-back)

<p align="center">
  <img src="images/rgSrvjG.png">
  <br/>
  <i><a href=http://www.slideshare.net/jboner/scalability-availability-stability-patterns/>출처: Scalability, availability, stability, patterns</a></i>
</p>

라이트 비하인드에서 애플리케이션은 다음을 수행합니다.

* 캐시에 항목을 추가/갱신
* 데이터 저장소에 비동기적으로 항목을 기록하여 쓰기 성능을 향상

##### 단점: 라이트 비하인드

* 캐시 내용이 데이터 저장소에 반영되기 전에 캐시가 죽으면 데이터가 유실될 수 있습니다.
* 캐시 어사이드나 라이트 스루보다 구현이 복잡합니다.

#### 리프레시 어헤드(Refresh-ahead)

<p align="center">
  <img src="images/kxtjqgE.png">
  <br/>
  <i><a href=http://www.slideshare.net/tmatyashovsky/from-cache-to-in-memory-data-grid-introduction-to-hazelcast>출처: From cache to in-memory data grid</a></i>
</p>

최근에 접근된 캐시 항목이 만료되기 전에 자동으로 갱신되도록 캐시를 설정할 수 있습니다.

캐시가 앞으로 필요할 항목을 정확히 예측할 수 있다면, 리프레시 어헤드는 리드 스루(read-through)보다 지연을 줄일 수 있습니다.

##### 단점: 리프레시 어헤드

* 앞으로 필요한 항목을 정확히 예측하지 못하면 리프레시 어헤드를 쓰지 않을 때보다 성능이 떨어질 수 있습니다.

### 단점: 캐시

* [캐시 무효화](https://en.wikipedia.org/wiki/Cache_algorithms)를 통해 캐시와 데이터베이스 같은 신뢰 원본(source of truth) 사이의 일관성을 유지해야 합니다.
* 캐시 무효화는 어려운 문제이며, 캐시를 언제 갱신할지에 대한 복잡성이 추가됩니다.
* Redis나 memcached를 추가하는 등 애플리케이션 변경이 필요합니다.

### 출처 및 더 읽을거리

* [From cache to in-memory data grid](http://www.slideshare.net/tmatyashovsky/from-cache-to-in-memory-data-grid-introduction-to-hazelcast)
* [Scalable system design patterns](http://horicky.blogspot.com/2010/10/scalable-system-design-patterns.html)
* [Introduction to architecting systems for scale](http://lethain.com/introduction-to-architecting-systems-for-scale/)
* [Scalability, availability, stability, patterns](http://www.slideshare.net/jboner/scalability-availability-stability-patterns/)
* [Scalability](https://web.archive.org/web/20230126233752/https://www.lecloud.net/post/9246290032/scalability-for-dummies-part-3-cache)
* [AWS ElastiCache strategies](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/Strategies.html)
* [Wikipedia](https://en.wikipedia.org/wiki/Cache_(computing))

## 비동기

<p align="center">
  <img src="images/54GYsSx.png">
  <br/>
  <i><a href=http://lethain.com/introduction-to-architecting-systems-for-scale/#platform_layer>출처: Intro to architecting systems for scale</a></i>
</p>

비동기 워크플로는 그대로 두면 인라인으로 수행되었을 비용이 큰 연산의 요청 시간을 줄여줍니다. 또한 데이터의 주기적 집계처럼 시간이 오래 걸리는 작업을 미리 해두는 데도 도움이 됩니다.

### 메시지 큐

메시지 큐는 메시지를 받고, 보관하고, 전달합니다. 어떤 연산이 인라인으로 수행하기에 너무 느리다면 다음과 같은 워크플로로 메시지 큐를 사용할 수 있습니다.

* 애플리케이션이 큐에 작업을 발행하고 사용자에게 작업 상태를 알림
* 워커가 큐에서 작업을 가져와 처리한 뒤 완료를 알림

사용자는 대기하지 않고, 작업은 백그라운드에서 처리됩니다. 이 시간 동안 클라이언트는 작업이 완료된 것처럼 보이도록 약간의 처리를 선택적으로 수행할 수 있습니다. 예를 들어 트윗을 올리는 경우, 트윗은 즉시 내 타임라인에 표시되지만 실제로 모든 팔로워에게 전달되기까지는 시간이 걸릴 수 있습니다.

**[Redis](https://redis.io/)**는 간단한 메시지 브로커로 유용하지만 메시지가 유실될 수 있습니다.

**[RabbitMQ](https://www.rabbitmq.com/)**는 인기가 많지만 'AMQP' 프로토콜에 적응해야 하고 노드를 직접 관리해야 합니다.

**[Amazon SQS](https://aws.amazon.com/sqs/)**는 호스팅형이지만 지연이 클 수 있고 메시지가 두 번 전달될 가능성이 있습니다.

### 태스크 큐

태스크 큐는 태스크와 관련 데이터를 받아 실행하고 결과를 전달합니다. 스케줄링을 지원할 수 있으며 계산 집약적인 작업을 백그라운드에서 실행하는 데 쓸 수 있습니다.

**[Celery](https://docs.celeryproject.org/en/stable/)**는 스케줄링을 지원하며 주로 파이썬을 지원합니다.

### 배압(Back pressure)

큐가 크게 늘어나기 시작하면 큐 크기가 메모리보다 커져 캐시 미스, 디스크 읽기, 나아가 더 느린 성능으로 이어집니다. [배압](http://mechanical-sympathy.blogspot.com/2012/05/apply-back-pressure-when-overloaded.html)은 큐 크기를 제한해 높은 처리량과 이미 큐에 있는 작업에 대한 좋은 응답 시간을 유지하도록 도와줍니다. 큐가 가득 차면 클라이언트는 서버 사용 중(server busy)이나 HTTP 503 상태 코드를 받고 나중에 다시 시도합니다. 클라이언트는 [지수 백오프](https://en.wikipedia.org/wiki/Exponential_backoff)를 적용해 나중에 요청을 재시도할 수 있습니다.

### 단점: 비동기

* 비용이 적은 계산이나 실시간 워크플로 같은 유스케이스는 동기 연산이 더 적합할 수 있습니다. 큐를 도입하면 지연과 복잡성이 늘어나기 때문입니다.

### 출처 및 더 읽을거리

* [It's all a numbers game](https://www.youtube.com/watch?v=1KRYH75wgy4)
* [Applying back pressure when overloaded](http://mechanical-sympathy.blogspot.com/2012/05/apply-back-pressure-when-overloaded.html)
* [Little's law](https://en.wikipedia.org/wiki/Little%27s_law)
* [What is the difference between a message queue and a task queue?](https://www.quora.com/What-is-the-difference-between-a-message-queue-and-a-task-queue-Why-would-a-task-queue-require-a-message-broker-like-RabbitMQ-Redis-Celery-or-IronMQ-to-function)

## 통신

<p align="center">
  <img src="images/5KeocQs.jpg">
  <br/>
  <i><a href=http://www.escotal.com/osilayer.html>출처: OSI 7 layer model</a></i>
</p>

### 하이퍼텍스트 전송 프로토콜(HTTP)

HTTP는 클라이언트와 서버 사이에서 데이터를 인코딩하고 전송하는 방법입니다. 요청/응답 프로토콜로, 클라이언트가 요청을 보내면 서버는 관련 콘텐츠와 요청 처리 완료 상태 정보를 담은 응답을 보냅니다. HTTP는 자기 완결적(self-contained)이어서, 요청과 응답이 로드 밸런싱·캐싱·암호화·압축을 수행하는 여러 중간 라우터와 서버를 거쳐 흐를 수 있습니다.

기본적인 HTTP 요청은 동사(메서드)와 리소스(엔드포인트)로 구성됩니다. 아래는 흔히 쓰이는 HTTP 동사입니다.

| 동사 | 설명 | 멱등성* | 안전 | 캐시 가능 |
|---|---|---|---|---|
| GET | 리소스를 읽음 | 예 | 예 | 예 |
| POST | 리소스를 생성하거나 데이터를 처리하는 프로세스를 트리거함 | 아니오 | 아니오 | 응답에 신선도 정보가 있으면 가능 |
| PUT | 리소스를 생성하거나 교체함 | 예 | 아니오 | 아니오 |
| PATCH | 리소스를 부분적으로 갱신함 | 아니오 | 아니오 | 응답에 신선도 정보가 있으면 가능 |
| DELETE | 리소스를 삭제함 | 예 | 아니오 | 아니오 |

*여러 번 호출해도 결과가 달라지지 않음.

HTTP는 **TCP**, **UDP** 같은 하위 프로토콜에 의존하는 애플리케이션 계층 프로토콜입니다.

#### 출처 및 더 읽을거리: HTTP

* [What is HTTP?](https://www.nginx.com/resources/glossary/http/)
* [Difference between HTTP and TCP](https://www.quora.com/What-is-the-difference-between-HTTP-protocol-and-TCP-protocol)
* [Difference between PUT and PATCH](https://laracasts.com/discuss/channels/general-discussion/whats-the-differences-between-put-and-patch?page=1)

### 전송 제어 프로토콜(TCP)

<p align="center">
  <img src="images/JdAsdvG.jpg">
  <br/>
  <i><a href=http://www.wildbunny.co.uk/blog/2012/10/09/how-to-make-a-multi-player-game-part-1/>출처: How to make a multiplayer game</a></i>
</p>

TCP는 [IP 네트워크](https://en.wikipedia.org/wiki/Internet_Protocol) 위에서 동작하는 연결 지향 프로토콜입니다. 연결은 [핸드셰이크](https://en.wikipedia.org/wiki/Handshaking)로 수립되고 종료됩니다. 전송된 모든 패킷은 다음을 통해 원래 순서대로 손상 없이 목적지에 도착하는 것이 보장됩니다.

* 각 패킷의 시퀀스 번호와 [체크섬 필드](https://en.wikipedia.org/wiki/Transmission_Control_Protocol#Checksum_computation)
* [확인 응답(ACK)](https://en.wikipedia.org/wiki/Acknowledgement_(data_networks)) 패킷과 자동 재전송

송신자가 올바른 응답을 받지 못하면 패킷을 재전송합니다. 타임아웃이 여러 번 발생하면 연결이 끊깁니다. TCP는 [흐름 제어](https://en.wikipedia.org/wiki/Flow_control_(data))와 [혼잡 제어](https://en.wikipedia.org/wiki/Network_congestion#Congestion_control)도 구현합니다. 이러한 보장은 지연을 유발하며 일반적으로 UDP보다 전송 효율이 떨어집니다.

높은 처리량을 위해 웹 서버는 많은 수의 TCP 연결을 열어 둘 수 있는데, 이는 메모리 사용량을 크게 만듭니다. 웹 서버 스레드와 [memcached](https://memcached.org/) 서버 사이에 많은 연결을 열어 두는 것은 비용이 큽니다. [커넥션 풀링](https://en.wikipedia.org/wiki/Connection_pool)이 도움이 되며, 가능한 곳에서는 UDP로 전환하는 것도 방법입니다.

TCP는 높은 신뢰성이 필요하지만 시간에 덜 민감한 애플리케이션에 유용합니다. 웹 서버, 데이터베이스 정보, SMTP, FTP, SSH 등이 그 예입니다.

다음의 경우 UDP 대신 TCP를 사용하세요.

* 모든 데이터가 온전히 도착해야 할 때
* 네트워크 처리량을 자동으로 최대한 활용하고 싶을 때

### 사용자 데이터그램 프로토콜(UDP)

<p align="center">
  <img src="images/yzDrJtA.jpg">
  <br/>
  <i><a href=http://www.wildbunny.co.uk/blog/2012/10/09/how-to-make-a-multi-player-game-part-1/>출처: How to make a multiplayer game</a></i>
</p>

UDP는 비연결형입니다. 데이터그램(패킷에 해당)은 데이터그램 수준에서만 보장됩니다. 데이터그램은 순서가 뒤바뀌어 도착하거나 아예 도착하지 않을 수 있습니다. UDP는 혼잡 제어를 지원하지 않습니다. TCP가 제공하는 보장이 없는 대신 UDP는 일반적으로 더 효율적입니다.

UDP는 브로드캐스트가 가능하여 서브넷의 모든 장치에 데이터그램을 보낼 수 있습니다. 이는 [DHCP](https://en.wikipedia.org/wiki/Dynamic_Host_Configuration_Protocol)에서 유용합니다. 클라이언트가 아직 IP 주소를 받지 못한 상태라 IP 주소 없이는 TCP로 스트리밍할 방법이 없기 때문입니다.

UDP는 신뢰성이 낮지만 VoIP, 영상 채팅, 스트리밍, 실시간 멀티플레이어 게임 같은 실시간 유스케이스에 적합합니다.

다음의 경우 TCP 대신 UDP를 사용하세요.

* 지연을 최소로 해야 할 때
* 늦게 도착한 데이터가 유실된 데이터보다 나쁠 때
* 오류 정정을 직접 구현하고 싶을 때

#### 출처 및 더 읽을거리: TCP와 UDP

* [Networking for game programming](https://gafferongames.com/post/udp_vs_tcp/)
* [Key differences between TCP and UDP protocols](http://www.cyberciti.biz/faq/key-differences-between-tcp-and-udp-protocols/)
* [Difference between TCP and UDP](http://stackoverflow.com/questions/5970383/difference-between-tcp-and-udp)
* [Transmission control protocol](https://en.wikipedia.org/wiki/Transmission_Control_Protocol)
* [User datagram protocol](https://en.wikipedia.org/wiki/User_Datagram_Protocol)
* [Scaling memcache at Facebook](http://www.cs.bu.edu/~jappavoo/jappavoo.github.com/451/papers/memcache-fb.pdf)

### 원격 프로시저 호출(RPC)

<p align="center">
  <img src="images/iF4Mkb5.png">
  <br/>
  <i><a href=http://www.puncsky.com/blog/2016-02-13-crack-the-system-design-interview>출처: Crack the system design interview</a></i>
</p>

RPC에서 클라이언트는 다른 주소 공간(보통 원격 서버)에서 프로시저가 실행되도록 합니다. 프로시저는 마치 로컬 프로시저 호출인 것처럼 작성되어, 서버와 통신하는 세부 사항을 클라이언트 프로그램으로부터 추상화합니다. 원격 호출은 보통 로컬 호출보다 느리고 신뢰성이 낮으므로, RPC 호출과 로컬 호출을 구분하는 것이 도움이 됩니다. 널리 쓰이는 RPC 프레임워크로는 [Protobuf](https://developers.google.com/protocol-buffers/), [Thrift](https://thrift.apache.org/), [Avro](https://avro.apache.org/docs/current/)가 있습니다.

RPC는 요청-응답 프로토콜입니다.

* **클라이언트 프로그램** - 클라이언트 스텁 프로시저를 호출합니다. 매개변수는 로컬 프로시저 호출처럼 스택에 쌓입니다.
* **클라이언트 스텁 프로시저** - 프로시저 id와 인자를 요청 메시지로 마샬링(포장)합니다.
* **클라이언트 통신 모듈** - OS가 클라이언트에서 서버로 메시지를 보냅니다.
* **서버 통신 모듈** - OS가 들어온 패킷을 서버 스텁 프로시저에 넘깁니다.
* **서버 스텁 프로시저** - 결과를 언마샬링하고, 프로시저 id에 맞는 서버 프로시저를 주어진 인자로 호출합니다.
* 서버 응답은 위 단계를 역순으로 반복합니다.

RPC 호출 예:

```
GET /someoperation?data=anId

POST /anotheroperation
{
  "data":"anId";
  "anotherdata": "another value"
}
```

RPC는 동작(behavior)을 노출하는 데 초점을 둡니다. 유스케이스에 맞춰 네이티브 호출을 직접 정교하게 만들 수 있으므로, 성능상의 이유로 내부 통신에 자주 쓰입니다.

다음의 경우 네이티브 라이브러리(SDK)를 선택하세요.

* 대상 플랫폼을 알고 있을 때
* "로직"에 어떻게 접근할지 통제하고 싶을 때
* 라이브러리 외부에서 오류 제어가 어떻게 일어날지 통제하고 싶을 때
* 성능과 최종 사용자 경험이 최우선 관심사일 때

공개 API에는 **REST**를 따르는 HTTP API가 더 자주 쓰이는 경향이 있습니다.

#### 단점: RPC

* RPC 클라이언트가 서비스 구현에 강하게 결합됩니다.
* 새로운 연산이나 유스케이스마다 새 API를 정의해야 합니다.
* RPC는 디버깅이 어려울 수 있습니다.
* 기존 기술을 그대로 활용하지 못할 수 있습니다. 예를 들어 [Squid](http://www.squid-cache.org/) 같은 캐싱 서버에서 [RPC 호출이 제대로 캐시되도록](https://web.archive.org/web/20170608193645/http://etherealbits.com/2012/12/debunking-the-myths-of-rpc-rest/) 하려면 추가적인 노력이 필요할 수 있습니다.

### REST(Representational state transfer)

REST는 클라이언트/서버 모델을 강제하는 아키텍처 스타일로, 클라이언트는 서버가 관리하는 리소스 집합에 대해 동작합니다. 서버는 리소스의 표현(representation)과, 리소스를 조작하거나 새로운 표현을 얻을 수 있는 동작을 제공합니다. 모든 통신은 무상태이며 캐시 가능해야 합니다.

RESTful 인터페이스에는 네 가지 특성이 있습니다.

* **리소스 식별(HTTP에서는 URI)** - 어떤 연산이든 동일한 URI를 사용합니다.
* **표현을 통한 변경(HTTP에서는 동사)** - 동사, 헤더, 본문을 사용합니다.
* **자기 서술적 오류 메시지(HTTP에서는 상태 응답)** - 상태 코드를 사용하고, 바퀴를 다시 발명하지 마세요.
* **[HATEOAS](http://restcookbook.com/Basics/hateoas/)(HTTP에서는 HTML 인터페이스)** - 웹 서비스는 브라우저에서 온전히 접근 가능해야 합니다.

REST 호출 예:

```
GET /someresources/anId

PUT /someresources/anId
{"anotherdata": "another value"}
```

REST는 데이터를 노출하는 데 초점을 둡니다. 클라이언트와 서버 사이의 결합을 최소화하며 공개 HTTP API에 자주 쓰입니다. REST는 URI로 리소스를 노출하고, [헤더를 통해 표현](https://github.com/for-GET/know-your-http-well/blob/master/headers.md)하며, GET·POST·PUT·DELETE·PATCH 같은 동사로 동작을 표현하는 더 일반적이고 일관된 방식을 사용합니다. 무상태이므로 수평 확장과 파티셔닝에 매우 적합합니다.

#### 단점: REST

* REST는 데이터 노출에 초점을 두므로, 리소스가 자연스럽게 조직되지 않거나 단순한 계층으로 접근되지 않는 경우에는 잘 맞지 않을 수 있습니다. 예를 들어 특정 이벤트 집합에 해당하며 지난 한 시간 동안 갱신된 모든 레코드를 반환하는 것은 경로로 표현하기 쉽지 않습니다. REST에서는 URI 경로, 쿼리 파라미터, 경우에 따라 요청 본문을 조합해 구현하게 될 가능성이 높습니다.
* REST는 보통 소수의 동사(GET, POST, PUT, DELETE, PATCH)에 의존하는데, 이것이 유스케이스에 맞지 않을 때가 있습니다. 예를 들어 만료된 문서를 보관 폴더로 옮기는 동작은 이 동사들에 깔끔하게 들어맞지 않을 수 있습니다.
* 중첩된 계층을 가진 복잡한 리소스를 가져오려면 하나의 화면을 그리는 데도 클라이언트와 서버 사이에 여러 번의 왕복이 필요합니다. 예를 들어 블로그 글 내용과 그 글의 댓글을 가져오는 경우가 그렇습니다. 네트워크 상태가 일정치 않은 모바일 애플리케이션에서 이런 다중 왕복은 매우 바람직하지 않습니다.
* 시간이 지나면서 API 응답에 필드가 추가되면, 오래된 클라이언트도 필요 없는 필드까지 모두 받게 되어 페이로드 크기가 커지고 지연이 늘어납니다.

### RPC와 REST 호출 비교

| 연산 | RPC | REST |
|---|---|---|
| 가입    | **POST** /signup | **POST** /persons |
| 탈퇴    | **POST** /resign<br/>{<br/>"personid": "1234"<br/>} | **DELETE** /persons/1234 |
| 사용자 조회 | **GET** /readPerson?personid=1234 | **GET** /persons/1234 |
| 사용자의 아이템 목록 조회 | **GET** /readUsersItemsList?personid=1234 | **GET** /persons/1234/items |
| 사용자의 아이템 목록에 아이템 추가 | **POST** /addItemToUsersItemsList<br/>{<br/>"personid": "1234";<br/>"itemid": "456"<br/>} | **POST** /persons/1234/items<br/>{<br/>"itemid": "456"<br/>} |
| 아이템 갱신    | **POST** /modifyItem<br/>{<br/>"itemid": "456";<br/>"key": "value"<br/>} | **PUT** /items/456<br/>{<br/>"key": "value"<br/>} |
| 아이템 삭제 | **POST** /removeItem<br/>{<br/>"itemid": "456"<br/>} | **DELETE** /items/456 |

<p align="center">
  <i><a href=https://apihandyman.io/do-you-really-know-why-you-prefer-rest-over-rpc/>출처: Do you really know why you prefer REST over RPC</a></i>
</p>

#### 출처 및 더 읽을거리: REST와 RPC

* [Do you really know why you prefer REST over RPC](https://apihandyman.io/do-you-really-know-why-you-prefer-rest-over-rpc/)
* [When are RPC-ish approaches more appropriate than REST?](http://programmers.stackexchange.com/a/181186)
* [REST vs JSON-RPC](http://stackoverflow.com/questions/15056878/rest-vs-json-rpc)
* [Debunking the myths of RPC and REST](https://web.archive.org/web/20170608193645/http://etherealbits.com/2012/12/debunking-the-myths-of-rpc-rest/)
* [What are the drawbacks of using REST](https://www.quora.com/What-are-the-drawbacks-of-using-RESTful-APIs)
* [Crack the system design interview](http://www.puncsky.com/blog/2016-02-13-crack-the-system-design-interview)
* [Thrift](https://code.facebook.com/posts/1468950976659943/)
* [Why REST for internal use and not RPC](http://arstechnica.com/civis/viewtopic.php?t=1190508)

## 보안

이 섹션은 갱신이 필요합니다. [기여](#기여하기)를 고려해 주세요!

보안은 범위가 넓은 주제입니다. 상당한 경험이 있거나, 보안 배경지식이 있거나, 보안 지식이 필요한 직무에 지원하는 것이 아니라면 기본 이상을 알 필요는 없을 것입니다.

* 전송 중(in transit)과 저장 시(at rest) 모두 암호화하세요.
* [XSS](https://en.wikipedia.org/wiki/Cross-site_scripting)와 [SQL 인젝션](https://en.wikipedia.org/wiki/SQL_injection)을 막기 위해 모든 사용자 입력과 사용자에게 노출되는 입력 파라미터를 검증·정제하세요.
* SQL 인젝션을 막기 위해 파라미터화된 쿼리를 사용하세요.
* [최소 권한](https://en.wikipedia.org/wiki/Principle_of_least_privilege) 원칙을 사용하세요.

### 출처 및 더 읽을거리

* [API security checklist](https://github.com/shieldfy/API-Security-Checklist)
* [Security guide for developers](https://github.com/FallibleInc/security-guide-for-developers)
* [OWASP top ten](https://www.owasp.org/index.php/OWASP_Top_Ten_Cheat_Sheet)

## 부록

가끔 '봉투 뒷면' 추정을 해보라는 요청을 받습니다. 예를 들어 디스크에서 이미지 썸네일 100개를 만드는 데 얼마나 걸릴지, 어떤 자료구조가 메모리를 얼마나 차지할지 계산해야 할 수 있습니다. **2의 거듭제곱 표**와 **모든 프로그래머가 알아야 할 지연 시간 수치**는 유용한 참고 자료입니다.

### 2의 거듭제곱 표

```
지수            정확한 값            근사값               바이트
---------------------------------------------------------------
7                             128
8                             256
10                           1024   1천                  1 KB
16                         65,536                       64 KB
20                      1,048,576   1백만                1 MB
30                  1,073,741,824   10억                 1 GB
32                  4,294,967,296                        4 GB
40              1,099,511,627,776   1조                  1 TB
```

#### 출처 및 더 읽을거리

* [Powers of two](https://en.wikipedia.org/wiki/Power_of_two)

### 모든 프로그래머가 알아야 할 지연 시간 수치

```
지연 시간 비교 수치
--------------------------
L1 캐시 참조                                 0.5 ns
분기 예측 실패                               5   ns
L2 캐시 참조                                 7   ns                      L1 캐시의 14배
뮤텍스 lock/unlock                          25   ns
메인 메모리 참조                           100   ns                      L2 캐시의 20배, L1 캐시의 200배
Zippy로 1K 바이트 압축                  10,000   ns       10 us
1 Gbps 네트워크로 1 KB 전송             10,000   ns       10 us
SSD에서 4 KB 임의 읽기*                150,000   ns      150 us          ~1GB/sec SSD
메모리에서 1 MB 순차 읽기              250,000   ns      250 us
같은 데이터센터 내 왕복                500,000   ns      500 us
SSD에서 1 MB 순차 읽기*              1,000,000   ns    1,000 us    1 ms  ~1GB/sec SSD, 메모리의 4배
HDD 탐색(seek)                      10,000,000   ns   10,000 us   10 ms  데이터센터 왕복의 20배
1 Gbps로 1 MB 순차 읽기             10,000,000   ns   10,000 us   10 ms  메모리의 40배, SSD의 10배
HDD에서 1 MB 순차 읽기              30,000,000   ns   30,000 us   30 ms  메모리의 120배, SSD의 30배
캘리포니아→네덜란드→캘리포니아 패킷 150,000,000   ns  150,000 us  150 ms

참고
-----
1 ns = 10^-9 초
1 us = 10^-6 초 = 1,000 ns
1 ms = 10^-3 초 = 1,000 us = 1,000,000 ns
```

위 수치를 바탕으로 한 유용한 지표:

* HDD에서 30 MB/s로 순차 읽기
* 1 Gbps 이더넷에서 100 MB/s로 순차 읽기
* SSD에서 1 GB/s로 순차 읽기
* 메인 메모리에서 4 GB/s로 순차 읽기
* 초당 6~7회의 전 세계 왕복
* 데이터센터 내에서는 초당 2,000회 왕복

#### 지연 시간 수치 시각화

![](https://camo.githubusercontent.com/77f72259e1eb58596b564d1ad823af1853bc60a3/687474703a2f2f692e696d6775722e636f6d2f6b307431652e706e67)

#### 출처 및 더 읽을거리

* [Latency numbers every programmer should know - 1](https://gist.github.com/jboner/2841832)
* [Latency numbers every programmer should know - 2](https://gist.github.com/hellerbarde/2843375)
* [Designs, lessons, and advice from building large distributed systems](http://www.cs.cornell.edu/projects/ladis2009/talks/dean-keynote-ladis2009.pdf)
* [Software Engineering Advice from Building Large-Scale Distributed Systems](https://static.googleusercontent.com/media/research.google.com/en//people/jeff/stanford-295-talk.pdf)

### 추가 시스템 설계 면접 문제

> 자주 나오는 시스템 설계 면접 문제와 각 문제를 푸는 데 도움이 되는 자료 링크입니다.

| 문제 | 참고 자료 |
|---|---|
| 드롭박스 같은 파일 동기화 서비스 설계하기 | [youtube.com](https://www.youtube.com/watch?v=PE4gwstWhmc) |
| 구글 같은 검색 엔진 설계하기 | [queue.acm.org](http://queue.acm.org/detail.cfm?id=988407)<br/>[stackexchange.com](http://programmers.stackexchange.com/questions/38324/interview-question-how-would-you-implement-google-search)<br/>[ardendertat.com](http://www.ardendertat.com/2012/01/11/implementing-search-engines/)<br/>[stanford.edu](http://infolab.stanford.edu/~backrub/google.html) |
| 구글 같은 확장 가능한 웹 크롤러 설계하기 | [quora.com](https://www.quora.com/How-can-I-build-a-web-crawler-from-scratch) |
| 구글 독스 설계하기 | [code.google.com](https://code.google.com/p/google-mobwrite/)<br/>[neil.fraser.name](https://neil.fraser.name/writing/sync/) |
| Redis 같은 키-값 저장소 설계하기 | [codecapsule.com](http://codecapsule.com/2012/11/07/ikvs-implementing-a-key-value-store-table-of-contents/)<br/>[allthingsdistributed.com](https://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf) |
| Memcached 같은 캐시 시스템 설계하기 | [slideshare.net](http://www.slideshare.net/oemebamo/introduction-to-memcached) |
| 아마존 같은 추천 시스템 설계하기 | [hulu.com](https://web.archive.org/web/20170406065247/http://tech.hulu.com/blog/2011/09/19/recommendation-system.html)<br/>[ijcai13.org](http://ijcai13.org/files/tutorial_slides/td3.pdf) |
| Bitly 같은 tinyurl 시스템 설계하기 | [n00tc0d3r.blogspot.com](http://n00tc0d3r.blogspot.com/) |
| WhatsApp 같은 채팅 앱 설계하기 | [highscalability.com](http://highscalability.com/blog/2014/2/26/the-whatsapp-architecture-facebook-bought-for-19-billion.html)
| 인스타그램 같은 사진 공유 시스템 설계하기 | [highscalability.com](http://highscalability.com/flickr-architecture)<br/>[highscalability.com](http://highscalability.com/blog/2011/12/6/instagram-architecture-14-million-users-terabytes-of-photos.html) |
| 페이스북 뉴스피드 기능 설계하기 | [quora.com](http://www.quora.com/What-are-best-practices-for-building-something-like-a-News-Feed)<br/>[quora.com](http://www.quora.com/Activity-Streams/What-are-the-scaling-issues-to-keep-in-mind-while-developing-a-social-network-feed)<br/>[slideshare.net](http://www.slideshare.net/danmckinley/etsy-activity-feeds-architecture) |
| 페이스북 타임라인 기능 설계하기 | [facebook.com](https://www.facebook.com/note.php?note_id=10150468255628920)<br/>[highscalability.com](http://highscalability.com/blog/2012/1/23/facebook-timeline-brought-to-you-by-the-power-of-denormaliza.html) |
| 페이스북 채팅 기능 설계하기 | [erlang-factory.com](http://www.erlang-factory.com/upload/presentations/31/EugeneLetuchy-ErlangatFacebook.pdf)<br/>[facebook.com](https://www.facebook.com/note.php?note_id=14218138919&id=9445547199&index=0) |
| 페이스북 같은 그래프 검색 기능 설계하기 | [facebook.com](https://www.facebook.com/notes/facebook-engineering/under-the-hood-building-out-the-infrastructure-for-graph-search/10151347573598920)<br/>[facebook.com](https://www.facebook.com/notes/facebook-engineering/under-the-hood-indexing-and-ranking-in-graph-search/10151361720763920)<br/>[facebook.com](https://www.facebook.com/notes/facebook-engineering/under-the-hood-the-natural-language-interface-of-graph-search/10151432733048920) |
| CloudFlare 같은 콘텐츠 전송 네트워크 설계하기 | [figshare.com](https://figshare.com/articles/Globally_distributed_content_delivery/6605972) |
| 트위터 같은 트렌딩 토픽 시스템 설계하기 | [michael-noll.com](http://www.michael-noll.com/blog/2013/01/18/implementing-real-time-trending-topics-in-storm/)<br/>[snikolov .wordpress.com](http://snikolov.wordpress.com/2012/11/14/early-detection-of-twitter-trends/) |
| 임의 ID 생성 시스템 설계하기 | [blog.twitter.com](https://blog.twitter.com/2010/announcing-snowflake)<br/>[github.com](https://github.com/twitter/snowflake/) |
| 특정 시간 구간의 상위 k개 요청 반환하기 | [cs.ucsb.edu](https://www.cs.ucsb.edu/sites/default/files/documents/2005-23.pdf)<br/>[wpi.edu](http://davis.wpi.edu/xmdv/docs/EDBT11-diyang.pdf) |
| 여러 데이터센터에서 데이터를 제공하는 시스템 설계하기 | [highscalability.com](http://highscalability.com/blog/2009/8/24/how-google-serves-data-from-multiple-datacenters.html) |
| 온라인 멀티플레이어 카드 게임 설계하기 | [indieflashblog.com](https://web.archive.org/web/20180929181117/http://www.indieflashblog.com/how-to-create-an-asynchronous-multiplayer-game.html)<br/>[buildnewgames.com](http://buildnewgames.com/real-time-multiplayer/) |
| 가비지 컬렉션 시스템 설계하기 | [stuffwithstuff.com](http://journal.stuffwithstuff.com/2013/12/08/babys-first-garbage-collector/)<br/>[washington.edu](http://courses.cs.washington.edu/courses/csep521/07wi/prj/rick.pdf) |
| API 레이트 리미터 설계하기 | [https://stripe.com/blog/](https://stripe.com/blog/rate-limiters) |
| 증권 거래소(NASDAQ이나 Binance 같은) 설계하기 | [Jane Street](https://youtu.be/b1e4t2k2KJY)<br/>[Golang Implementation](https://around25.com/blog/building-a-trading-engine-for-a-crypto-exchange/)<br/>[Go Implementation](http://bhomnick.net/building-a-simple-limit-order-in-go/) |
| 시스템 설계 문제 추가하기 | [기여하기](#기여하기) |

### 실제 아키텍처 사례

> 실제 시스템이 어떻게 설계되었는지에 대한 글들입니다.

<p align="center">
  <img src="images/TcUo2fw.png">
  <br/>
  <i><a href=https://www.infoq.com/presentations/Twitter-Timeline-Scalability>출처: Twitter timelines at scale</a></i>
</p>

**아래 글들의 세세한 부분에 집중하기보다는 다음에 초점을 맞추세요.**

* 이 글들에 공통적으로 나타나는 원칙, 기술, 패턴을 파악하기
* 각 컴포넌트가 어떤 문제를 해결하는지, 어디에서 잘 동작하고 어디에서 그렇지 않은지 공부하기
* 배운 교훈을 되짚어 보기

|유형 | 시스템 | 참고 자료 |
|---|---|---|
| 데이터 처리 | **MapReduce** - 구글의 분산 데이터 처리 | [research.google.com](http://static.googleusercontent.com/media/research.google.com/zh-CN/us/archive/mapreduce-osdi04.pdf) |
| 데이터 처리 | **Spark** - Databricks의 분산 데이터 처리 | [slideshare.net](http://www.slideshare.net/AGrishchenko/apache-spark-architecture) |
| 데이터 처리 | **Storm** - 트위터의 분산 데이터 처리 | [slideshare.net](http://www.slideshare.net/previa/storm-16094009) |
| | | |
| 데이터 저장소 | **Bigtable** - 구글의 분산 칼럼 지향 데이터베이스 | [harvard.edu](http://www.read.seas.harvard.edu/~kohler/class/cs239-w08/chang06bigtable.pdf) |
| 데이터 저장소 | **HBase** - Bigtable의 오픈소스 구현 | [slideshare.net](http://www.slideshare.net/alexbaranau/intro-to-hbase) |
| 데이터 저장소 | **Cassandra** - 페이스북의 분산 칼럼 지향 데이터베이스 | [slideshare.net](http://www.slideshare.net/planetcassandra/cassandra-introduction-features-30103666)
| 데이터 저장소 | **DynamoDB** - 아마존의 문서 지향 데이터베이스 | [harvard.edu](http://www.read.seas.harvard.edu/~kohler/class/cs239-w08/decandia07dynamo.pdf) |
| 데이터 저장소 | **MongoDB** - 문서 지향 데이터베이스 | [slideshare.net](http://www.slideshare.net/mdirolf/introduction-to-mongodb) |
| 데이터 저장소 | **Spanner** - 구글의 전 세계 분산 데이터베이스 | [research.google.com](http://research.google.com/archive/spanner-osdi2012.pdf) |
| 데이터 저장소 | **Memcached** - 분산 메모리 캐싱 시스템 | [slideshare.net](http://www.slideshare.net/oemebamo/introduction-to-memcached) |
| 데이터 저장소 | **Redis** - 영속성과 값 타입을 지원하는 분산 메모리 캐싱 시스템 | [slideshare.net](http://www.slideshare.net/dvirsky/introduction-to-redis) |
| | | |
| 파일 시스템 | **Google File System (GFS)** - 분산 파일 시스템 | [research.google.com](http://static.googleusercontent.com/media/research.google.com/zh-CN/us/archive/gfs-sosp2003.pdf) |
| 파일 시스템 | **Hadoop File System (HDFS)** - GFS의 오픈소스 구현 | [apache.org](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html) |
| | | |
| 기타 | **Chubby** - 구글의 느슨하게 결합된 분산 시스템용 잠금 서비스 | [research.google.com](http://static.googleusercontent.com/external_content/untrusted_dlcp/research.google.com/en/us/archive/chubby-osdi06.pdf) |
| 기타 | **Dapper** - 분산 시스템 추적 인프라 | [research.google.com](http://static.googleusercontent.com/media/research.google.com/en//pubs/archive/36356.pdf)
| 기타 | **Kafka** - 링크드인의 pub/sub 메시지 큐 | [slideshare.net](http://www.slideshare.net/mumrah/kafka-talk-tri-hug) |
| 기타 | **Zookeeper** - 동기화를 가능하게 하는 중앙집중형 인프라 및 서비스 | [slideshare.net](http://www.slideshare.net/sauravhaloi/introduction-to-apache-zookeeper) |
| | 아키텍처 추가하기 | [기여하기](#기여하기) |

### 기업별 아키텍처

| 회사 | 참고 자료 |
|---|---|
| Amazon | [Amazon architecture](http://highscalability.com/amazon-architecture) |
| Cinchcast | [Producing 1,500 hours of audio every day](http://highscalability.com/blog/2012/7/16/cinchcast-architecture-producing-1500-hours-of-audio-every-d.html) |
| DataSift | [Realtime datamining At 120,000 tweets per second](http://highscalability.com/blog/2011/11/29/datasift-architecture-realtime-datamining-at-120000-tweets-p.html) |
| Dropbox | [How we've scaled Dropbox](https://www.youtube.com/watch?v=PE4gwstWhmc) |
| ESPN | [Operating At 100,000 duh nuh nuhs per second](http://highscalability.com/blog/2013/11/4/espns-architecture-at-scale-operating-at-100000-duh-nuh-nuhs.html) |
| Google | [Google architecture](http://highscalability.com/google-architecture) |
| Instagram | [14 million users, terabytes of photos](http://highscalability.com/blog/2011/12/6/instagram-architecture-14-million-users-terabytes-of-photos.html)<br/>[What powers Instagram](http://instagram-engineering.tumblr.com/post/13649370142/what-powers-instagram-hundreds-of-instances) |
| Justin.tv | [Justin.Tv's live video broadcasting architecture](http://highscalability.com/blog/2010/3/16/justintvs-live-video-broadcasting-architecture.html) |
| Facebook | [Scaling memcached at Facebook](https://cs.uwaterloo.ca/~brecht/courses/854-Emerging-2014/readings/key-value/fb-memcached-nsdi-2013.pdf)<br/>[TAO: Facebook’s distributed data store for the social graph](https://cs.uwaterloo.ca/~brecht/courses/854-Emerging-2014/readings/data-store/tao-facebook-distributed-datastore-atc-2013.pdf)<br/>[Facebook’s photo storage](https://www.usenix.org/legacy/event/osdi10/tech/full_papers/Beaver.pdf)<br/>[How Facebook Live Streams To 800,000 Simultaneous Viewers](http://highscalability.com/blog/2016/6/27/how-facebook-live-streams-to-800000-simultaneous-viewers.html) |
| Flickr | [Flickr architecture](http://highscalability.com/flickr-architecture) |
| Mailbox | [From 0 to one million users in 6 weeks](http://highscalability.com/blog/2013/6/18/scaling-mailbox-from-0-to-one-million-users-in-6-weeks-and-1.html) |
| Netflix | [A 360 Degree View Of The Entire Netflix Stack](http://highscalability.com/blog/2015/11/9/a-360-degree-view-of-the-entire-netflix-stack.html)<br/>[Netflix: What Happens When You Press Play?](http://highscalability.com/blog/2017/12/11/netflix-what-happens-when-you-press-play.html) |
| Pinterest | [From 0 To 10s of billions of page views a month](http://highscalability.com/blog/2013/4/15/scaling-pinterest-from-0-to-10s-of-billions-of-page-views-a.html)<br/>[18 million visitors, 10x growth, 12 employees](http://highscalability.com/blog/2012/5/21/pinterest-architecture-update-18-million-visitors-10x-growth.html) |
| Playfish | [50 million monthly users and growing](http://highscalability.com/blog/2010/9/21/playfishs-social-gaming-architecture-50-million-monthly-user.html) |
| PlentyOfFish | [PlentyOfFish architecture](http://highscalability.com/plentyoffish-architecture) |
| Salesforce | [How they handle 1.3 billion transactions a day](http://highscalability.com/blog/2013/9/23/salesforce-architecture-how-they-handle-13-billion-transacti.html) |
| Stack Overflow | [Stack Overflow architecture](http://highscalability.com/blog/2009/8/5/stack-overflow-architecture.html) |
| TripAdvisor | [40M visitors, 200M dynamic page views, 30TB data](http://highscalability.com/blog/2011/6/27/tripadvisor-architecture-40m-visitors-200m-dynamic-page-view.html) |
| Tumblr | [15 billion page views a month](http://highscalability.com/blog/2012/2/13/tumblr-architecture-15-billion-page-views-a-month-and-harder.html) |
| Twitter | [Making Twitter 10000 percent faster](http://highscalability.com/scaling-twitter-making-twitter-10000-percent-faster)<br/>[Storing 250 million tweets a day using MySQL](http://highscalability.com/blog/2011/12/19/how-twitter-stores-250-million-tweets-a-day-using-mysql.html)<br/>[150M active users, 300K QPS, a 22 MB/S firehose](http://highscalability.com/blog/2013/7/8/the-architecture-twitter-uses-to-deal-with-150m-active-users.html)<br/>[Timelines at scale](https://www.infoq.com/presentations/Twitter-Timeline-Scalability)<br/>[Big and small data at Twitter](https://www.youtube.com/watch?v=5cKTP36HVgI)<br/>[Operations at Twitter: scaling beyond 100 million users](https://www.youtube.com/watch?v=z8LU0Cj6BOU)<br/>[How Twitter Handles 3,000 Images Per Second](http://highscalability.com/blog/2016/4/20/how-twitter-handles-3000-images-per-second.html) |
| Uber | [How Uber scales their real-time market platform](http://highscalability.com/blog/2015/9/14/how-uber-scales-their-real-time-market-platform.html)<br/>[Lessons Learned From Scaling Uber To 2000 Engineers, 1000 Services, And 8000 Git Repositories](http://highscalability.com/blog/2016/10/12/lessons-learned-from-scaling-uber-to-2000-engineers-1000-ser.html) |
| WhatsApp | [The WhatsApp architecture Facebook bought for $19 billion](http://highscalability.com/blog/2014/2/26/the-whatsapp-architecture-facebook-bought-for-19-billion.html) |
| YouTube | [YouTube scalability](https://www.youtube.com/watch?v=w5WVu624fY8)<br/>[YouTube architecture](http://highscalability.com/youtube-architecture) |

### 기업 엔지니어링 블로그

> 면접을 보는 회사의 아키텍처입니다.
>
> 마주칠 문제가 같은 도메인에서 나올 수 있습니다.

* [Airbnb Engineering](http://nerds.airbnb.com/)
* [Atlassian Developers](https://developer.atlassian.com/blog/)
* [AWS Blog](https://aws.amazon.com/blogs/aws/)
* [Bitly Engineering Blog](http://word.bitly.com/)
* [Box Blogs](https://blog.box.com/blog/category/engineering)
* [Cloudera Developer Blog](http://blog.cloudera.com/)
* [Dropbox Tech Blog](https://tech.dropbox.com/)
* [Engineering at Quora](https://www.quora.com/q/quoraengineering)
* [Ebay Tech Blog](http://www.ebaytechblog.com/)
* [Evernote Tech Blog](https://blog.evernote.com/tech/)
* [Etsy Code as Craft](http://codeascraft.com/)
* [Facebook Engineering](https://www.facebook.com/Engineering)
* [Flickr Code](http://code.flickr.net/)
* [Foursquare Engineering Blog](http://engineering.foursquare.com/)
* [GitHub Engineering Blog](https://github.blog/category/engineering)
* [Google Research Blog](http://googleresearch.blogspot.com/)
* [Groupon Engineering Blog](https://engineering.groupon.com/)
* [Heroku Engineering Blog](https://engineering.heroku.com/)
* [Hubspot Engineering Blog](http://product.hubspot.com/blog/topic/engineering)
* [High Scalability](http://highscalability.com/)
* [Instagram Engineering](http://instagram-engineering.tumblr.com/)
* [Intel Software Blog](https://software.intel.com/en-us/blogs/)
* [Jane Street Tech Blog](https://blogs.janestreet.com/category/ocaml/)
* [LinkedIn Engineering](http://engineering.linkedin.com/blog)
* [Microsoft Engineering](https://engineering.microsoft.com/)
* [Microsoft Python Engineering](https://blogs.msdn.microsoft.com/pythonengineering/)
* [Netflix Tech Blog](http://techblog.netflix.com/)
* [Paypal Developer Blog](https://developer.paypal.com/community/blog/)
* [Pinterest Engineering Blog](https://medium.com/@Pinterest_Engineering)
* [Reddit Blog](http://www.redditblog.com/)
* [Salesforce Engineering Blog](https://developer.salesforce.com/blogs/engineering/)
* [Slack Engineering Blog](https://slack.engineering/)
* [Spotify Labs](https://labs.spotify.com/)
* [Stripe Engineering Blog](https://stripe.com/blog/engineering)
* [Twilio Engineering Blog](http://www.twilio.com/engineering)
* [Twitter Engineering](https://blog.twitter.com/engineering/)
* [Uber Engineering Blog](http://eng.uber.com/)
* [Yahoo Engineering Blog](http://yahooeng.tumblr.com/)
* [Yelp Engineering Blog](http://engineeringblog.yelp.com/)
* [Zynga Engineering Blog](https://www.zynga.com/blogs/engineering)

#### 출처 및 더 읽을거리

블로그를 추가하고 싶으신가요? 작업이 중복되지 않도록 다음 저장소에 회사 블로그를 추가하는 것을 고려해 보세요.

* [kilimchoi/engineering-blogs](https://github.com/kilimchoi/engineering-blogs)

## 개발 중

섹션을 추가하거나 진행 중인 작업을 완성하는 데 관심이 있으신가요? [기여해 주세요](#기여하기)!

* MapReduce를 이용한 분산 컴퓨팅
* 일관된 해싱(Consistent hashing)
* 스캐터 게더(Scatter gather)
* [기여하기](#기여하기)

## 크레딧

이 저장소 곳곳에 크레딧과 출처가 표기되어 있습니다.

특별히 다음에 감사드립니다.

* [Hired in tech](http://www.hiredintech.com/system-design/the-system-design-process/)
* [Cracking the coding interview](https://www.amazon.com/dp/0984782850/)
* [High scalability](http://highscalability.com/)
* [checkcheckzz/system-design-interview](https://github.com/checkcheckzz/system-design-interview)
* [shashank88/system_design](https://github.com/shashank88/system_design)
* [mmcgrana/services-engineering](https://github.com/mmcgrana/services-engineering)
* [System design cheat sheet](https://gist.github.com/vasanthk/485d1c25737e8e72759f)
* [A distributed systems reading list](http://dancres.github.io/Pages/)
* [Cracking the system design interview](http://www.puncsky.com/blog/2016-02-13-crack-the-system-design-interview)

## 연락처

이슈, 질문, 의견이 있으면 편하게 연락 주세요.

연락처는 원저자의 [GitHub 페이지](https://github.com/donnemartin)에서 확인할 수 있습니다.

## 라이선스

*이 저장소의 코드와 자료는 오픈 소스 라이선스로 제공됩니다. 이곳은 개인 저장소이므로, 코드와 자료에 대한 라이선스는 고용주(Facebook)가 아니라 원저자 개인이 부여하는 것입니다.*

    Copyright 2017 Donne Martin

    Creative Commons Attribution 4.0 International License (CC BY 4.0)

    http://creativecommons.org/licenses/by/4.0/
