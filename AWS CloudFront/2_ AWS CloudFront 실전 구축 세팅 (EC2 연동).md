# AWS CloudFront 실전 구축 세팅 (EC2와 연동하기)

---

## CloudFront 배포

---

CloudFront 배포는 CloudFront의 가장 기본적인 단위이다.
이 배포가 독립적인 도메인을 가지게 된다.

이후 DNS 서비스인 Route 53이나 별도의 DNS 서버에서 배포 도메인을 CNAME으로 설정하면
사용자가 구매한 도메인과 연결할 수 있다.

여기서 중요한 개념이 오리진(Origin)이다. CloudFront는 CDN 서비스이기 때문에
항상 원본에서 파일을 복사해서 가져와야 한다.

이 파일을 가져오는 원본 서버가 오리진이다.

CloudFront가 지원하는 오리진은 다음과 같다.

- **S3 버킷** 
: 가장 기본적인 오리진이다. S3에 버킷을 생성하고 파일을 저장하기만 하면 되기 때문에 가장
간편하다.
- **EC2 인스턴스**
: EC2 인스턴스에 웹 서버를 구축하면 오리진으로 사용할 수 있다.
- **ELB(Elastic Load Balancing)**
: EC2 인스턴스 여러 대의 부하를 분산하는 ELB도 오리진으로 사용할 수 있다.
- **AWS 이외의 웹 서버**
: 웹 서버의 형태만 띠고 있다면 운영체제, 서버 애플리케이션의 종류에 상관 없이
오리진으로 사용할 수 있다.

## CloudFront 커스텀 오리진 사용 (EC2와 연동하기)

---

