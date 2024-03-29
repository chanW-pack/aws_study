# AWS S3 버킷 로그 설정

---

## Amazon S3 버킷 액세스 로그란?

---

![Untitled](https://user-images.githubusercontent.com/84123877/182093559-f142d141-f073-4109-ab14-95f3bb431fea.png)

S3에 저장된 객체의 접속 로그(Logging)를 텍스트 파일 형태로 저장할 수 있다.

즉, 

- 버킷에 이루어진 요청에 따라 상세 레코드를 제공하는 기능
- 액세스 로그 버킷을 생성하여 로그를 객체로 저장한다.
- 액세스 로그 정보의 유용성은 다음과 같다.
    - 보안 및 액세스 감사
    - 고객 기반을 이해
    - Amazon S3 청구 비용 파악

![Untitled 1](https://user-images.githubusercontent.com/84123877/182093534-a9593307-acd1-40a9-afa7-a410cdeac730.png)

> 사용자의 버킷과 액세스 로그 버킷이 동일한 경우 로그가 계속해서 들어오게 되는
로깅 루프(logging loop)가 일어날 수 있으니 주의해야 한다.
> 

## S3 버킷 생성

---

먼저 S3 버킷을 생성한다.

S3 버킷은 위의 그림대로 액세스 로그 버킷을 따로 생성해야 하기 때문에

총 2개의 버킷을 생성하겠다.

[이미지 파일이 있는 버킷]

버킷 이름 : cw-test-1234

모든 퍼블릭 액세스 차단을 해제한다.

버킷 버전 관리를 활성화한다.

![Untitled 2](https://user-images.githubusercontent.com/84123877/182093540-f6bd65c2-713c-4722-b9e8-1cd2d86744e7.png)

> 생성 후 버킷 정책에 다음과 같이 입력했다.
(본인의 버킷 ARN으로 수정해서 입력하세욤^^)
> 

![Untitled 3](https://user-images.githubusercontent.com/84123877/182093541-a19bdf14-cc9c-482f-9436-caa36ec94413.png)

> 액세스를 시험해보기 위해 임의의 파일을 올려본다.
> 

로그가 저장될 버킷을 생성하였다.

버킷 이름 : cw-logging-test

![Untitled 4](https://user-images.githubusercontent.com/84123877/182093542-e2f8c86f-340b-4da3-95c2-0653bd699e90.png)

## 액세스 로그 버킷 설정

---

두 개의 버킷을 생셩하였고,

cw-test-1234 버킷에서 액세스가 발생했을 경우

cw-logging-test 버킷에 로그가 저장되도록 설정하겠다.

버킷의 속성에서 서버 엑세스 로킹 편집으로 이동한다.

![Untitled 5](https://user-images.githubusercontent.com/84123877/182093544-fbb88084-83a6-4a82-901e-19bf1a0e625b.png)

> S3 버킷 로그 설정
> 

- Enabled(서버 액세스 로깅) 
: 로깅에 대한 활성/비활성 설정
- Target Bucket(대상 버킷)
: 현재 선택된 버킷의 로그를 남길 수도, 다른 버킷의 로그를 남길 수도 있다.
목록(S3 찾아보기)를 클릭하면 선택할 수 있는 버킷 목록이 표시된다.

![Untitled 6](https://user-images.githubusercontent.com/84123877/182093545-ae4ccfe4-5fcd-44ce-af02-c30886f745ad.png)

> 활성화 선택 후 로그를 저장할 대상 버킷을 선택할 수 있는 칸이 나오는데
cw-logging-test 버킷을 선택한다.
> 

버킷경로 뒤에 /s3-logs/ 를 추가하고 변경사항을 저장하였다.

![Untitled 7](https://user-images.githubusercontent.com/84123877/182093547-0e9585cb-40da-4959-882c-dc706bdb9f28.png)

또한, 서버 액세스 로깅을 활성화하면 해당 버킷의 버킷정책이 자동으로 추가된다.

![Untitled 8](https://user-images.githubusercontent.com/84123877/182093553-baef8c9f-fade-427a-a344-8496b83ef624.png)

> Service : [logging.s3.amazons.com](http://logging.s3.amazons.com) 같이 정책들이 추가되었다.
> 

<aside>
💥 액세스 로그 설정 후 10~20분이 지나야 로그파일이 생성된다!!!!!!!!!!!!
(본인은 오류로 미생성되는줄알고 계속 시도하다가 로그파일 폭탄을 맞았다.)
~~그래도 많이생성되진 않았다.~~

</aside>

![Untitled 9](https://user-images.githubusercontent.com/84123877/182093556-e3fb25f2-d0ab-41ae-becf-8353e3435471.png)

> cw-test-1234 버킷에 대한 로그가 저장되었다.
> 

![Untitled 10](https://user-images.githubusercontent.com/84123877/182093557-928c401a-724f-48e1-8b62-7dcc7d334db8.png)

> 파일을 열어서 로그 내용들을 확인할 수 있다.
> 

- 로그의 내용 분석
    - 원본 버킷의 정식 사용자 ID : 306f318a2feb3ea2256ba7d4f0ebd322d3b9ec7d93f0d3e5311a19bee92629ce
    - 요청이 처리된 버킷의 이름 : user-bucket-202108
    - 요청이 수신된 시간 : [19/Aug/2021:16:54:07 +0000]
    - 요청자의 명백한 인터넷 주소 : 122.38.73.237
    - 요청자의 정식 사용자 ID : arn:aws:sts::************:assumed-role/cm-lim.chaejeong/cm-lim.chaejeong
    - 요청의 고유 식별을 위한 문자열 : AW98WX6NX1W7TNYK
    - 작업 내용 : REST.HEAD.BUCKET
    - 요청의 키(파라미터가 없을 경우 -로 표시) : -
    - HTTP 요청 메시지의 Request-URI : "HEAD /user-bucket-202108 HTTP/1.1"
    - 응답의 숫자 HTTP 상태 코드 : 200
    - 오류 코드 : -
    - HTTP 프로토콜 오버헤드를 제외한 보낸 응답 바이트 수 : -
    - 해당 객체의 총 크기 : -
    - 요청이 수신된 시간부터 응답의 마지막 바이트가 전송된 시간 : 7
    - 요청을 처리하는 데 소비한 시간 : 6
    - HTTP Referrer 헤더의 값 : "-"
    - HTTP User-Agent 헤더의 값 : "S3Console/0.4, aws-internal/3 aws-sdk-java/1.11.1002 Linux/5.4.129-72.229.amzn2int.x86_64 OpenJDK_64-Bit_Server_VM/25.282-b08 java/1.8.0_282 vendor/Oracle_Corporation cfg/retry-mode/legacy"
    - 요청의 버전 ID : -
    - x-amz-id-2 또는 Amazon S3 확장 요청 ID : BOY/uf8pOEOnp+HGW+UejMdD42FTXDPRMtGVECDorULxq4RYnp57dXbiLuF+8U/gGXQRpSn1Cjw=
    - 요청을 인증하는 데 사용된 서명 버전 : SigV4
    - HTTPS 요청에 대해 협상된 Secure Sockets Layer (SSL) 암호 : ECDHE-RSA-AES128-GCM-SHA256
    - 사용된 요청 인증 유형 : AuthHeader
    - Amazon S3에 연결하는 데 사용된 엔드포인트 : s3-ap-northeast-2.amazonaws.com
    - 클라이언트가 협상한 TLS(전송 계층 보안) 버전 : TLSv1.2 -
    

로그를 나누어 확인해보니 수많은 정보가 포함되어 있었다.

이렇게 로그를 분석할수 있는 것만으로 많은 정보를 확인할 수 있기 때문에 유용하게 사용 가능하다.

---
