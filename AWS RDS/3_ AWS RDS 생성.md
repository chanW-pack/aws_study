# AWS RDS 생성

---

이번 페이지에서는 RDS 생성 및 사용 방법, 멀티 AZ와 Read Replica를 지원하는 RDS를

만들어보고, 만일 Primary를 강제로 내리면 Stanby가 fail over 되는 동작을 

실전으로 구축해보겠다.

## RDS 생성하기 (Multi AZ)

---

![Untitled](https://user-images.githubusercontent.com/84123877/194469601-945bebfa-54c0-4c7b-963d-900352343525.png)

![Untitled 1](https://user-images.githubusercontent.com/84123877/194469546-0cef19ef-e84b-4634-b091-f211d223326f.png)

> 데이터베이스탭으로 이동하여 생성을 선택한다.
본인은 MySQL로 생성하겠다.
> 

![Untitled 2](https://user-images.githubusercontent.com/84123877/194469552-d752a80e-c34b-421f-9a13-d7ed6c4fa029.png)

> 탬플릿을 선택한다. **프로덕션은 멀티 AZ를 지원**하고, **나머지는 싱글 RDS 인스턴스**이다.
> 

만약 무료 버전을 이용하고 싶으면 프리티어 항목을 선택하면 된다.

본인은 멀티 AZ를 구축할 예정이기에, 프로덕션을 선택했다.

![Untitled 3](https://user-images.githubusercontent.com/84123877/194469553-0d6ee2c7-433f-429c-ab99-0ea762d14378.png)

> DB 인스턴스에 대해 고유한 이름을 지정 및 마스터 사용자 이름을 설정한다.
> 

마스터 사용자의 아이디/비밀번호를 작성할 수 있다.
이는 나중에 CLI나 GUL Tool로 접속 할때 사용된다.

![Untitled 4](https://user-images.githubusercontent.com/84123877/194469555-0ae01060-c6eb-4bda-9fe1-2a7fd31dfc64.png)

> 인스턴스 클래스를 선택한다. 표준, 메모리 최적화, 버스트 성능의 3종류가 있다.
> 

![Untitled 5](https://user-images.githubusercontent.com/84123877/194469557-dcba08cb-7969-44fe-a24d-948f2508c3bf.png)

![Untitled 6](https://user-images.githubusercontent.com/84123877/194469559-f0b25635-9d26-4b93-adab-f9abf776381f.png)

> RDS 인스턴스의 EBS를 선택한다.
> 

RDS 인스턴스도 일종의 EC2이기 때문에 EBS 스토리지가 필요하다.
(그래야 데이터베이스의 데이터 내용들을 저장할 수 있기 때문이다..)

선택할 수 있는 스토리지 유형은 다음과 같다.

- 범용(SSD) 스토리지
- 프로비저닝된 IOPS(SSD) 스토리지
- 마그네틱

<aside>
💡 만일 **범용 (SSD) 스토리지**를 선택할 경우, **스토리지 자동 조정 옵션**을 설정할 수 있는데,
스토리지 자동 조정 기능은 처리량이 급증하여 DB 여유공간이 부족할 때, 자동으로
스토리지를 확장하는 기능이다.
(**EC2의 Auto scaling 서비스의 RDS 버전이라고 보면 된다**.)

</aside>

![Untitled 7](https://user-images.githubusercontent.com/84123877/194469560-8ba736ba-dac7-49c4-8518-abf3f535c513.png)

> 구축 연습이므로 가장 저렴한 마그네틱을 선택했다.
(프리티어의 스토리지 최대 할당량은 20GB이다.)
> 

![Untitled 8](https://user-images.githubusercontent.com/84123877/194469561-522aad64-adec-4c8a-9554-c46572fd4ce2.png)

> 멀티 AZ 예비 인스턴스 생성 선택
> 

다중 AZ 배포를 선정하면, DB 인스턴스에 고가용성과 장애 조치 기능을 
지원하게 설정 가능하다.

![Untitled 9](https://user-images.githubusercontent.com/84123877/194469565-a915350f-ddac-4a8b-8789-d567df101715.png)

> VPC 전반 설정을 진행한다.
> 

RDS 인스턴스도 일종의 EC2이기 때문에, 위치할 VPC를 설정해주어야 한다.

퍼블릭 액세스 외부 접속 허용시켰다.

- VPC / 서브넷그룹
: VPC가 구성되어 있다면 선택해주고, Default를 사용해도 무방하다.
EC2 인스턴스와 연동할 경우 동일한 곳에 설치하는 것이 일반적
- 퍼블릭 액세스 가능 
: 데이터베이스 인스턴스에 공인 IP 주소를 할당할지 여부를 나타낸다.
외부에서 MySQL 워크벤츠에 접속할 예정이므로 활성하를 했다.
- VPC 보안 그룹
: 퍼블릭 액서스 가능을 설정하게 되면, 인/아웃바운드 규칙이 정의된 보안 그룹을
설정해야한다.
- 포트
: MySQL 기본 포트인 3306을 사용한다.

![Untitled 10](https://user-images.githubusercontent.com/84123877/194469567-a17b24b8-05b5-4eae-af26-ff4349bf7252.png)

> 데이터베이스 암호화 형식을 설정한다.
> 

RDS 암호화 인증 방식 3가지중 고를 수 있다.

가장 무난한 아이디/패스워드 식 암호 인증을 선택했다.

![Untitled 11](https://user-images.githubusercontent.com/84123877/194469568-6a27d9f5-f324-4d3f-b192-08ded4caca37.png)

> 추가 구성을 설정한다.
> 

추가 구성은 넘어가도 되지만 하는것을 권장한다고 한다.

그 이유는 초기 데이터베이스 이름을 지정해야하기 때문인데,

그 외 백업 같은 나머지는 디폴트로 두고 넘어갔다. 
(나중에 수정 가능)

![Untitled 12](https://user-images.githubusercontent.com/84123877/194469572-0ece1708-e13c-49b7-9ede-48dee420a31f.png)

> RDS 요금을 미리 확인할 수 있다.
> 

멀티 AZ 인프라 구성을 구축했기에, 예비 Stanby 인스턴스가 지속적으로 돌아가게되어
인스턴스 요금이 2배로 청구되는것을 볼 수 있다.

설정이 완료되었으면 데이터베이스 생성 버튼을 선택한다.

데이터베이스가 만들어지는데 시간이 걸리니 기다려야한다.

---
