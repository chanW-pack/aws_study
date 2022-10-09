# AWS RDS 타임존, 문자셋, collation 설정

---


![Untitled 13](https://user-images.githubusercontent.com/84123877/194469573-2e001647-4134-4e72-b29d-5ad370a4a526.png)

![Untitled 14](https://user-images.githubusercontent.com/84123877/194469574-f1bb2e0d-01f1-466a-98ba-a1a53b562e43.png)

![Untitled 15](https://user-images.githubusercontent.com/84123877/194469575-fb6f3033-8f42-41a9-bee0-f64c522d9b75.png)

> RDS 생성 중 마스터 패스워드를 지정해주었다.
마스터 사용자 이름과 마스터 암호를 이용해서 데이터베이스에 접속할 수 있게 되었다.
> 

![Untitled 16](https://user-images.githubusercontent.com/84123877/194469576-8f8bd11d-f530-4eb7-87ab-6d4f97073c78.png)

> 파라미터 그룹을 생성한다. (파리미터 그룹 → 파라미터 그룹 생성 선택)
> 

RDS 인스턴스를 생성하면서 기본으로 생성된 **디폴트 파라미터 그룹은 수정이 불가하다.**

그래서 파라미터 그룹을 새로 만들어 데이터베이스 인스턴스에 적용해야 한다.

[파라미터 그룹 생성] 버튼을 누른다.

![Untitled 17](https://user-images.githubusercontent.com/84123877/194469578-d6a46646-2e26-4412-b5ed-e19f05762228.png)

> 파라미터 그룹을 생성한다.
> 

파라미터 그룹 패밀리는 생성한 데이터베이스의 버전과 맞춰주면 된다.

이후 그룹 이름과 설명을 작성한다.

![Untitled 18](https://user-images.githubusercontent.com/84123877/194469580-73cbe2c6-17e2-40f5-bbd7-f5999e8593a5.png)

![Untitled 19](https://user-images.githubusercontent.com/84123877/194469581-5691897b-5c82-4252-af52-43924d4dd30a.png)

### 데이터베이스 타임존 설정

![Untitled 20](https://user-images.githubusercontent.com/84123877/194469584-7d1b8f20-79bb-4f36-8126-2f7be9a93dc8.png)

> **데이터베이스 타임존**을 설정했다.
> 

생성한 파라미터 그룹에서 파라미터 편집으로 이동한 뒤,
검색창에 time_zone을 검색하여 time_zone 파라미터를 
Asia/Seoul로 변경해준다.

### 데이터베이스 문자셋 설정

![Untitled 21](https://user-images.githubusercontent.com/84123877/194469586-b2bf82fd-51dd-430b-beed-a8d72c4487cd.png)

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

![Untitled 22](https://user-images.githubusercontent.com/84123877/194469589-b12fa0b4-070d-4f51-871b-aadec1bc4a46.png)

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
