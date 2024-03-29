# AWS RDS 개념 & 아키텍쳐 정리

---

## RDS (Relational Database Service) 란?

---

AWS는 RDS란 **관계형 데이터베이스**를 간편하게 **클라우드**에서 설정, 운영, 확장이
가능하도록 지원하는 웹 서비스이다.

RDS는 MySQL이나 오라클 같은 데이터베이스의 설치, 모니터링, 백업, 알람 등
관리를 대신해주며, 하드웨어 프로비저닝, 데이터베이스 설정, 패치 및 백업과 같이
잦은 운영 작업을 자동화하여 비용 효울적이고 크기 조정 가능한 DB 서비스를 제공한다.

따라서 RDS를 통해 개발자는 DB 인프라를 구성하는데 힘을 들이지 않고,
개발이라는 본질적인 작업에 집중할 수 있게되는 장점이 있다.

![Untitled](https://user-images.githubusercontent.com/84123877/191171510-e5974089-d027-45bf-9703-080616c70ee3.png)

물론 EC2 인스턴스 자체에서 직접 데이터베이스를 설치해서 사용해도 된다.
(EC2 자체가 컴퓨터이니…)

하지만 AWS RDS는 EC2에 RDB(관계형 데이터베이스)를 직접 구축하여 운영할 때보다
더 많은 부분을 자동으로 관리할 수 있어 편리하기 때문에 많이 이용한다.

![Untitled 1](https://user-images.githubusercontent.com/84123877/191171474-58e70acd-18ff-4d17-bd24-2f74901bd779.png)

RDS의 데이터베이스의 제공 방식은 EC2와 비슷하다.
EC2 인스턴스를 생성해서 컴퓨팅을 사용하듯이, RDS 인스턴스를 생성해서 DB를 사용하는 원리이다.

하지만 EC2같이 유저가 시스템에 직접 로그인은 불가능하다.
그래서 RDS 인스턴스의 OS패치, 관리 등은 AWS가 전담한다.
또한, RDS는 데이터베이스 모니터링 기능을 지원해주는데, DB에서 발생하는 여러 로그
(Error Log, General Log 등)를 CloudWatch와 연동하여 확인도 가능하다.

RDS는 기본적으로 VPC안에서 동작하며, 기본적으로 Public IP를 부여하지 않아
**외부에서 접근 불가능하다.**

다만, 설정에 따라 public으로 오픈 가능하며 대신 로드밸런서 같이 DNS로만 접근이 가능하다.

![Untitled 2](https://user-images.githubusercontent.com/84123877/191171477-1ddd6d5b-17f0-4027-92c6-a346dd079c91.png)

EC2 인스턴스를 생성할 때 EC2 타입과 EBS 타입을 고르듯, 
RDS 인스턴스를 생성할 때에도 인스턴스 타입을 지정해주며, 
스토리지는 EBS를 그대로 활용하기 때문에 EBS 타입의 선택도 필요하다.

(그리고 RDS는 유동적으로 데이터 저장소 용량을 증설하는게 아닌, 생성시 EBS의 용량을 지정해서
생성한다. 추후에 용량 증설 가능)

<cf> AWS 데이터베이스 서비스인 Amazon Aurora는 용량지정 X, 사용한만큼만 비용 지불

# 🤩🥰

RDS의 가장 큰 특징은 파라미터 그룹(Paramater Group) 시스템인데, 
이는 DB의 설정값들을 모아 그룹화한 개념이다.

이 DB 설정들을 모은 그룹을 각 DB 인스턴스에 적용시켜 DB의 설정값을 적용하는 시스템이다.
(왜냐하면 직접 RDS 인스턴스 수정이 불가능하기 때문에 우회적으로 설정값을 세팅하는것)

<aside>
💡 AWS 프리티어로는 RDS를 12개월동안 단일 AZ, db.t2.micro 인스턴스를 
750시간 무료 이용 가능하다.

</aside>

## RDS 데이터베이스 종류

---

RDS에서는 Amazon Aurora, PostgreSQL, MySQL, MariaDB, Oracle, MS SQLServer 
총 6개의 데이터베이스엔진 중에서 원하는 DBMS를 선택할 수 있다.

또한 AWS Databae Migration Services를 사용하여 기존 데이터베이스를
Amazon RDS로 손쉽게 마이그레이션 도는 복제할 수 있다.

![Untitled 3](https://user-images.githubusercontent.com/84123877/191171478-563e3d76-e601-4494-96f4-b889bc0e1de3.png)

<aside>
💡 **Info**

**Amazon Aurora**는 MySQL 및 PostgreSQL호환 관계형 데이터베이스로, 오픈 소스 
데이터베이스의 간편 성과 비용 효율성을 결합한 것이다. 
Amazon Aurora의 속도는 표준 MySQL 데이터베이스보다 5배, PostgreSQL 데이터베이스
보다 3배 빠르다고 한다. 
또한 상용 데이터베이스의 보안, 가용성 및 안전성을 1/10의 비용으로 제공하기도 한다. 

**PostgreSQL**은 오픈 소스 관계형 데이터베이스 중 기능도 많고 성능도 좋은 거의 
원탑의 데이터베이스이다.  

**MySQL**은 세계적으로 가장 많이 사용되는 오픈 소스 관계형 
데이터베이스다. Amazon RDS를 통해 비용 효율적이고 크기 조정이 가능한 MySQL 
서버를 몇 분 안에 생성할 수 있다. 
백업, 소프트웨어 패치, 모니터링, 확장 & 축소, 복제 같은 시간이 걸리는 작업은 
모두 관리되므로 사용자는 개발에만 집중할 수 있다. 

**MariaDB**는 MySQL을 개발한 개발자가 만든 오픈 소스 관계형 데이터베이스 이다. MySQL 업그레이드 판 이라고 봐도 된다. MySQL과 동일하게 Amazon RDS를 통해 효율적인 MariaDB 데이터베이스를 생성할 수 있고, 모든 시간 소모적 작업을 대신 처리해 준다. 

**Oracle**는 오라클사의 유료 관계형 데이터베이스로서, RDS를 사용해 클라우드에서 손쉽게 배포, 설정, 운영 할 수 있는 완전 관리형 상용 데이터베이스이다. 다만 유료 데이터베이스라 라이선스 비용이 든다. 

**SQL Server(MSSQL)**는 Microsoft에서 개발한 관계형 데이터베이스 관리 시스템으로 Amazon RDS를 통해 손쉽게 배포, 운영, 확장이 가능하다.

</aside>

## RDS의 특징 및 기능

---

### 1. 관리 부담 감소

- **사용 편의성**
: 몇 분 이내에 데이터베이스 인스턴스를 시작하고 애플리케이션을 연결할 수 있다.
DB 파라미터 그룹을 사용하면 데이터베이스를 세부적으로 제어하고 튜닝할 수도 있다.
- **자동 소프트웨어 패치**
: RDS는 최신 패치를 통해 배포를 지원하는 관계형 데이터베이스 소프트웨어가 최신 상태로
유지되도록 한다. 데이터베이스 인스턴스의 패치 여부와 시기를 선택적으로 제어 가능하다.
- **모범 사례 권장 시스템**
: RDS에서는 데이터베이스 인스턴스의 구성과 사용 지표를 분석하여 모범 사례 지침을 
제공한다. 고객은 권장 조치를 즉각 수행하거나 다음 유지 관리 기간에 수행하도록
예약하거나 완전히 무시할 수 있다.

### 2. 확장성

- **즉각적인 컴퓨팅 규모 조정**
: 배포에 사용할 컴퓨팅 및 메모리 리소스를 최대 vCPU 32개와 RAM 244G의 범위 내에서
확장하거나 축소할 수 있다. 컴퓨팅 규모 조정 작업은 일반적으로 몇 분이면 완료된다.
- **간편한 스토리지 규모 조정**
: 스토리지에 대한 요구가 증가함에 따라 추가 스토리지를 프로비저닝할 수도 있다.
또한 DB 가동 중단 없이 즉시 스토리지 확장이 가능하다.
- **읽기 전용 복제본 시스템**
: DB 인스턴스의 복제본을 하나 이상 생성하여 대량의 애플리케이션 읽기 트래픽을
처리할 수 있는 기능을 제공 한다.

### 3. 가용성 및 내구성

- **자동 백업**
: RDS는 데이터베이스와 트랜직션 로그를 백업하고 사용자가 지정한 보전 기간 동안
이를 모두 저장할 수 있다. 이를 통해 데이터베이스를 보전 기간 중 어느 시점(초 단위)으로나
복원할 수 있다. (최근 5분까지 가능)
자동 백업 보존 기간은 최대 35일로 구성할 수 있다.
- **데이터베이스 스냅샷**
: 사용자는 원하는 경우 언제든 데이터베이스 스냅샷으로 새 RDB 인스턴스를 생성할 수 있다.
- **다중 AZ 구성 배포**
: 다중 AZ 구성은 현재 서비스되는 RDB에 문제가 되어도 다른 AZ에 있는 RDB로 빠르게 
장애 복구를 할 수 있다. 가용성 및 내구성을 높여주므로 프로덕션 데이터베이스 워크로드에 
적합하다.
- **자동 호스트 교체**
: 하드웨어에 장애가 발생할 경우, 배포를 지원하는 컴퓨팅 인스턴스를 자동으로 교체한다.

### 4. 보안

- **저장 데이터 및 전송 데이터 암호화**
: RDS를 사용하면 사용자가 AWS Key Management Service(KMS)를 통해 관리하는 키를
사용해 데이터베이스를 암호화할 수 있다.
자동 백업, 읽기 전용 복제본 및 스냅샷과 마찬가지로 기본 스토리지에 저장하는 데이터가
암호화 된다.
- **네트워크 격리**
: VPC에서 데이터베이스 인스턴스를 실행하여 방화벽 설정을 구성하고 데이터베이스
인스턴스에 대한 네트워크 액세스를 제어할 수 있다.
- **IAM 리소스 수준 권한**
: RDS는 IAM과 통합되어 사용자 및 그룹이 특정 Amazon RDS 리소스 (데이터베이스 인스턴스,
스냅샷, 파라미터 그룹 및 옵션 그룹)에서 수행할 수 있는 작업을 제어하는 기능을 제공한다.
또한 RDS 리소스에 태그를 지정하고, IAM 사용자 및 그룹이 태그가 동일하고 연관된 값을
가진 리소스 그룹에서 수행할 수 있는 작업을 제어할 수 있다.

(ex: 개발자는 “제어” 데이터베이스 인스턴스를 수정할 수 있고, “프로덕션” 데이터베이스 
인스턴스는 데이터베이스 관리자만 수정할 수 있게끔 IAM 규칙을 수정할 수 있다.)

### 5. 관리 효율성

- **모니터링 및 지표**
: RDS는 추가 비용 없이 데이터베이스 인스턴스에 대한 Amazon CloudWatch와 연동해
지표를 제공한다. RDS 관리 콘솔을 사용하면 컴퓨팅/메모리/스토리지 용량 사용률,
I/O 작업, 인스턴스 연결 등 주요 작업 지표를 보고 성능 문제를 신속하게 감지할 수 있는
편리한 도구인 성능 개선 도우미에 액세스할 수 있다.
- **이벤트 알림**
: Amazon SNS를 통해 데이터베이스 이벤트를 이메일이나 SMS 텍스트 메세지로 
알려줄 수 있다.

### 6. 비용 효율성

- **사용한 만큼 비용 지불**
- **예약 인스턴스**
: Amazon RDS 예약 인스턴스는 1년 또는 3년 약정기간 동안 DB 인스턴스를 예약할 수 있는
옵션을 제공하므로 온디맨드 인스턴스 요금과 비교하여 DB 인스턴스의 시간당 요금을 대폭
할인받을 수 있다.
- **인스턴스 중지 및 시작**
: Amazon RDS를 활용하면 한 번에  최대 7일까지 데이터베이스 인스턴스를 쉽게 중지했다
시작할 수 있다. 따라서 데이터베이스를 상시 구동하지 않아도 되는 개발 및 테스트 작업에
데이터베이스를 쉽고 저렴하게 이용할 수 있다.

## RDS 백업 시스템

---

### 자동 백업

자동 백업(Automated Backups) 줄여서 **AB**는 매일마다 **스냅샷과 *트랜잭션 로그를 참고하여
자동으로 백업**을 해준다.
(RDS에서는 디폴트로 AB 기능이 설정되어 있다.)

그리고 AB를 통해  데이터베이스를 **Retention Period(1~35일)** 안의 과거 특정 시간으로
되돌아갈 수 있다.

단, 롤백 동작은 과거 상태로 그대로 돌아가는게 아닌, 
**다른 DB 인스턴스를 새로 생성해서 스냅샷을 적용 시키는 형식**임을 유의해야 한다.

RDB 백업 정보는 S3에 저장되며, AB 동안 약간의 I/O suspension(딜레이)이 존재할 수 있다.
그나마 Multi-az로 하면 Standby를 통해 백업을 수행하기 때문에 딜레이가 덜하다.

* 트랜잭션 : 데이터베이스의 상태를 변화시키는 하나의 논리적 기능을 수행하기 위한 작업의 단위

![Untitled 4](https://user-images.githubusercontent.com/84123877/191171480-772a0b33-cec9-40b5-8726-d35178f129e1.png)

### 수동 백업

AB(자동 백업)이 자동으로 스냅샷을 떠서 백업하는 것이라면, 
수동 백업은 유저 혹은 다른 프로세스로부터 요청에 따라 만들어지는 DB 스냅샷이다.

즉, EC2 스냅샷을 뜨듯이 사용자에 의해 수동적으로 진행되는 백업이다.

만약 원본 RDS를 **삭제한다고 하더라도, 스냅샷은 S3 버킷에 그대로 존재**한다.
따라서 스냅샷만으로 RDS 인스턴스를 복원시킬 수 있다.

반대로 **AB 백업 기능은 인스턴스를 삭제할 때 스냅샷도 모두 없어진다**는 특징이 있다.

이 역시 스냅샷의 복구는 항상 새로운 DB Instance를 생성 하여 수행되며,
만약 데이터베이스를 복구해야 한다면, 새로운 DB를 만들고 기존 DB의 연결을 끊고
새로 만든 DB에 연결해주는 작업이 필요하다.

![Untitled 5](https://user-images.githubusercontent.com/84123877/191171481-c8e3b8b2-8bd4-43ba-9ca2-2333f75adb27.png)

### RDS 백업 복원시 도메인 변경

원본 RDS 인스턴스를 가지고 새로운 DB를 복원시 새로운 인스턴스와 Endpoint가 생성된다.

원본 DNS는 앞에 original 인 반면, 복원된 것은 앞에 restored가 붙게 된다.


![Untitled 6](https://user-images.githubusercontent.com/84123877/191171482-61a794fe-85f0-464f-aa23-946bca069d0e.png)

### RDS 인증 여러가지 방법

![Untitled 7](https://user-images.githubusercontent.com/84123877/191171483-0013ad3a-a0a3-4a08-9e58-ee71a5558b2e.png)

1. **전통적인 유저/패스워드 방식**
- AWS Secret Manger(기타 유저 패스워드 인증 관리 서비스)와 연동하여 자동 로테이션
가능하게 할 수 있다.
- 일정주기마다 자동으로 패스워드를 변경해주기도 하고 다른 서비스들이 RDS나 다른 서비스에 접근할 때  패스워드를 하드코딩 할 필요없게 해준다.

![Untitled 8](https://user-images.githubusercontent.com/84123877/191171485-e2d36f7a-284d-4077-b92b-d38f331a8d09.png)

1. **IAM DB 인증**
- 데이터베이스 IAM 유저 크레덴셜/Role을 통해 관리 가능

![Untitled 9](https://user-images.githubusercontent.com/84123877/191171487-34880f18-f432-4711-b3dc-7c84c2117682.png)

1. **Kerberos 인증**
- 마이크로소프트의 액티브 디렉토리 안에 포함되어 있는 프로토콜
- 네트워크 상에 직접 아이디/패스워드를 입력하지 않아도 되게끔 만들어주는 대칭키
기반의 프로토콜

![Untitled 10](https://user-images.githubusercontent.com/84123877/191171488-5a605d81-a059-4800-9ce3-58dfa15b8d3d.png)

## RDS 가격 모델

---

- 컴퓨팅 파워 + 스토리지 용량 + 백업 용량 + 네트워크 비용을 모두 합쳐서 지불
- EC2 인스턴스를 구매해 사용했던 것처럼, RDS에는 Reserved Instance(RI)를 구매해서
사용하는 시스템이다.
    - EC2와 마찬가지로 일정기간을 계약하여 저렴한 서비스를 이용
    - 3년 약정이면 거의 56%이상 저렴
    - 단, 인스턴스 타입 변경이 쉽지 않기 때문에 수요에 대해 신중하게 결정
    
![Untitled 11](https://user-images.githubusercontent.com/84123877/191171489-8241b45a-9e3e-4e99-bcaf-17a988483e68.png)
    

## RDS 요금 정책

---

![Untitled 12](https://user-images.githubusercontent.com/84123877/191171492-cbb99f9f-15ed-4579-a18f-8c4499721fb2.png)

## 그 외의 AWS 데이터베이스 서비스

---

![Untitled 13](https://user-images.githubusercontent.com/84123877/191171494-feb4d6c1-0681-4218-a126-0bbff9ac8356.png)

AWS 데이터베이스 서비스는 RDS 이외에 다양한 데이터베이스 서비스를 제공한다.

- Amazon DocumentDB 
: 문서 기반 데이터베이스를 제공한다.
- Amazon Neptune 
: 그래프 데이터베이스를 제공한다.
- Amazon Timestream 
: 시계열 데이터베이스를 제공한다.
- Amazon Quamtum Ledger Database
: 장부 데이터베이스를 제공한다.
- Amazon ElasticCache
: 인 메모리 데이터베이스이다. 메모리를 활용하여 처리 속도가 빠르며
Redis 용과 Memcached 용이 있다.


---
