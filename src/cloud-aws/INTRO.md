# AWS Cloud 개요

* 클라우드 컴퓨팅?
  * 인프라는 더 이상 하드웨어가 아니다.
  * 인프라를 서비스로 생각하자.
  * IT 리소스와 애플리케이션을 온디맨드로 제공하는 서비스
  * 종량 과금제
  * 유연성: IT 리소스를 필요한만큼 프로비저닝할 수 있음 => 하드웨어 낭비를 줄임

* 클라우드 컴퓨팅의 종류: 아래 구분은 AWS 이후 타 업체에서 구분한 것임.
  * IaaS: 컴퓨터, 네트워크, 스토리지 등 (EC2, EBS, S3, ...)
  * PaaS: 인프라 관리할 필요 없이 애플리케이션을 실행 (NoSQL, SQS, ...)
  * SaaS: 서비스 제공자가 관리하는 제품을 사용 (Workspaces, ...)

* 이점
  * 자본비용의 가변화: 사용한 만큼만 지불
  * 규모의 경제: 많이 사용할수록 더 저렴해짐
  * 용량 추정 불필요: 필요한 IT 리소스 산정할 필요가 없음
  * 속도 및 민첩성 개선: 필요한 리소스를 몇 분 만에 사용할 수 있음
  * IDC 투자비용 불필요
  * 전세계 배포

* AWS: 웹 기반 클라우드 서비스
  * 관리형 vs 비관리형 서비스
    * 비관리형: 사용자가 스케일링, 내결함성, 가용성을 관리
      * ex) EC2, 오토 스케일링을 설정하지 않는 한 트래픽 처리량 동일함
    * 관리형: 서비스에 내장되어 있음
      * ex) S3 정적 웹사이트 호스팅, 서비스가 각 파일의 가용성을 관리

* 클라우드 배포모델
  * 올인 클라우드: 애플리케이션의 모든 부분이 클라우드에서 실행
    * 애플리케이션이 클라우드에서 생성 or 기존 인프라에서 클라우드로 Mig.
    * 낮은 수준의 인프라 상에 구축하거나, 주요 인프라를 관리 설계/확장할 필요가 없음
  * 하이브리드: 레거시 리소스와 클라우드 리소스간의 연결
    * 클라우드 리소스를 내부 시스템(On-premise)에 연결

* 마이크로서비스 특징
  * 민첩성: 서비스별 소규모 독립 팀으로 구성
  * 유연한 조정: 기능 요구사항에 맞게 인프라 요구에 맞게 조정
  * 손쉬운 배포: CI/CD 가능
  * 기술적 자유: 팀에 맞는 최적의 도구를 자유롭게 선택
  * 재사용 가능한 코드
  * 복원력

* MSA 모범사례
  * 구성요소를 오류없이 변경
    * 인터페이스는 계약: 한번 만든 외부 공개 API는 변경하면 안됨
    * 기능 변경이 소비자에게 영향을 끼치면 안됨
  * 간단한 API를 사용
    * 서비스 사용비용을 절감
    * 복잡성 증가는 변경에 대한 저항 증가
    * 적게 공유할수록 오류도 적음
    * 상세 정보를 숨김
  * 서버를 상태 비저장 모드로 처리
    * 상태를 저장하지 않으면 Auto Scaling을 통해 인스턴스를 쉽게 추가하고 제거할 수 있음

* 아키텍쳐 진화
  * EC2: H/W 추상화 / VM 단위 / 서버, 스토리지, 네트워킹, OS를 구성
  * ECS: OS 추상화 / 작업 단위 / 애플리케이션 구성
  * Lambda: 실행시간 / 함수 단위 / 필요시 코드를 실행

## AWS 인프라 개요

* AWS 데이터 센터
  * 단일 데이터 센터에서 수천 개 서버 운영
  * 모든 데이터 센터를 온라인으로 연결
* 리전 및 AZ
  * 리전: 여러 AZ를 포함하고 있는 지리적 위치
  * AZ
    * 하나 이상의 데이터 센터로 구성
    * 결함 분리 방식으로 설계
    * 고속 프라이빗 링크를 통해 다른 AZ와 상호연결
      * AWS 내부 통신은 외부와 비교해 빠름
* 리전 선택 방법
  * 법적 요구사항: 현지 법에 따라 특정 데이터의 물리적 위치를 강제할 때
  * 고객 근접성: 가까울수록 빠르다
  * 가용 서비스: 리전에 따라 사용할 수 있는 서비스의 차이가 있음
  * 비용: 리전별로 다름 / 제일 저렴? CA / 제일 비싸? SP

## AWS 기초 서비스

### EC2

* 특징
  * 종량 과금제: 인스턴스 사용 시간만큼 비용 지불
  * HW/SW 선택의 자유: 원하는 OS, 원하는 스토리지 선택
  * Amazon Machine Image: Windows, Ubuntu, RHEL 등 제공하는 이미지 선택 가능
  * 필요할 때만 사용: 서버를 **인스턴스**로 사용
* 인스턴스 유형
  * 범용(T2, M5, M4): 트래픽이 적은 웹 서버 및 소형 DB
  * 컴퓨팅 최적화(C5, C4): 비디오 인코딩
  * 메모리 최적화(X1e, X1, R4): 고성능 DB, 하둡/Spark
  * 스토리지 최적화(H1, I3, D2): DW, 로그/데이터 처리 애플리케이션
  * 가속 컴퓨팅(P3, P2, G3, F1): ML, 3D App 스트리밍
* 요금
  * 단위
    * 초 단위 결제: Amazon Linux, Ubuntu
    * 시간 단위: 그 외 OS
  * 인스턴스 종류
    * 온디맨드: 일반 인스턴스
    * 예약 인스턴스
    * 스팟 인스턴스
      * 사용되지 않는 인스턴스
      * 언제든 사라질 수 있음
      * 온디맨드 대비 최대 90% 비용 절감 가능

### ELB

* 특징
  * 트래픽을 여러 AZ / 여러 EC2 인스턴스로 분산
  * EC2 상태확인 기능 지원
  * EC2 인스턴스에 대한 트래픽 라우팅 / LB 지원
  * 상태확인: EC2 가용성 확인을 위해 주기적으로 Ping(상태확인)을 보냄
    * 인스턴스 비정상이 확인되면 해당 인스턴스로의 라우팅을 중단
  * 고정세션
    * 로드밸런서가 사용자의 세션을 특정 서버에 바인딩
    * 단점
      * 애플리케이션으니 확장을 제한
      * 서버 전체에서의 불균등한 로드
      * 단일 사용자의 로드가 서버에 균등하게 분배하지 않기때문에 사용자 응답지연 가능성
  * 고정세션 대신 캐시를 사용하자
    * 세션정보를 외부 캐시서버(EleastiCache/DynamoDB)로 빼자

### Auto Scaling

* 특징
  * 애플리케이션 처리를 위한 EC2 인스턴스 수를 적절하게 유지
  * 사용자 정의 조건(ex: CPU 사용률 80%)에 따라 인스턴스 수를 조정
  * 조정 타입
    * 수동 조정: 최대/최소 및 원하는 용량 설정
    * 예약 조정: 예측가능한 이벤트에 따라 AS 발생 시간을 지정
    * 동적 조정: 성능 측정 임계값을 기반으로 조정