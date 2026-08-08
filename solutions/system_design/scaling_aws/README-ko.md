# AWS에서 수백만 사용자까지 확장되는 시스템 설계하기

> 한국어 번역본입니다. 원문: [README.md](README.md) | 전체 문서: [README-ko.md](../../../README-ko.md)

*참고: 이 문서는 중복을 피하기 위해 [시스템 설계 주제](https://github.com/donnemartin/system-design-primer#index-of-system-design-topics)의 관련 항목으로 직접 링크합니다. 일반적인 논점, 트레이드오프, 대안은 링크된 내용을 참고하세요.*

## 1단계: 유스케이스와 제약 조건 정리하기

> 요구사항을 수집하고 문제 범위를 정합니다.
> 유스케이스와 제약 조건을 명확히 하기 위해 질문하세요.
> 가정에 대해 논의하세요.

명확히 답해줄 면접관이 없으므로, 여기서는 몇 가지 유스케이스와 제약 조건을 직접 정의하겠습니다.

### 유스케이스

이 문제는 1) **벤치마크/부하 테스트**, 2) 병목 **프로파일링**, 3) 대안과 트레이드오프를 평가하며 병목 해결, 4) 반복이라는 접근으로 풉니다. 기본 설계를 확장 가능한 설계로 발전시키는 데 좋은 패턴입니다.

AWS 경험이 있거나 AWS 지식을 요구하는 직무에 지원하는 경우가 아니라면, AWS에 특화된 세부 사항은 필수가 아닙니다. 다만 **이 연습에서 논의하는 원칙의 상당수는 AWS 생태계 밖에서도 더 일반적으로 적용할 수 있습니다.**

#### 다음 유스케이스만 다루도록 문제 범위를 좁힙니다

* **사용자**가 읽기 또는 쓰기 요청을 보냄
    * **서비스**가 처리를 수행하고 사용자 데이터를 저장한 뒤 결과를 반환함
* **서비스**는 소수의 사용자에서 수백만 사용자까지 감당하도록 발전해야 함
    * 많은 사용자와 요청을 처리하도록 아키텍처를 발전시키며 일반적인 확장 패턴을 논의함
* **서비스**는 고가용성을 가짐

### 제약 조건과 가정

#### 가정 정리

* 트래픽은 고르게 분포하지 않음
* 관계형 데이터가 필요함
* 사용자 1명에서 수천만 명까지 확장
    * 사용자 증가를 다음과 같이 표기함
        * Users+
        * Users++
        * Users+++
        * ...
    * 사용자 1,000만 명
    * 월 10억 건의 쓰기
    * 월 1,000억 건의 읽기
    * 읽기 대 쓰기 비율 100:1
    * 쓰기당 콘텐츠 1 KB

#### 사용량 계산

**봉투 뒷면 계산을 해야 하는지 면접관에게 확인하세요.**

* 월 1 TB의 신규 콘텐츠
    * 쓰기당 1 KB * 월 10억 건의 쓰기
    * 3년간 36 TB의 신규 콘텐츠
    * 대부분의 쓰기는 기존 콘텐츠 갱신이 아니라 신규 콘텐츠라고 가정
* 평균 초당 400건의 쓰기
* 평균 초당 40,000건의 읽기

편리한 환산 가이드:

* 월 250만 초
* 초당 1건 = 월 250만 건
* 초당 40건 = 월 1억 건
* 초당 400건 = 월 10억 건

## 2단계: 상위 수준 설계 만들기

> 중요한 구성 요소를 모두 담아 상위 수준 설계를 그립니다.

