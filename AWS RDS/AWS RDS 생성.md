# AWS RDS 생성

---

이번 페이지에서는 RDS 생성 및 사용 방법, 멀티 AZ와 Read Replica를 지원하는 RDS를

만들어보고, 만일 Primary를 강제로 내리면 Stanby가 fail over 되는 동작을 

실전으로 구축해보겠다.

## RDS 생성하기 (Multi AZ)

---

![Untitled](AWS%20RDS%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%2005e8e24b943a4a80a5c9d13d1fdf1853/Untitled.png)

![Untitled](AWS%20RDS%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%2005e8e24b943a4a80a5c9d13d1fdf1853/Untitled%201.png)

> 데이터베이스탭으로 이동하여 생성을 선택한다.
본인은 MySQL로 생성하겠다.
> 

![Untitled](AWS%20RDS%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%2005e8e24b943a4a80a5c9d13d1fdf1853/Untitled%202.png)

> 탬플릿을 선택한다. **프로덕션은 멀티 AZ를 지원**하고, **나머지는 싱글 RDS 인스턴스**이다.
> 

만약 무료 버전을 이용하고 싶으면 프리티어 항목을 선택하면 된다.

본인은 멀티 AZ를 구축할 예정이기에, 프로덕션을 선택했다.

![Untitled](AWS%20RDS%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%2005e8e24b943a4a80a5c9d13d1fdf1853/Untitled%203.png)

> DB 인스턴스에 대해 고유한 이름을 지정 및 마스터 사용자 이름을 설정한다.
> 

마스터 사용자의 아이디/비밀번호를 작성할 수 있다.
이는 나중에 CLI나 GUL Tool로 접속 할때 사용된다.

![Untitled](AWS%20RDS%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%2005e8e24b943a4a80a5c9d13d1fdf1853/Untitled%204.png)

> 인스턴스 클래스를 선택한다. 표준, 메모리 최적화, 버스트 성능의 3종류가 있다.
> 

![Untitled](AWS%20RDS%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%2005e8e24b943a4a80a5c9d13d1fdf1853/Untitled%205.png)

![Untitled](AWS%20RDS%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%2005e8e24b943a4a80a5c9d13d1fdf1853/Untitled%206.png)

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

![Untitled](AWS%20RDS%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%2005e8e24b943a4a80a5c9d13d1fdf1853/Untitled%207.png)

> 구축 연습이므로 가장 저렴한 마그네틱을 선택했다.
(프리티어의 스토리지 최대 할당량은 20GB이다.)
> 

![Untitled](AWS%20RDS%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%2005e8e24b943a4a80a5c9d13d1fdf1853/Untitled%208.png)

> 멀티 AZ 예비 인스턴스 생성 선택
> 

다중 AZ 배포를 선정하면, DB 인스턴스에 고가용성과 장애 조치 기능을 
지원하게 설정 가능하다.

![Untitled](AWS%20RDS%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%2005e8e24b943a4a80a5c9d13d1fdf1853/Untitled%209.png)

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

![Untitled](AWS%20RDS%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%2005e8e24b943a4a80a5c9d13d1fdf1853/Untitled%2010.png)

> 데이터베이스 암호화 형식을 설정한다.
> 

RDS 암호화 인증 방식 3가지중 고를 수 있다.

가장 무난한 아이디/패스워드 식 암호 인증을 선택했다.

![Untitled](AWS%20RDS%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%2005e8e24b943a4a80a5c9d13d1fdf1853/Untitled%2011.png)

> 추가 구성을 설정한다.
> 

추가 구성은 넘어가도 되지만 하는것을 권장한다고 한다.

그 이유는 초기 데이터베이스 이름을 지정해야하기 때문인데,

그 외 백업 같은 나머지는 디폴트로 두고 넘어갔다. 
(나중에 수정 가능)

![Untitled](AWS%20RDS%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%2005e8e24b943a4a80a5c9d13d1fdf1853/Untitled%2012.png)

> RDS 요금을 미리 확인할 수 있다.
> 

멀티 AZ 인프라 구성을 구축했기에, 예비 Stanby 인스턴스가 지속적으로 돌아가게되어
인스턴스 요금이 2배로 청구되는것을 볼 수 있다.

설정이 완료되었으면 데이터베이스 생성 버튼을 선택한다.

데이터베이스가 만들어지는데 시간이 걸리니 기다려야한다.

---

![Untitled](AWS%20RDS%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%2005e8e24b943a4a80a5c9d13d1fdf1853/Untitled%2013.png)

![Untitled](AWS%20RDS%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%2005e8e24b943a4a80a5c9d13d1fdf1853/Untitled%2014.png)

![Untitled](AWS%20RDS%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%2005e8e24b943a4a80a5c9d13d1fdf1853/Untitled%2015.png)

