# AWS CDN CloudFront

---

## CloudFront

---

CloudFront는 개발자 친화적 환경에서 짧은 지연 시간과 빠른 전송 속도로
데이터, 동영상, 애플리케이션 및 API를 전세계 고객에게 안전하게 전송하는
고속 컨텐츠 전송 네트워크(CDN) 서비스이다.

CloudFront는 CDN 서비스와 이외에도 기본 보안 기능(Anti-DDOS)을 제공한다.

![Untitled](https://user-images.githubusercontent.com/84123877/183291445-6d061980-334d-4d5a-adf3-de5ce1007c95.png)

## CDN이란?

---

**CDN(Content Delivery Network or Content Distribution Network, 컨텐츠 전송 네트워크)**
은 **컨텐츠를 효율적으로 전달하기 위해** 여러 노드를 가진 네트워크에 데이터를 저장하여 제공하는
시스템이다.

인터넷 서비스 제공자(ISP, Imternet Service Provider)에 직접 연결되어 데이터를 전송하므로,
컨텐츠 병목을 피할 수 있는 장점이 있다.

<aside>
💡 제이쿼리 같은 라이브러리를 링크 src로 불러오는 것 등이 CDN 서비스를 이용하는 것이다.

</aside>

![Untitled 1](https://user-images.githubusercontent.com/84123877/183291422-dbf2c5f7-88d0-4d60-ac97-54541a405f95.png)

> CloudFront 전 세계 여러 곳에 위치한 엣지 서버
> 

## CDN 특징

---

- 웹 페이지, 이미지, 동영상 등의 컨텐츠를 본래 서버에서 받아와 **캐싱**
- 해당 컨텐츠에 대한 요청이 들어오면 캐싱해 둔 컨텐츠 제공
- 컨텐츠를 제공하는 서버와 실제 요청 지점 간의 지리적 거리가 매우 먼 경우
or 통신 환경이 안좋은 경우 → 요청지점의 CDN을 통해 빠르게 컨텐츠 제공 가능
- 서버의 요청이 필요 없기 때문에 **서버의 부하를 낮추는 효과**

## 엣지 로케이션

---

- 컨텐츠가 캐싱되고 유저에게 제공되는 지점
- AWS가 CDN을 제공하기 위해서 만든 서비스인 CloudFront의 캐시 서버
(데이터 센터의 전 세계 네트워크)
- **CloudFront 서비스는 엣지 로케이션을 통해 컨텐츠 제공**
- CloudFront를 통해 서비스하는 컨텐츠를 사용자가 요청하면 지연 시간이 가장 낮은
엣지 로케이션으로 라우팅되므로 컨텐츠 전송 성능이 뛰어나다.
- 컨텐츠가 **이미 지연 시간이 가장 낮은 엣지 로케이션에 있는 경우 CloudFront가 컨텐츠를 
즉시 제공**

![Untitled 2](https://user-images.githubusercontent.com/84123877/183291424-d3c29eee-03d0-41dc-9244-d5567d7db4ff.png)

## CloudFront 동작 방식

---

CloudFront는 AWS 백본 네트워크를 통해 컨텐츠를 가장 효과적으로 서비스할 수 있는
엣지로 각 사용자 요청을 라우팅하여 컨텐츠 배포 속도를 높인다.

일반적으로 CloudFront 엣지가 최종 사용자에게 가장 빨리 제공한다.

1. 컨텐츠가 엣지 로케이션에 **없는** 경우
    - CloudFront는 컨텐츠의 최종 버전에 대한 소스로 지정된 오리진
    (Amazon S3 버킷, MediaPackge 채널, HTTP서버(예:웹 서버)등) 에서 컨텐츠를 검색
    - 컨텐츠를 제공하는 근원에서 제공받아 전송
2. 컨텐츠가 엣지 로케이션에 **있는** 경우
    - 바로 전달
    

## CloudFront 데이터 전달 과정

---

![Untitled 3](https://user-images.githubusercontent.com/84123877/183291425-73d3d167-d341-4bb8-81d6-96b9280d449e.png)

1. 클라이언트로부터 Edge Server로의 요청이 발생한다.
2. Edge Server는 요청이 발생한 데이터에 대하여 **캐싱 여부를 확인**
3. 사용자의 근거리에 위치한 Edge Server 중 캐싱 데이터가 존재한다면 사용자에 요청에 맞는
데이터를 응답. 사용자의 요청에 적합한 데이터가 캐싱되어 있지 않은 경우 Origin Server로
요청이 포워딩
4. 요청받은 데이터에 대해 Origin Server에서 획득한 후 Edge Server에 캐싱 데이터를 생성하고,
클라이언트로 응답이 발생

## CloudFront 장점

---

- AWS 네트워크를 사용하면 사용자의 요청이 반드시 통과해야 하는 네트워크의 수가 
줄어들어 성능이 향샹
- 파일의 첫 바이트를 로드하는데 걸리는 지연 시간이 줄어들고 데이터 전송 속도가 빨라진다.
- 파일(객체)의 사본이 전 세계 여러 엣지 로케이션에 유지(또는 캐시)되므로 안정성과 
가용성이 향샹된다.
- 보안성 향샹
    - 오리진 서버에 대한 종단 간 연결에 보안이 보장됨(HTTPS)
    - 서명된 URL 및 쿠키 사용 옵션으로 자체 사용자 지정 오리진에서 프라이빗
    컨텐츠를 제공하도록 할 수 있음

![Untitled 4](https://user-images.githubusercontent.com/84123877/183291426-4adfa907-94ae-46da-aaa4-6a210c868141.png)

> 초록색 바(CloudFront)가 1,2,3,4로 갈수록 요청 시간(ms)가 줄어듬을 볼 수 있다.
> 

## CloudFront 다양한 기능

---

### 정적 & 동적 컨텐츠 분별 제공

**정적(Static) 컨텐츠**

- 서버를 거치지 않고 클라이언트에서 직접 보여주는 내용
ex) 이미지, CSS, 기타 서버가 필요없는 내용들
- **캐싱**으로 접근속도 최적화

**동적(Dynamic) 컨텐츠**

- 서버 계산, DB 조회 등이 필요한 내용
ex) 로그인, 게시판 등
- 네트워크 최적화, 연결 유지, Gzip 압축 등을 사용
- 서버랑 통신을 할 때 전처리 작업이 있는데, 주소가 어디로 전달되는지(NDS Lookup),
TCP Connection, Time to First Byte 등을 CloudFront에서 네트워크를 최적화한다.
- 실제로 내용을 최적화 해서 보내는 것이 아니라, 통신을 최적화 해서 **속도를 최적화** 시키는 것.

![Untitled 5](https://user-images.githubusercontent.com/84123877/183291427-1b9c0476-3918-46de-b8ac-54c1a0b71b30.png)

> 준비단계를 최적화 시켜 Content Download를 빠르게 한다.
> 

**동적/정적 컨텐츠 처리**

경로 패턴으로 URL에 따라 정적/동적 컨텐츠 분기 처리 한다.

![Untitled 6](https://user-images.githubusercontent.com/84123877/183291428-6258b410-f0bf-4dfd-8cf8-50a0ae0d7572.png)

### HTTPS 지원

Origin에서 HTTPS를 지원하지 않더라도 클라우드 프론트내에서 HTTPS 통신을 지원할 수 있도록
구성 가능하다. 

예를 들어, S3 정적 웹 호스팅 URL 같은 경우 SSL 설정이 쉽지 않은데, CloudFront를 통해서
HTTPS 통신을 지원할 수 있게끔 할 수 있다.

![Untitled 7](https://user-images.githubusercontent.com/84123877/183291430-fb1360eb-2416-4de2-b093-44abbcc3f765.png)

### 지리적 제한 설정

특정 지역에 컨텐츠 접근을 제한 가능 하다.

예를 들어 넷플릭스 스트리밍 서비스를 하는데 라이센스나 계약에 따라 일본권에서는 볼 수 있지만
중국권은 볼 수 없게 설정 가능하다.

![Untitled 8](https://user-images.githubusercontent.com/84123877/183291431-565c9f89-c3a2-4388-9d7c-42bc8632e859.png)

### 다른 서비스와 연계

AWS WAF, Lanbda@Edge 등과 연동 가능

**Lambda@Edge**

- 엣지 로케이션에서 돌아가는 람다
- 람다엣지 사용 사례
    - 한국에서 요청이 올 경우 한국 웹서버, 미국에서 요청이 올 경우 미국 웹서버로 분산
    - 커스텀 에러 페이지
    - Cookie를 검사해 다른 페이지로 리다이렉팅 → A/B 테스팅
    - CloudFront에서 Origin 도착 이전에 인증 .. 등
    

![Untitled 9](https://user-images.githubusercontent.com/84123877/183291434-45c798d8-f4a2-4530-8138-b9f34f6612f0.png)

- 유저에서 CloudFront 도착하기 전,
- CloudFront에서 Origin에 요청 보내기 전,
- Origin에서 CloudFront로 응답을 보내기 전,
- CloudFront에서 유저한테 응답 보내기 전

이 4단계에서 람다를 실행해서 이 전달 내용을 변경 할 수 있다.

### CloudFront Function

- Lambda@Edge의 6분의1 비용으로 경량 Javascript 실행
- 아주 간단한 액션에서 사용
- 사용사례 : 캐싱, 헤더 조작 등

![Untitled 10](https://user-images.githubusercontent.com/84123877/183291435-fea29ae3-b1a0-4c9b-aec2-4baa81e314fa.png)

![Untitled 11](https://user-images.githubusercontent.com/84123877/183291436-53408c15-8539-4344-9672-a22096d7eab4.png)

### CloudFront 리포팅

- 주요 CloudFront 이용 지표 확인 가능
ex) 캐시 상태, 가장 많이 요청 받은 컨텐츠, Top Referer
- **구글 애널리틱스 같은것이라고 보면 된다.**

![Untitled 12](https://user-images.githubusercontent.com/84123877/183291437-d92497ed-2658-4cd6-a889-f2c8f3dadd97.png)

![Untitled 13](https://user-images.githubusercontent.com/84123877/183291439-eacc36d3-c1ca-47bb-ab84-8e27af6f292f.png)

### CloudFront 뷰어 정보

CloudFront에서 뷰어의 정보를  헤더에 더해 Origin에 전송한다.

- 디바이스 타입 (Android / IOS / SmartTV / Desktop / Tablet)
- IP Address
- Country / 도시 / 위도 / 경도 / 타임존 등

<aside>
💡 이 뷰어 정보를 통해서 어떤 Country에서는 어떤 컨텐츠를 제공한다던지 할 수 있다.

</aside>

## CloudFront 보안

---

### Sigend URL

어플리케이션에서 CloudFront의 컨텐츠에 접근할 수 있는 URL을 제공하여
컨텐츠 제공을 제어하는 방법이다.

- URL에는 시작시간, 종료시간, IP, 파일명, URL의 유효기간 등의 정보를 담을 수 있음
- 이 URL 접근 이외의 접근을 막고 허용된 유저에게만 URL을 전달하여 컨텐츠 제공을 
제어 가능
- **단 하나의 파일** 또는 컨텐츠에 대한 허용만 가능
- **S3 Signed URL과 비슷한 방식**

![Untitled 14](https://user-images.githubusercontent.com/84123877/183291440-89241b61-7e59-4441-8046-35c9df42e2ef.png)

### **Signed Cookie**

Signed URL은 하나의 컨텐츠만 제공 제어를 한다고 하면,
Signed Cookie는 다수의 컨텐츠의 제공방식을 제어하고 싶을 때 사용된다.

- Signed URL과 마찬가지로 여러 제약사항 설정 가능
- 다수의 파일 및 스트리밍 접근 허용 가능
- 사용사례 : 정기 구독 프리미엄 유저만 볼 수 있는 강의 동영상 등

![Untitled 15](https://user-images.githubusercontent.com/84123877/183291441-68f8b9d2-f5b2-4f2b-ad18-413db354e0cc.png)

### Origin Access Identity (OAI)

**S3의 컨텐츠를 CloudFront를 사용해서만 볼 수 있도록 제한**하는 방법이다.

즉, S3의 정적인 컨텐츠 URL로 바로 접근하는게 아니라 CDN을 통해서 접근하는 것이다.
왜냐하면 S3로 직접 접속을 하면 캐싱을 못해 속도 측면에서 마이너스가 될 수 있고,
국가별 라우팅, 인증 등이 CloudFront에 구현되어있다면 유저가 직접 S3로 접속하면 안되기 때문임.

- S3는 CloudFront와 잘 맞는다 → 정적인 컨텐츠를 호스팅하기 때문에 CDN과 찰떡궁합
- CloudFront만 권한을 가지고 S3에 접근하고 나머지 접근권한은 없음 → CloudFront는 유저와
S3사이에서 중개하는 역할
- **S3 Bucket Policy로 CloudFront의 접근을 허용**해야 사용 가능

![Untitled 16](https://user-images.githubusercontent.com/84123877/183291443-948aab4d-b980-4122-9308-624711242bed.png)

### Field Level Encryption

- CloudFront로부터 Origin 사이의 통신을 **암호화**
- 최대 10개의 필드까지 가능
- 공개키 방식으로 암호화 → CloudFront에 공개키를 제공 후 Origin에서 
Private Key 로 해독

![Untitled 17](https://user-images.githubusercontent.com/84123877/183291444-11f7975f-f458-4d60-8fc0-4f4a05930573.png)

---
