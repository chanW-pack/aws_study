# EC2 인스턴스 접속을 위한 키 쌍

**키 쌍(Key Pair)**은 EC2 인스턴스에 접속하기 위해 사용하는 **암호화된 파일**이다.

AWS에서는 보안 문제 때문에 EC2 인스턴스에 접속할 때 ID, 비밀번호의 방식을 권장하지 않는다.

키 쌍 접속 방식은 ID와 비밀번호를 분실하지만 않으면 매우 안전하다.

이 키 쌍은 RSA 공개 키 알고리즘(Public Key Algorithm)을 사용하고 있다.

암호화 방식에는 두 가지가 있는데 **대칭 키**와 **공개 키** 방식이 대표적이다.

대칭 키 방식은 암호화(Encyption)하는 키와 복호화(Decyption)하는 키가 동일하여 

이 대칭 키를 상대방에게 전달하는 구간에서 **키가 노출되는 단점**이 있다.

공개 키 방식은 이러한 단점을 개선하여 암호화하는 키와 복호화하는 키를 다르게 설계하였다.

![Untitled](https://user-images.githubusercontent.com/84123877/175291680-6b3ea35d-d9b1-4fb0-8c65-159460af1871.png)

> 공개 키 암호화 알고리즘
> 

<aside>
💡 **무차별 대입 공격(Brute force attack)**
Linux와 같이 SSH를 이용하여 터미널로 접속하는 OS는 로그인할 때마다 ID와 비밀번호를 입력하게 된다. 이런 경우 ID와 비밀번호를 반복적으로 대입하는 공격에 노출 될 수 있다.

</aside>

## 키 페어 생성

---

![Untitled 1](https://user-images.githubusercontent.com/84123877/175291653-774d99b2-fd2d-4c54-91a6-238961b9aa0f.png)

> 키 페어로 검색 후 키 페어 (EC2 기능) 선택
> 

![Untitled 2](https://user-images.githubusercontent.com/84123877/175291659-7ef21677-5607-47fb-ac87-b85d1f539a03.png)

> 우측 상단에 키 페어 생성을 선택한다.
> 

![Untitled 3](https://user-images.githubusercontent.com/84123877/175291660-7e0b4837-ec7f-40ce-8239-c7a667145991.png)

> 키 페어 이름 입력 → .ppk 선택 → 키 페어 생성
> 

![Untitled 4](https://user-images.githubusercontent.com/84123877/175291662-de7b7ca3-f758-486b-a0e9-56b4aeb4da4b.png)

> 생성하면 .ppk 파일이 다운로드 받아진다.
> 

![Untitled 5](https://user-images.githubusercontent.com/84123877/175291664-9b11d51e-2d1e-41a1-b2bc-bcd89e77512a.png)

> PuTTYgen 실행하여 Load → .ppk 파일을 불러온다.
> 

![Untitled 6](https://user-images.githubusercontent.com/84123877/175291665-8e296df2-8947-4522-8ddf-da1df9c418bf.png)

> 키 페어 내용을 복사한다.
> 

# EC2 - SSH

![Untitled 7](https://user-images.githubusercontent.com/84123877/175291668-4cdf3945-6b0d-41e0-a636-827fabb8bf77.png)

![Untitled 8](https://user-images.githubusercontent.com/84123877/175291672-7212bba8-f51f-43ee-a421-e488bbec80ac.png)

> 기존 키 페어로 EC2 SSH에 접속 후 .ssh/authrozed_keys에 복사한 키 페어 내용을
추가한다.
> 

# Putty

![Untitled 9](https://user-images.githubusercontent.com/84123877/175291674-68d2705b-eefe-497a-b2be-e4edba59b014.png)

> 퍼블릭 IPv4 DNS 입력
> 

![Untitled 10](https://user-images.githubusercontent.com/84123877/175291677-c630b63d-d3fe-46a2-84cf-e4cb0f631b3d.png)

> Connection → SSH → Auth
Browser → .ppk 파일을 불러온 후 Opne
> 

![Untitled 11](https://user-images.githubusercontent.com/84123877/175291679-b887e357-7439-4cf3-9e45-19e30c0e35b6.png)

> 인스턴스 계정으로 접속 확인
> 

---
