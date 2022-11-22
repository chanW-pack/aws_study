# AWS RDS 실전 구축 (Multy AZ) 외부 접속(EC2, 워크벤치) 및 RDS fail-over 테스트

---

## 외부에서 RDS 접속하기

---

### 워크벤치에서 RDS 접속하기

전에 생성한 데이터베이스에 들어가서 연결 & 보안 탭에 
엔드포인트 DNS 주소를 복사한다.

이 엔드포인트를 통해서, 워크벤치에서 AWS로 데이터베이스를 연결하여
원격 조작을 행할 수 있다.

![Untitled](https://user-images.githubusercontent.com/84123877/203323826-cea5115d-f213-4e85-8618-7536b80c13bf.png)

![Untitled 1](https://user-images.githubusercontent.com/84123877/203323797-a0cfa7db-3730-4bc4-a869-8959d586be7d.png)

> RDS 접속 전용 Connection을 새로 하나 생성한다.
> 

![Untitled 2](https://user-images.githubusercontent.com/84123877/203323806-8bcd5332-ca14-4770-bf3d-38aa0e122f04.png)

> 이름을 작성하고, 엔드포인트를 붙여넣는다.
> 

![Untitled 3](https://user-images.githubusercontent.com/84123877/203323808-1f2ce168-5a1a-4eac-80ab-1fc6945573b7.png)

![Untitled 4](https://user-images.githubusercontent.com/84123877/203323810-fbc60dbd-c9b7-4a63-8a6f-e100d99143f8.png)

> RDS 접속 전용 Connection이 성공적으로 생성되었다.
> 

![Untitled 5](https://user-images.githubusercontent.com/84123877/203323813-25ab5865-bf25-438e-9821-40bbc2ca98a9.png)

```sql
use testDB;
create table test (
	id bigint(20) NOT NULL auto_increment,
    content varchar(255) default NULL,
    primary key(id)
) engine = innoDB;
 
insert into test(content) values('한글 테스트');
select * from test;
```

> 한글이 잘 나오는지 테스트하기 위해, 해당 쿼리로 테이블을 만들고
한글 문자 데이터를 넣고 조회를 진행해보았다.
> 

---

## EC2에서 RDS 접근하기

---

EC2 CLI에 접속해서 MySQL 접속 명령어를 입력하면 RDS에 접속할 수 있게 된다.

```bash
$ sudo -s
$ yum -y install mysql # mysql 설치
```

```bash
$ mysql -u {마스터 사용자 이름} -p -h {RDS 인스턴스 엔드포인트} # mysql cli 접속하기
```

![Untitled 6](https://user-images.githubusercontent.com/84123877/203323815-9c532502-0b53-4900-93eb-f807b580f0d0.png)

## RDS fail-over 테스트

---

멀티 AZ 환경에서 Primary 데이터베이스 인스턴스가 죽을 경우,
fail over(장애 복구)가 잘 되는지 테스트를 진행하겠다.

![Untitled 7](https://user-images.githubusercontent.com/84123877/203323816-eb1154be-e77a-4887-8109-ab99b5c4b25c.png)

우선 같은 VPC내 EC2 인스턴스에 접속한다.

이후 다음 명령어를 실행한다.

```bash
# while true; do host <데이터베이스 엔드포인트> | grep alias; sleep 1; done
$ while true; do host mydb.cauufra5lcmt.ap-northeast-2.rds.amazonaws.com | grep alias; 
sleep 1; done
```

생성한 RDS 인스턴스의 
cwdb.ca2a6cnmgn9e.ap-southeast-2.rds.amazonaws.com 엔드포인트가
어떤 인스턴스를 가리키고 있는지 1초마다 인스턴스 DNS를 무한 반복으로 출력하는
명령어이다.

![Untitled 8](https://user-images.githubusercontent.com/84123877/203323818-5add0142-c376-4475-9049-86a8dccbcb93.png)

> 이를 통해 DNS가 어떤 RDS 인스턴스를 가리키고 있는지 알 수 있다.
> 

위 사진에서 보듯 EC2-54-206-233-73 이라는 인스턴스 DNS를 가리키고 있다는 걸 알 수 있다.

이제 데이터베이스 인스턴스를 재부팅해보겠다.

데이터베이스 서버를 죽일수는 없으니, 장애 때문에 Primary 인스턴스가 떨어져 나가는
듯한 효과를 재부팅을 통해 진행하려는 것이다.

![Untitled 9](https://user-images.githubusercontent.com/84123877/203323822-9a879e14-097c-4d3c-8ebe-b7adc51a5464.png)

![Untitled 10](https://user-images.githubusercontent.com/84123877/203323823-ac4e393d-9504-4d11-8fdb-295717f2f876.png)

재부팅 이후 조금만 대기하면 터미널에 다음과 같이 EC2 아이디가 변경되는것을 확인 할 수 있다.

이것이 fail over를 통해 인스턴스의 DNS를 Stanby 인스턴스에게 자동으로 연결됨을 
보여주는 것이다.

![Untitled 11](https://user-images.githubusercontent.com/84123877/203323824-d9c07bfb-9494-448d-8686-8bedebc7acfd.png)

---