> RDS 생성 중 마스터 패스워드를 지정해주었다.
마스터 사용자 이름과 마스터 암호를 이용해서 데이터베이스에 접속할 수 있게 되었다.
> 

![Untitled](AWS%20RDS%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%2005e8e24b943a4a80a5c9d13d1fdf1853/Untitled%2016.png)

> 파라미터 그룹을 생성한다. (파리미터 그룹 → 파라미터 그룹 생성 선택)
> 

RDS 인스턴스를 생성하면서 기본으로 생성된 **디폴트 파라미터 그룹은 수정이 불가하다.**

그래서 파라미터 그룹을 새로 만들어 데이터베이스 인스턴스에 적용해야 한다.

[파라미터 그룹 생성] 버튼을 누른다.

![Untitled](AWS%20RDS%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%2005e8e24b943a4a80a5c9d13d1fdf1853/Untitled%2017.png)

> 파라미터 그룹을 생성한다.
> 

파라미터 그룹 패밀리는 생성한 데이터베이스의 버전과 맞춰주면 된다.

이후 그룹 이름과 설명을 작성한다.

![Untitled](AWS%20RDS%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%2005e8e24b943a4a80a5c9d13d1fdf1853/Untitled%2018.png)

![Untitled](AWS%20RDS%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%2005e8e24b943a4a80a5c9d13d1fdf1853/Untitled%2019.png)

### 데이터베이스 타임존 설정

![Untitled](AWS%20RDS%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%2005e8e24b943a4a80a5c9d13d1fdf1853/Untitled%2020.png)

> **데이터베이스 타임존**을 설정했다.
> 

생성한 파라미터 그룹에서 파라미터 편집으로 이동한 뒤,
검색창에 time_zone을 검색하여 time_zone 파라미터를 
Asia/Seoul로 변경해준다.

### 데이터베이스 문자셋 설정

![Untitled](AWS%20RDS%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%2005e8e24b943a4a80a5c9d13d1fdf1853/Untitled%2021.png)

> 데이터베이스 문자셋도 설정한다.
> 

한글 데이터가 깨지지 않고 잘 출력시키게 하기 위해서 utf-8 인코더를 설정해주어야 한다.

char 검색 후 나오는 모든 부분의 값을 utf8mb4로 설정한다.

- - character_set_client  : utf8mb4
- - character_set_connection : utf8mb4
- - character_set_database : utf8mb4
- - character_set_results : utf8mb4
- - character_set_server : utf8mb4

<aside>
💡 utf8과 utf8mb4 모두 한글이 깨지지 않게 인코딩해주지만 차이가 있다.
utf8은 이모지를 저장할 수 없지만, utf8mb4는 이모지를 저장할 수 있다.
utf8은 가변 3바이트를 사용하는데 반해서, utf8mb4는 내부적으로 
한 문자를 표현하는데 4바이트를 사용한다. 
따라서 3바이트만 사용하는 utf8은 이모지를 저장할 수 없다.

</aside>

### collation 변경

![Untitled](AWS%20RDS%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%2005e8e24b943a4a80a5c9d13d1fdf1853/Untitled%2022.png)

![Untitled](AWS%20RDS%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%2005e8e24b943a4a80a5c9d13d1fdf1853/Untitled%2023.png)

> 이번에는 collation을 검색해서 모두 ufw8mb4_general_ci 로 변경한다.
> 

### !! 위 사진은 잘못 설정했다. utf8_ge.. 가 아닌 utf8md4_ge.. 로 변경해야한다.

- collation_connection : uft8mb4_general_ci
- collation_server : utf8mb4_general_ci

### RDS 인스턴스에 파라미터 그룹 변경하기

![Untitled](AWS%20RDS%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%2005e8e24b943a4a80a5c9d13d1fdf1853/Untitled%2024.png)

![Untitled](AWS%20RDS%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%2005e8e24b943a4a80a5c9d13d1fdf1853/Untitled%2025.png)

> 추가 구성 화면에서 DB 파라미터 그룹을 방금 생성한 파라미터 그룹으로 변경한다.
> 

![Untitled](AWS%20RDS%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC%2005e8e24b943a4a80a5c9d13d1fdf1853/Untitled%2026.png)

> 설정 변경 즉시 적용한다.
> 

최하단 계속 버튼을 누르면 다음과 같이 수정 사항이 요약된 것을 확인할 수 있다.
여기서 반영 시점을 즉시 적용으로 한다.

만일 예약된 다음 유지 시간으로 하면 지금 하지 않고, 새벽 시간대에 진행한다.

<aside>
💡 이 수정사항이 반영되는 동안 데이터베이스가 작동하지 않을 수도 있으므로  예약
시간을 걸어두라는 의미이지만, 
본인은 아직 서비스가 오픈되지 않았기 때문에 즉시 적용으로 진행하였다.

</aside>

---