![Imgur](http://i.imgur.com/B8LDKD7.png)

## 3단계: 핵심 컴포넌트 설계하기

> 각 핵심 컴포넌트를 깊이 파고듭니다.

### 유스케이스: 사용자가 읽기 또는 쓰기 요청을 보낸다

#### 목표

* 사용자가 1~2명뿐이라면 기본적인 구성만 있으면 됨
    * 단순함을 위해 단일 박스
    * 필요할 때 수직 확장
    * 병목을 파악하기 위한 모니터링

#### 단일 박스로 시작하기

* EC2 위의 **웹 서버**
    * 사용자 데이터 저장소
    * [**MySQL 데이터베이스**](https://github.com/donnemartin/system-design-primer#relational-database-management-system-rdbms)

**수직 확장**을 사용합니다.

* 그냥 더 큰 박스를 선택하면 됨
* 어떻게 확장할지 결정하기 위해 지표를 주시할 것
    * CPU, 메모리, IO, 네트워크 등 기본 모니터링으로 병목 파악
    * CloudWatch, top, nagios, statsd, graphite 등
* 수직 확장은 매우 비싸질 수 있음
* 이중화/장애 조치가 없음

*트레이드오프, 대안, 추가 세부 사항:*

* **수직 확장**의 대안은 [**수평 확장**](https://github.com/donnemartin/system-design-primer#horizontal-scaling)입니다.

#### SQL로 시작하고 NoSQL을 고려하기

제약 조건에서 관계형 데이터가 필요하다고 가정했습니다. 단일 박스 위의 **MySQL 데이터베이스**로 시작할 수 있습니다.

*트레이드오프, 대안, 추가 세부 사항:*

* [관계형 데이터베이스 관리 시스템(RDBMS)](https://github.com/donnemartin/system-design-primer#relational-database-management-system-rdbms) 섹션 참고
* [SQL이냐 NoSQL이냐](https://github.com/donnemartin/system-design-primer#sql-or-nosql)를 선택하는 이유를 논의

#### 공인 고정 IP 할당하기

* Elastic IP는 재부팅해도 IP가 바뀌지 않는 공개 엔드포인트를 제공함
* 장애 조치에 도움이 됨. 도메인을 새 IP로 가리키기만 하면 됨

#### DNS 사용하기

Route 53 같은 **DNS**를 추가해 도메인을 인스턴스의 공인 IP에 매핑합니다.

*트레이드오프, 대안, 추가 세부 사항:*

* [도메인 네임 시스템](https://github.com/donnemartin/system-design-primer#domain-name-system) 섹션 참고

#### 웹 서버 보안 강화하기

* 필요한 포트만 열기
    * 웹 서버가 다음 요청에 응답하도록 허용
        * HTTP는 80
        * HTTPS는 443
        * SSH는 22, 화이트리스트에 등록된 IP만
    * 웹 서버가 외부로 연결을 시작하지 못하도록 차단

*트레이드오프, 대안, 추가 세부 사항:*

* [보안](https://github.com/donnemartin/system-design-primer#security) 섹션 참고

## 4단계: 설계 확장하기

> 제약 조건을 고려해 병목을 찾아내고 해결합니다.

### Users+

![Imgur](http://i.imgur.com/rrfjMXB.png)

#### 가정

사용자 수가 늘기 시작하면서 단일 박스의 부하가 증가하고 있습니다. **벤치마크/부하 테스트**와 **프로파일링** 결과, **MySQL 데이터베이스**가 메모리와 CPU 자원을 점점 더 많이 차지하고 있으며 사용자 콘텐츠가 디스크 공간을 채우고 있습니다.

지금까지는 **수직 확장**으로 이 문제들을 해결해 왔습니다. 그러나 이 방법은 비용이 상당히 비싸졌고, **MySQL 데이터베이스**와 **웹 서버**를 독립적으로 확장할 수도 없습니다.

#### 목표

* 단일 박스의 부하를 줄이고 독립적인 확장이 가능하게 함
    * 정적 콘텐츠를 **오브젝트 스토어**에 별도로 저장
    * **MySQL 데이터베이스**를 별도의 박스로 이전
* 단점
    * 이러한 변경은 복잡성을 높이고, **웹 서버**가 **오브젝트 스토어**와 **MySQL 데이터베이스**를 가리키도록 수정해야 함
    * 새 컴포넌트를 보호하기 위한 추가 보안 조치가 필요함
    * AWS 비용이 증가할 수 있으나, 유사한 시스템을 직접 관리하는 비용과 견주어 판단해야 함

#### 정적 콘텐츠 별도 저장하기

* S3 같은 관리형 **오브젝트 스토어**를 사용해 정적 콘텐츠를 저장하는 것을 고려
    * 확장성과 신뢰성이 높음
    * 서버 측 암호화
* 정적 콘텐츠를 S3로 이전
    * 사용자 파일
    * JS
    * CSS
    * 이미지
    * 동영상

#### MySQL 데이터베이스를 별도 박스로 이전하기

* RDS 같은 서비스로 **MySQL 데이터베이스**를 관리하는 것을 고려
    * 관리와 확장이 간단함
    * 다중 가용 영역
    * 저장 시 암호화

#### 시스템 보안 강화하기

* 전송 중과 저장 시 데이터를 암호화
* 가상 사설 클라우드(VPC) 사용
    * 단일 **웹 서버**가 인터넷과 트래픽을 주고받을 수 있도록 퍼블릭 서브넷 생성
    * 나머지는 모두 프라이빗 서브넷에 두어 외부 접근 차단
    * 각 컴포넌트마다 화이트리스트에 등록된 IP에 대해서만 포트를 개방
* 이 연습의 나머지 부분에서 추가되는 새 컴포넌트에도 동일한 패턴을 적용해야 함

*트레이드오프, 대안, 추가 세부 사항:*

* [보안](https://github.com/donnemartin/system-design-primer#security) 섹션 참고

### Users++

![Imgur](http://i.imgur.com/raoFTXM.png)

#### 가정

**벤치마크/부하 테스트**와 **프로파일링** 결과, 단일 **웹 서버**가 피크 시간대에 병목이 되어 응답이 느려지고 경우에 따라 다운타임이 발생합니다. 서비스가 성숙해감에 따라 가용성과 이중화도 높이고자 합니다.

#### 목표

* 다음 목표들은 **웹 서버**의 확장 문제를 해결하기 위한 것입니다.
    * **벤치마크/부하 테스트**와 **프로파일링** 결과에 따라 이 기법 중 한두 가지만 적용해도 될 수 있습니다.
* 증가하는 부하를 감당하고 단일 장애점을 해결하기 위해 [**수평 확장**](https://github.com/donnemartin/system-design-primer#horizontal-scaling)을 사용
    * Amazon ELB나 HAProxy 같은 [**로드 밸런서**](https://github.com/donnemartin/system-design-primer#load-balancer)를 추가
        * ELB는 고가용성을 가짐
        * **로드 밸런서**를 직접 구성한다면, 여러 가용 영역에 [액티브-액티브](https://github.com/donnemartin/system-design-primer#active-active)나 [액티브-패시브](https://github.com/donnemartin/system-design-primer#active-passive)로 여러 서버를 두면 가용성이 향상됨
        * 백엔드 서버의 연산 부하를 줄이고 인증서 관리를 단순화하기 위해 **로드 밸런서**에서 SSL을 종료
    * 여러 가용 영역에 걸쳐 여러 대의 **웹 서버**를 사용
    * 이중화 향상을 위해 여러 가용 영역에 걸쳐 [**마스터-슬레이브 장애 조치**](https://github.com/donnemartin/system-design-primer#master-slave-replication) 모드의 **MySQL** 인스턴스를 여러 개 사용
* **웹 서버**와 [**애플리케이션 서버**](https://github.com/donnemartin/system-design-primer#application-layer)를 분리
    * 두 계층을 독립적으로 확장하고 설정
    * **웹 서버**는 [**리버스 프록시**](https://github.com/donnemartin/system-design-primer#reverse-proxy-web-server)로 동작할 수 있음
    * 예를 들어 **읽기 API**를 처리하는 **애플리케이션 서버**와 **쓰기 API**를 처리하는 서버를 따로 둘 수 있음
* 부하와 지연을 줄이기 위해 정적 콘텐츠(및 일부 동적 콘텐츠)를 CloudFront 같은 [**콘텐츠 전송 네트워크(CDN)**](https://github.com/donnemartin/system-design-primer#content-delivery-network)로 이전

*트레이드오프, 대안, 추가 세부 사항:*

* 자세한 내용은 위에 링크된 문서를 참고하세요.

### Users+++

![Imgur](http://i.imgur.com/OZCxJr0.png)

**참고:** 복잡해지지 않도록 **내부 로드 밸런서**는 표시하지 않았습니다.

#### 가정

**벤치마크/부하 테스트**와 **프로파일링** 결과, 읽기가 많고(쓰기 대비 100:1) 많은 읽기 요청 때문에 데이터베이스 성능이 나빠지고 있습니다.

#### 목표

* 다음 목표들은 **MySQL 데이터베이스**의 확장 문제를 해결하기 위한 것입니다.
    * **벤치마크/부하 테스트**와 **프로파일링** 결과에 따라 이 기법 중 한두 가지만 적용해도 될 수 있습니다.
* 부하와 지연을 줄이기 위해 다음 데이터를 Elasticache 같은 [**메모리 캐시**](https://github.com/donnemartin/system-design-primer#cache)로 이전
    * **MySQL**에서 자주 접근되는 콘텐츠
        * **메모리 캐시**를 도입하기 전에, 먼저 **MySQL 데이터베이스** 캐시 설정만으로 병목이 해소되는지 확인해 볼 것
    * **웹 서버**의 세션 데이터
        * **웹 서버**가 무상태가 되어 **오토스케일링**이 가능해짐
    * 메모리에서 1 MB를 순차적으로 읽는 데 약 250마이크로초가 걸리는 반면, SSD에서는 4배, 디스크에서는 80배 더 오래 걸립니다.<sup><a href=https://github.com/donnemartin/system-design-primer#latency-numbers-every-programmer-should-know>1</a></sup>
* 쓰기 마스터의 부하를 줄이기 위해 [**MySQL 읽기 복제본**](https://github.com/donnemartin/system-design-primer#master-slave-replication)을 추가
* 응답성 향상을 위해 **웹 서버**와 **애플리케이션 서버**를 추가

*트레이드오프, 대안, 추가 세부 사항:*

* 자세한 내용은 위에 링크된 문서를 참고하세요.

#### MySQL 읽기 복제본 추가하기

* **메모리 캐시**를 추가하고 확장하는 것에 더해, **MySQL 읽기 복제본**도 **MySQL 쓰기 마스터**의 부하를 덜어줄 수 있음
* 쓰기와 읽기를 분리하는 로직을 **웹 서버**에 추가
* **MySQL 읽기 복제본** 앞에 **로드 밸런서**를 추가(복잡해지지 않도록 그림에는 표시하지 않음)
* 대부분의 서비스는 쓰기보다 읽기가 많음

*트레이드오프, 대안, 추가 세부 사항:*

* [관계형 데이터베이스 관리 시스템(RDBMS)](https://github.com/donnemartin/system-design-primer#relational-database-management-system-rdbms) 섹션 참고

### Users++++

![Imgur](http://i.imgur.com/3X8nmdL.png)

#### 가정

**벤치마크/부하 테스트**와 **프로파일링** 결과, 미국의 정규 업무 시간에 트래픽이 급증하고 사용자가 퇴근하면 크게 떨어집니다. 실제 부하에 따라 서버를 자동으로 늘리고 줄이면 비용을 절감할 수 있다고 판단했습니다. 작은 조직이므로 **오토스케일링**과 일반 운영을 위한 DevOps를 최대한 자동화하고자 합니다.

#### 목표

* 필요에 따라 용량을 확보하도록 **오토스케일링**을 추가
    * 트래픽 급증에 대응
    * 사용하지 않는 인스턴스를 꺼서 비용 절감
* DevOps 자동화
    * Chef, Puppet, Ansible 등
* 병목 해결을 위해 지표 모니터링을 지속
    * **호스트 수준** - 개별 EC2 인스턴스 확인
    * **집계 수준** - 로드 밸런서 통계 확인
    * **로그 분석** - CloudWatch, CloudTrail, Loggly, Splunk, Sumo
    * **외부 사이트 성능** - Pingdom 또는 New Relic
    * **알림과 장애 대응** - PagerDuty
    * **오류 리포팅** - Sentry

#### 오토스케일링 추가하기

* AWS **오토스케일링** 같은 관리형 서비스를 고려
    * **웹 서버**마다, 그리고 **애플리케이션 서버** 유형마다 그룹을 하나씩 만들고, 각 그룹을 여러 가용 영역에 배치
    * 인스턴스의 최소·최대 개수를 설정
    * CloudWatch를 통해 확장·축소를 트리거
        * 부하가 예측 가능하다면 단순한 시간대 기반 지표, 또는
        * 일정 기간 동안의 지표:
            * CPU 부하
            * 지연 시간
            * 네트워크 트래픽
            * 사용자 정의 지표
    * 단점
        * 오토스케일링은 복잡성을 유발할 수 있음
        * 수요 증가에 맞춰 시스템이 적절히 확장되거나 수요 감소 시 축소되기까지 시간이 걸릴 수 있음

### Users+++++

![Imgur](http://i.imgur.com/jj3A5N8.png)

**참고:** 복잡해지지 않도록 **오토스케일링** 그룹은 표시하지 않았습니다.

#### 가정

서비스가 제약 조건에 명시된 수치를 향해 계속 성장함에 따라, **벤치마크/부하 테스트**와 **프로파일링**을 반복 수행하여 새로운 병목을 찾아내고 해결합니다.

#### 목표

문제의 제약 조건에 따른 확장 문제를 계속 해결해 나갑니다.

* **MySQL 데이터베이스**가 너무 커지기 시작하면, 데이터베이스에는 제한된 기간의 데이터만 저장하고 나머지는 Redshift 같은 데이터 웨어하우스에 저장하는 것을 고려할 수 있음
    * Redshift 같은 데이터 웨어하우스는 월 1 TB의 신규 콘텐츠라는 제약 조건을 여유롭게 감당할 수 있음
* 평균 초당 40,000건의 읽기 요청에 대해, 인기 콘텐츠의 읽기 트래픽은 **메모리 캐시**를 확장해 대응할 수 있음. 메모리 캐시는 고르지 않게 분포한 트래픽과 트래픽 급증을 처리하는 데도 유용함
    * **SQL 읽기 복제본**은 캐시 미스를 감당하기 어려울 수 있으므로, 추가적인 SQL 확장 패턴을 적용해야 할 가능성이 높음
* 평균 초당 400건의 쓰기(피크는 훨씬 높을 것으로 추정)는 단일 **SQL 쓰기 마스터-슬레이브**에는 부담스러울 수 있어, 이 역시 추가적인 확장 기법이 필요함을 시사함

SQL 확장 패턴에는 다음이 있습니다.

* [페더레이션](https://github.com/donnemartin/system-design-primer#federation)
* [샤딩](https://github.com/donnemartin/system-design-primer#sharding)
* [비정규화](https://github.com/donnemartin/system-design-primer#denormalization)
* [SQL 튜닝](https://github.com/donnemartin/system-design-primer#sql-tuning)

많은 읽기·쓰기 요청을 더 잘 해결하려면, 적절한 데이터를 DynamoDB 같은 [**NoSQL 데이터베이스**](https://github.com/donnemartin/system-design-primer#nosql)로 옮기는 것도 고려해야 합니다.

독립적인 확장이 가능하도록 [**애플리케이션 서버**](https://github.com/donnemartin/system-design-primer#application-layer)를 더 분리할 수 있습니다. 실시간으로 처리할 필요가 없는 배치 작업이나 연산은 **큐**와 **워커**를 이용해 [**비동기적으로**](https://github.com/donnemartin/system-design-primer#asynchronism) 수행할 수 있습니다.

* 예를 들어 사진 서비스라면 사진 업로드와 썸네일 생성을 분리할 수 있습니다.
    * **클라이언트**가 사진을 업로드
    * **애플리케이션 서버**가 SQS 같은 **큐**에 작업을 등록
    * EC2나 Lambda의 **워커 서비스**가 **큐**에서 작업을 가져와 다음을 수행
        * 썸네일 생성
        * **데이터베이스** 갱신
        * 썸네일을 **오브젝트 스토어**에 저장

*트레이드오프, 대안, 추가 세부 사항:*

* 자세한 내용은 위에 링크된 문서를 참고하세요.

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
