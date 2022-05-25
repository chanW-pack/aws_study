# Linux 가상 머신 시작

AWS 계정을 생성하면 12개월동안 월별 750시간의 프리티어가 제공되어 개인적으로 학습하기에는 안성맞춤이었다.

aws 홈페이지에 아마존 튜토리얼 페이지 (실습 자습서) 가 있어 쉽게 따라할 수 있고, 이를 참고하여 복습 겸 작성하였다.

로그인을 하면 첫 페이지가 나타난다.

![Untitled](https://user-images.githubusercontent.com/84123877/170216786-b85a812e-f445-44c4-8a59-a9da8f8136ab.png)

> (22.5.4)얼마 전까지만 해도 위와 같은 형식이였는데 6월부터 아래와 같이 바뀐다고 한다.
> 

![Untitled 1](https://user-images.githubusercontent.com/84123877/170216716-6b015071-21a1-4f3d-a6ae-4ba3c80d48ac.png)

미리 익숙해질 겸 바뀐 버전으로 진행해보기로 했다.

하단 솔루션 구축 위젯에 가상 머신 시작 이라는 버튼이 있다.

(*EC2 로 자동 선택됨)

본인은 EC2로 진행했지만 aws 튜토리얼에서는 Amazon Lightsatil 서비스를 사용하는데 무엇이 다른건가?

### Amazon EC2와 Amazon Lightsail의 차이점

[Amazon EC2와 Amazon Lightsail의 차이점]((https://github.com/chanwoo9730/aws_study/blob/main/Week%201/Amazon%20EC2%EC%99%80%20Amazon%20Lightsail%EC%9D%98%20%EC%B0%A8%EC%9D%B4%EC%A0%90.md))

---

## EC2 인스턴스 시작

![Untitled 2](https://user-images.githubusercontent.com/84123877/170216722-3ec2d0df-813e-452f-95e0-5c1addb37272.png)

> EC2 대시보드 화면. 인스턴스 시작 버튼으로 생성이 가능하다.
> 

![Untitled 3](https://user-images.githubusercontent.com/84123877/170216725-15082761-8238-4f8c-b6ad-33b417f9976c.png)

> 리전 : 실제 서버가 위치한 지역으로 이해했다. 서비스 할 위치에 맞게 설정하자.
> 

![Untitled 4](https://user-images.githubusercontent.com/84123877/170216726-e4b5abd3-3367-4abc-9411-b6735843fcf1.png)

> 인스턴스 이름 및 태그 설정
> 

해당 인스턴스의 이름, 태그를 생성 및 추가할 수 있다.

![Untitled 5](https://user-images.githubusercontent.com/84123877/170216728-f0cb1245-bc45-4aa4-80f6-e824948e5441.png)

> 애플리케이션 및 OS 이미지 (Amazon Machine Image)
> 

Amazon EC2에서는 사용하려는 인스턴스의 사양과 소프트웨어를 지정할 수 있다.

*AMI란?

AMI는 소프트웨어 구성 (예: 운영 체제, 애플리케이션 서버 및 애플리케이션) 이 포함된 탬플릿

AMI에서 인스턴스를 바로 시작할 수 있는데, 이 인스턴스는 AMI의 사본으로, 클라우드에서 실행되는 가상 서버다.

이것 외에도 더 있는데 본인은 Amaznon Linux AMI 을 사용했다.

![Untitled 6](https://user-images.githubusercontent.com/84123877/170216732-81f088f8-c6bb-4447-982f-c82de2562001.png)

> 인스턴스 유형 선택
> 

인스턴스 유형은 다양한 CPU, 메모리, 스토리지 및 네트워킹 용량의 조합으로 구성되므로, 애플리케이션에 맞는 적절한 조합을 선택할 수 있다.

다만 본인은 기본값으로 설정된 t2 micro를 사용. (프리티어 사용 가능함. 나머지는 돈줘야댐....)

![Untitled 7](https://user-images.githubusercontent.com/84123877/170216734-47e9684d-f08a-4c5d-a987-7ee698a470f5.png)

> 키 페어 설정
> 

키 페어는 인스턴스에 로그인하는데 사용된다.

집 열쇠가 집에 들어가는데 사용되는 것과 같다.

새 키 페어 생성을 선택하고 키 페어 이름은 chanwoo로 했다. 명칭은 임의로 지정.

그리고 강조된것을 보면 알 수 있겠지만, 반드시 키 페어를 다운로드 해야 인스턴스를 시작할 수 있다. 다운로드한 Keypair 파일을 컴퓨터의 안전한 위치에 저장한다.

내가 참고한 글에선 “.ssh” 폴더 하위에 저장하는 것이 좋다고 하는데 이유는 잘 모르겠지만 동일하게 진행했다.

![Untitled 8](https://user-images.githubusercontent.com/84123877/170216738-0375ee89-b124-4c58-9753-6c0928648dc5.png)

~ **.ssh 폴더 생성 방법**

- Windows Explorer에서는 폴더 이름이 마침표로 끝나지 않는 한 마침표로 시작하는 폴더 이름을 생성할 수 없습니다. 이름(.ssh.)을 입력하면 마지막에 있는 마침표가 자동으로 제거됩니다.

키 페어 다운로드 진행.

![Untitled 9](https://user-images.githubusercontent.com/84123877/170216740-52dafae6-7a57-490e-bb53-c199b3241a74.png)

> 네트워크 설정 영역
> 

인스턴스에 연결될 네트워크를 설정할 수 있다. 

직접 VPC를 만들어 연결 가능하며, 본인은 디폴트 값으로 진행했다.

또한 하단에 SSH 트래픽, HTTPs 트래픽 등을 허용시키는 선택박스가 있는데

이를 선택하여 진행하면 해당 포트가 열리는 방식이다. (ex 22, 8080)

![Untitled 10](https://user-images.githubusercontent.com/84123877/170216743-b78f1a27-752e-4178-ad3c-9662be30f625.png)

> 스토리지 구성 가능
> 

인스턴스에 관한 스토리지 옵션을 지정할 수 있다.

SSD 등 디스크를 추가 제거하는 것이라고 보면 된다.

(새 볼륨을 추가하거나 성능이 좋을수록 요금이 올라간다.)

EC2를 종료하더라도 볼륨은 따로 중지, 제거해야 한다.

---

그 외 고급 세부 정보와 현재까지의 설정 요약이 있다.

확인하고 인스턴스 시작을 진행한다.

![Untitled 11](https://user-images.githubusercontent.com/84123877/170216745-081b2c6c-4e0d-48e5-b1a6-1cab639ae4de.png)

> 인스턴스 생성이 완료되었다.
> 

![Untitled 12](https://user-images.githubusercontent.com/84123877/170216748-6a7f63f6-6daa-430f-ada3-78f140a220ad.png)

> 상태 검사를 기다리고 (약 1분) 작동 중인 것을 확인 가능하다.
> 

![Untitled 13](https://user-images.githubusercontent.com/84123877/170216753-530b074b-a335-4094-a7bd-b03a53dfae3f.png)

> 인스턴스 세부 정보
> 

해당 인스턴스에 접속을 원할 시 세부 정보의 퍼블릭 IPv4 주소로 접속하게 되면 된다.

기본적으로 IPv4주소는 유동적이다. (고정되어 있지 않다.)

즉, 인스턴스를 중지, 실행 할 때마다 IPv4 주소가 변환된다.

이를 고정 IP로 만들기 위해서는 네트워크 및 보안 탭의 탄력적 IP를 생성하여 부여하면 된다.

![Untitled 14](https://user-images.githubusercontent.com/84123877/170216761-524512cd-12dd-4611-9132-1d0e94240158.png)

> 탄력적 IP 주소 할당
> 

고정적인 IP 주소를 사용하게 되면, 이 IP를 사용하고 있지 않을 때 비용이 청구된다.

(IPv4주소의 자리가 부족하여 사용하고 있지 않을 때 즉, 낭비되고 있을 때 비용이 청구되는 듯 하다.)

사용중일때에는 탄력적 IP 주소 할당에 대한 비용은 청구되지 않는다는 이야기로,

인스턴스에서 고정적인 IP가 필요하지 않을 때에는 사용을 멈춰야 비용적인 측면에서의 효율을 높일 수 있다.

---

## 생성한 인스턴스로 SSH 접속

![Untitled 15](https://user-images.githubusercontent.com/84123877/170216765-9946ae93-f0f5-4155-9db6-f1da853d899e.png)

퍼블릭 IPv4 의 아이피로 접속한다.

EC2의 기본적인 사용자명은 ec2-user 이기 때문에

번거로운 로그인 작업을 패스하고자 사용자명을 입력시켰고,

키 파일을 생성할때 다운로드된 .pem 파일을 넣어 인증을 완료한다.

![Untitled 16](https://user-images.githubusercontent.com/84123877/170216768-789be375-af95-4f47-a8d4-94626659820c.png)

> 성공적으로 접속이 된 모습
> 

여기까지 AWS EC2 인스턴스를 생성하고 SSH 연결까지 완료하였다.

인스턴스를 중지하고 시작할 때마다 동일한 IP 주소를 유지하러면

탄력적 IP 주소를 생성하고,

백업으로 유지하러면 인스턴스의 스냅샷을 생성할 수 있다.

스토리지 공간을 추가하러면 인스턴스에 디스크를 연결(볼륨 추가) 하면 되겠다.

---
