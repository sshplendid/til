# Developing on AWS

## 교육환경

* 강사: 정도현
* 교육관련 웹사이트
  * http://aws-class.com/31792

## 1일차

* 클라우드 컴퓨팅: 인프라를 소프트웨어로 생각하고 사용할 수 있음
* 장점
  * 가변 비용화: 필요할 때 필요한 만큼 씀
  * 규모의 경제: 싸다
  * 용량 추정 불필요
  * 속도/민첩성: 자주 사용하는 서비스를 대부분 제공하기 때문에 빠르게 개발이 가능함
  * 몇 분만에 전세계 배포

* AWS 서비스 스택
  * 인프라
    * 가용영역(AZ): IDC같은 개념
    * 리전: 2개 이상의 가용영역을 묶은 논리단위
    * 엣지 로케이션: DNS, 캐시서버같은 엔드포인트
  * 기초서비스
    * 컴퓨팅
      * EC2: Virtual Machine
      * Auto Scaling: 인스턴스 스케일링
    * 네트워크
    * 스토리지
      * S3: http 프로토콜을 이용한 key:value 스토어, 최대 16TB
    * 관리도구
      * CloudWatch: 상태 모니터링 서비스
      * CloudTrail: AWS REST API 로그 추적
      * CloudFormation: JSON/YAML 생성 서비스

* 관리형 vs 비관리형
  * 비관리형: 사용자가 확장, 내결함성 및 가용성을 ㅗ간리
  * 관리형: 서비스에 내장

* 클라우드 배포 모델
  * 올인 클라우드: 처음부터 AWS에서 시작
  * 하이브리드: 기존 자원과 함께 사용하며 필요할 때 AWS 자원을 사용

* 마이크로 서비스
  * 복잡성 관리

* 아키텍쳐 진화
  1. EC2: VM, H/W
  1. ECS: Linux Container(Docker), OS
  1. Lambda: Function, 실행시간

* 리전 및 AZ(가용영역))
  * 리전: 2개 이상의 가용영역
  * 가용영역: 하나 이상의 데이터 센터

### 모듈2: AWS 기반 개발


* Database vs Data Warehouse
  * Database: I/O, Transaction 위주
  * Data Warehouse:
    * 저장/분석 용도
    * 분석용도에 맞춰서 큰 테이블 생성(join)
    * GB, TB 단위 테이블 다수 존재
    * 분산 처리

* NoSQL
  * SQL은 시계열성 데이터 축적에 성능부하가 생김
  * NoSQL DB는 RDS의 Consistency에서 자유로움
  
### DynamoDB

#### 특징

* RDS의 Database 레이어가 없다. => Table이 최상위 단위
* 파티션 키(필수), 정렬 키(옵션)
* 파티션 키를 필수로 복수 개의 파티션에 저장됨 => 핫 파티션 방지

#### 읽기 일관성

* **최종적 일관성**: 쓰기작업이 진행되는 경우 이전 데이터를 반환할 수 있음
  * 정합성을 포기하는 대신 빠름
* 강력한 일관성: 가장 최신 데이터를 반환

#### 처리단위

* RCU(읽기 용량 단위): 최대 4kb/s
* WCU(쓰기 용량 단위): 1KB/s

#### 인덱스

* GSI: 다른 키를 가지고 검색을 할때?
* LSI: 기존에 걸어둔 소트키 외로 소트를 걸어줄때?

#### 스트림

RDS 트리처럼, 이벤트가 발생할 때마다 람다 호출

#### 글로벌 테이블

* 전 세계 리전에 동일 테이블에 동기화
* 동일 데이터가 복수 리전에 추가되는 경우, 가장 최근에 추가된 데이터가 생존

#### 조건부 쓰기작업

* 낙관 lock: 일단 작업을 진행하고 데이터가 변경된 경우에 fail
  * 비관 lock보다 성능이 좋음

#### 배치

* 배치 작업에서 실패가 존재해도 전체 작업은 실패하지 않음
  * 실패 케이스에 대한 처리를 해줘야 함

#### 워크로드

* 적절한 파티션 키 선정이 중요
  * 헤비 유저 vs 라이트 유저: 유저 ID만으로 충분할까?

#### 핫 데이터 vs 콜드 데이터

엑세스 빈도에 따라 테이블을 분리

* 시계열 성 데이터는 시간단위(분기, 월, 주, 일, ...)로 테이블을 분리

### 마이크로 서비스

#### 주요 개념

* 과도기 SOA
  * 각 기능을 분리-네트워크로 연결: 서비스 분리는 성공
  * 그러나 데이터베이스(트랜잭션)는 하나로 연결
    * 일관성
  * 각 서비스에서 트랜잭션이 각각 발생할때 퍼포먼스 저하
  * DB가 하나이기 때문에 기능 복잡도 증가

* 마이크로 서비스
  * SOA의 실패를 거울삼아...
  * DB도 분리
  * 본질적으론 SOA와 똑같지만
  * 데이터를 분산
  * 외부 공개 API가 중요
  * 마이크로 서비스 구조
    * 데이터 스토어
    * 애플리케이션 로직
    * **퍼블릭 API**: 외부로 공개하기 때문에 변경이 (가능한) 없어야 함
      * 변경이 없으려면? 간단한 일 하나만 하는 API를 여러개 만들자!
  * 전략
    * 점진적 개선
      * 작은 목표를 성공하는 것을 목표로 점진적으로 개선
      * 각 단계마다 테스트/배포 등의 오버헤드가 큼
      * 사람만으로 할 수 없음. 툴에 의한 자동화가 필요

### 람다

* 서버리스 컴퓨팅?
  * 프로비저닝 할 필요 없음
  * 사용량에 따라 조정
  * 유휴시간에 대한 지불 필요 없음

### API Gateway

캐시, 인증, 보안 등등의 공통처리 가능