![Untitled](AWS%20CloudFront%20%E1%84%89%E1%85%B5%E1%86%AF%E1%84%8C%E1%85%A5%E1%86%AB%20%E1%84%80%E1%85%AE%E1%84%8E%E1%85%AE%E1%86%A8%20%E1%84%89%E1%85%A6%E1%84%90%E1%85%B5%E1%86%BC%20(EC2%E1%84%8B%E1%85%AA%20%E1%84%8B%E1%85%A7%E1%86%AB%E1%84%83%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%84%80%20cd658b07f7b24fa7a35a4acd9eed5d79/Untitled.png)

> 배포에 앞서 EC2 웹서버를 띄우기 위해 인스턴스를 생성하였다.
> 

```bash
$ sudo -s
$ yum install -s httpd # 웹서버 설치
$ service httpd start # 웹서버 실행
# 본인은 우분투를 사용하여 httpd 대신 apache2 사용
```

![Untitled](AWS%20CloudFront%20%E1%84%89%E1%85%B5%E1%86%AF%E1%84%8C%E1%85%A5%E1%86%AB%20%E1%84%80%E1%85%AE%E1%84%8E%E1%85%AE%E1%86%A8%20%E1%84%89%E1%85%A6%E1%84%90%E1%85%B5%E1%86%BC%20(EC2%E1%84%8B%E1%85%AA%20%E1%84%8B%E1%85%A7%E1%86%AB%E1%84%83%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%84%80%20cd658b07f7b24fa7a35a4acd9eed5d79/Untitled%201.png)

![Untitled](AWS%20CloudFront%20%E1%84%89%E1%85%B5%E1%86%AF%E1%84%8C%E1%85%A5%E1%86%AB%20%E1%84%80%E1%85%AE%E1%84%8E%E1%85%AE%E1%86%A8%20%E1%84%89%E1%85%A6%E1%84%90%E1%85%B5%E1%86%BC%20(EC2%E1%84%8B%E1%85%AA%20%E1%84%8B%E1%85%A7%E1%86%AB%E1%84%83%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%84%80%20cd658b07f7b24fa7a35a4acd9eed5d79/Untitled%202.png)

> 아파치2 웹서버가 정상적으로 실행되었다.
> 

웹서버에 대한 요청 로그들을 보는 방법이다.

```bash
$ cd /var/log/apache2
$ ls
access_log error_log

$ tail -f access_log
```

![Untitled](AWS%20CloudFront%20%E1%84%89%E1%85%B5%E1%86%AF%E1%84%8C%E1%85%A5%E1%86%AB%20%E1%84%80%E1%85%AE%E1%84%8E%E1%85%AE%E1%86%A8%20%E1%84%89%E1%85%A6%E1%84%90%E1%85%B5%E1%86%BC%20(EC2%E1%84%8B%E1%85%AA%20%E1%84%8B%E1%85%A7%E1%86%AB%E1%84%83%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%84%80%20cd658b07f7b24fa7a35a4acd9eed5d79/Untitled%203.png)

> 웹서버를 실행했고, 요청 로그들을 볼 수 있게 세팅했다.
> 

그리고 index.html 웹페이지를 생성했다.

```bash
$ vi /var/www/apache2/index.html
```

```html
<html>
<body>
     <h1> Hello ChanWoo !! </h1>
     <img src = 이미지를 넣었습니당> 
</body>
</html>
```

![Untitled](AWS%20CloudFront%20%E1%84%89%E1%85%B5%E1%86%AF%E1%84%8C%E1%85%A5%E1%86%AB%20%E1%84%80%E1%85%AE%E1%84%8E%E1%85%AE%E1%86%A8%20%E1%84%89%E1%85%A6%E1%84%90%E1%85%B5%E1%86%BC%20(EC2%E1%84%8B%E1%85%AA%20%E1%84%8B%E1%85%A7%E1%86%AB%E1%84%83%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%84%80%20cd658b07f7b24fa7a35a4acd9eed5d79/Untitled%204.png)

> html를 생성하고 EC2 DNS 주소로 접속하였다.
이렇게 EC2 인스턴스로 웹 서버를 생성완료했다.
이제 이 웹 서버를 CloudFront로 배포하겠다.
> 

![Untitled](AWS%20CloudFront%20%E1%84%89%E1%85%B5%E1%86%AF%E1%84%8C%E1%85%A5%E1%86%AB%20%E1%84%80%E1%85%AE%E1%84%8E%E1%85%AE%E1%86%A8%20%E1%84%89%E1%85%A6%E1%84%90%E1%85%B5%E1%86%BC%20(EC2%E1%84%8B%E1%85%AA%20%E1%84%8B%E1%85%A7%E1%86%AB%E1%84%83%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%84%80%20cd658b07f7b24fa7a35a4acd9eed5d79/Untitled%205.png)

> CloudFront 탭에서 배포 → 배포 생성을 선택한다.
> 

![Untitled](AWS%20CloudFront%20%E1%84%89%E1%85%B5%E1%86%AF%E1%84%8C%E1%85%A5%E1%86%AB%20%E1%84%80%E1%85%AE%E1%84%8E%E1%85%AE%E1%86%A8%20%E1%84%89%E1%85%A6%E1%84%90%E1%85%B5%E1%86%BC%20(EC2%E1%84%8B%E1%85%AA%20%E1%84%8B%E1%85%A7%E1%86%AB%E1%84%83%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%84%80%20cd658b07f7b24fa7a35a4acd9eed5d79/Untitled%206.png)

> 위에서 생성한 EC2 DNS 주소를 입력한다.
S3 정적 호스팅 도메인이나 ELB 도메인 역시 사용 가능하다.
> 

![Untitled](AWS%20CloudFront%20%E1%84%89%E1%85%B5%E1%86%AF%E1%84%8C%E1%85%A5%E1%86%AB%20%E1%84%80%E1%85%AE%E1%84%8E%E1%85%AE%E1%86%A8%20%E1%84%89%E1%85%A6%E1%84%90%E1%85%B5%E1%86%BC%20(EC2%E1%84%8B%E1%85%AA%20%E1%84%8B%E1%85%A7%E1%86%AB%E1%84%83%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%84%80%20cd658b07f7b24fa7a35a4acd9eed5d79/Untitled%207.png)

> 원본 경로같은 경우는 다른 HTML이나 라우팅이 필요할 시 기재한다.
> 

![Untitled](AWS%20CloudFront%20%E1%84%89%E1%85%B5%E1%86%AF%E1%84%8C%E1%85%A5%E1%86%AB%20%E1%84%80%E1%85%AE%E1%84%8E%E1%85%AE%E1%86%A8%20%E1%84%89%E1%85%A6%E1%84%90%E1%85%B5%E1%86%BC%20(EC2%E1%84%8B%E1%85%AA%20%E1%84%8B%E1%85%A7%E1%86%AB%E1%84%83%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%84%80%20cd658b07f7b24fa7a35a4acd9eed5d79/Untitled%208.png)

<aside>
💡 **뷰어 프로토콜 정책**
- HTTP and HTTPS : http또는 https 둘다 사용할때
- Redirect HTTP to HTTPS : 만일 http로 접속하면 https로 리다이렉트. 보통 가장 많이 사용
- HTTPS only : https 로만

</aside>

![Untitled](AWS%20CloudFront%20%E1%84%89%E1%85%B5%E1%86%AF%E1%84%8C%E1%85%A5%E1%86%AB%20%E1%84%80%E1%85%AE%E1%84%8E%E1%85%AE%E1%86%A8%20%E1%84%89%E1%85%A6%E1%84%90%E1%85%B5%E1%86%BC%20(EC2%E1%84%8B%E1%85%AA%20%E1%84%8B%E1%85%A7%E1%86%AB%E1%84%83%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%84%80%20cd658b07f7b24fa7a35a4acd9eed5d79/Untitled%209.png)

1. **캐시 정책 (Cache Control)** : 캐싱 방법 및 압축

- TTL 및 Cache Key 정책
- CloudFront가 어떻게 캐싱을 할지를 결정

1. **원본 요청 정책 (Origin Request)** : Origin으로 어떤 내용을 보낼 것인가

- Origin에서 쿠키, 헤더, 쿼리스트링 중 어떤 것을 보낼 것인가

1. **응답 헤더 정책**

- 뷰어에게 보낼 HTTP Header
- CloudFront가 응답과 함께 실어 보낼 HTTP Header

```
**캐시 키 (Cache Key)**
- 어떤 기준으로 컨텐츠를 캐싱할 것인지 결정
- 기본적으로 URL로 캐싱
- 설정에 따라 Header와 Cookies, 쿼리스트링 등을 사용 가능 (같은 URL로 접속했지만 헤더나 쿠키를 이용해 영어로된 컨텐츠를 보여주거나 광고를 보여주거나 등 다르게 세팅 가능)

**TTL (Time to live)**
- 캐싱된 아이템이 살아있는 시간 → TTL초 이후에는 캐싱에서 삭제

**정책**
- CloudFront가 동작하는 방법을 정의한 정책
- 어떻게 캐싱을 할지, 어떤 내용을 Origin에 보낼지, 어떤 헤더를 허용할지 등 결정
```

![Untitled](AWS%20CloudFront%20%E1%84%89%E1%85%B5%E1%86%AF%E1%84%8C%E1%85%A5%E1%86%AB%20%E1%84%80%E1%85%AE%E1%84%8E%E1%85%AE%E1%86%A8%20%E1%84%89%E1%85%A6%E1%84%90%E1%85%B5%E1%86%BC%20(EC2%E1%84%8B%E1%85%AA%20%E1%84%8B%E1%85%A7%E1%86%AB%E1%84%83%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%84%80%20cd658b07f7b24fa7a35a4acd9eed5d79/Untitled%2010.png)

> 이후 설정값은 모두 기본으로 설정하고 배포 생성한다.
> 

```
**가격 분류 (Price Class)**
- 어느 지역 범위까지의 Edge Node를 사용할 건지 선택

**AWS WAF 웹 ACL**
- 원하지 않는 요청에 대한 방화벽을 설정
- 방화벽에 의해 거절된 요청을 HTTP Status 403(Forbidden)을 응답받는다.

**대체 도메인 이름 Alternate Domain Names(CNAMEs)**
- CloudFront 생성 시 할당받는 도메인 이름 대신 객체의 URL에 사용할 하나 이상의 도메인 이름을 지정

**SSL 인증서**
- HTTPS를 사용하여 객체에 접근하는 경우 인증서를 등록해야 한다.
  * Default CloudFront Certificte : 기본적으로 CloudFront에서 제공하는 인증서 사용
  * Custom SSL Certificate : 사용자 지정 SSL 인증서를 사용, 개인 소유의 SSL 인증서나 ACM을 통해 발급받은  SSL 인증서를 사용

**지원되는 HTTP 버젼(Supported HTTP Versions)**
- 사용자와 CloudFront 간 사용할 HTTP version을 선택

**기본값 루트 객체 (Default Root Object)**
- 특정 객체가 아닌 배포 URL을 요청할 때 CloudFront가 Origin에 요청할 객체 정보
```

<aside>
🤩 클라우드 프론트는 만들어지는데 시간이 오래 걸린다는 특성이 있다.
이는 전세계에 있는 모든 엣지 로케이션에 대상으로 배포를 하기 때문이다.

</aside>

![Untitled](AWS%20CloudFront%20%E1%84%89%E1%85%B5%E1%86%AF%E1%84%8C%E1%85%A5%E1%86%AB%20%E1%84%80%E1%85%AE%E1%84%8E%E1%85%AE%E1%86%A8%20%E1%84%89%E1%85%A6%E1%84%90%E1%85%B5%E1%86%BC%20(EC2%E1%84%8B%E1%85%AA%20%E1%84%8B%E1%85%A7%E1%86%AB%E1%84%83%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%84%80%20cd658b07f7b24fa7a35a4acd9eed5d79/Untitled%2011.png)

> CloudFront 도메인으로 브라우저에서 접속하면
EC2 인스턴스 웹서버로 접속하게 된다.
> 

---
