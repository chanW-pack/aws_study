# AWS S3 심화 (정적 웹사이트 호스팅, HTTP Referer로 S3 접근 제한)

---

## S3정적 웹사이트 호스팅 사용

---

S3를 웹 서버 형태로 사용해보겠다.

사실 저번 실습을 진행해보면 알겠지만 

정적 웹사이트 호스팅 설정을 하지 않더라도 S3에 파일을 올리면

웹 브라우저로 파일에 접근할 수 있다.

하지만, 그냥 사용하는 것과

정적 웹사이트 호스팅 설정을 사용하는 것은 큰 차이점이 있다.

다음은 **일반적인 S3의 URL 형태**이다.

- http://s3-ap-northeast-1.amazonaws.com/examplebucket10/Jellyfish.jpg
- s3-<리전 이름>.amazonaws.com/<버킷 이름>/<파일 이름>

다음은 **정적 웹사이트 호스팅을 사용한 S3의 URL 형태이다**.

- http://examplebucket10.s3-website-ap-northeast-1.amazonaws.com/Jellyfish.jpg
- <버킷 이름>.s3-website-<리전 이름>.amazonaws.com/<파일 이름>
- 이것을 S3 Website Endpoint라고 말합니다.

이처럼 URL 형태에 큰 차이점이 있다.

아래 URL 형태처럼 버킷 이름이 서브 도메인 형태로 들어가야

DNS 서버에서 **CNAME** 설정을 해 줄 수 있다.

<aside>
💡 즉, [chanwoo.com](http://chanwoo.com) 도메인의 CNAME을 
chanwoo10.s3-website-ap-northeast-1.amazonaws.com 로 설정하면
chanwoo.com으로 접속했을 때 버킷(chanwoo10)의 내용이 표시된다.

</aside>

* **CNAME** : 대체 도메인 이름

이 CNAME 설정은 AWS의 DNS 서비스인 Route 53에서 설정 가능하다.

Route 53가 아니더라도 BIND와 같은 일반적인 DNS 서버에서도 설정이 가능하다.

정적 웹사이트 호스팅 설정을 이용하면 

인덱스 페이지와 에러 페이지의 파일명을 따로 지정해 줄 수 있고,

다양한 리다이렉션 행동을 할 수 있다.

정적 웹사이트 호스팅용 버킷을 새로 만든 뒤 간단한 웹사이트를 만들어보겠다.

![Untitled](AWS%20S3%20%E1%84%89%E1%85%B5%E1%86%B7%E1%84%92%E1%85%AA%20(%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%A8%20%E1%84%8B%E1%85%B0%E1%86%B8%E1%84%89%E1%85%A1%E1%84%8B%E1%85%B5%E1%84%90%E1%85%B3%20%E1%84%92%E1%85%A9%E1%84%89%E1%85%B3%E1%84%90%E1%85%B5%E1%86%BC,%20HTTP%20Refer%2036c8887008d3443d916d89bfe0d9874b/Untitled.png)

> S3 버킷 목록에서 위쪽의 Create Bucket 버튼을 선택한다.
> 

![Untitled](AWS%20S3%20%E1%84%89%E1%85%B5%E1%86%B7%E1%84%92%E1%85%AA%20(%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%A8%20%E1%84%8B%E1%85%B0%E1%86%B8%E1%84%89%E1%85%A1%E1%84%8B%E1%85%B5%E1%84%90%E1%85%B3%20%E1%84%92%E1%85%A9%E1%84%89%E1%85%B3%E1%84%90%E1%85%B5%E1%86%BC,%20HTTP%20Refer%2036c8887008d3443d916d89bfe0d9874b/Untitled%201.png)

> 시드니 리전으로 버킷을 생성하였다.
> 

![Untitled](AWS%20S3%20%E1%84%89%E1%85%B5%E1%86%B7%E1%84%92%E1%85%AA%20(%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%A8%20%E1%84%8B%E1%85%B0%E1%86%B8%E1%84%89%E1%85%A1%E1%84%8B%E1%85%B5%E1%84%90%E1%85%B3%20%E1%84%92%E1%85%A9%E1%84%89%E1%85%B3%E1%84%90%E1%85%B5%E1%86%BC,%20HTTP%20Refer%2036c8887008d3443d916d89bfe0d9874b/Untitled%202.png)

> 이후 웹사이트이므로 버킷에 올라가는 모든 파일을 인터넷에 공개하도록 권한 설정을
해주어야 한다.
> 

![Untitled](AWS%20S3%20%E1%84%89%E1%85%B5%E1%86%B7%E1%84%92%E1%85%AA%20(%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%A8%20%E1%84%8B%E1%85%B0%E1%86%B8%E1%84%89%E1%85%A1%E1%84%8B%E1%85%B5%E1%84%90%E1%85%B3%20%E1%84%92%E1%85%A9%E1%84%89%E1%85%B3%E1%84%90%E1%85%B5%E1%86%BC,%20HTTP%20Refer%2036c8887008d3443d916d89bfe0d9874b/Untitled%203.png)

> 메모장으로 간단한 웹 페이지를 작성하였다. (index.html)
> 

![Untitled](AWS%20S3%20%E1%84%89%E1%85%B5%E1%86%B7%E1%84%92%E1%85%AA%20(%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%A8%20%E1%84%8B%E1%85%B0%E1%86%B8%E1%84%89%E1%85%A1%E1%84%8B%E1%85%B5%E1%84%90%E1%85%B3%20%E1%84%92%E1%85%A9%E1%84%89%E1%85%B3%E1%84%90%E1%85%B5%E1%86%BC,%20HTTP%20Refer%2036c8887008d3443d916d89bfe0d9874b/Untitled%204.png)

> 정적 웹사이트 호스팅 탭을 선택한다. 활성화로 변경하고
인덱스 페이지의 파일명을 설정하였다. index.html로 지정했다.
> 

![Untitled](AWS%20S3%20%E1%84%89%E1%85%B5%E1%86%B7%E1%84%92%E1%85%AA%20(%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%A8%20%E1%84%8B%E1%85%B0%E1%86%B8%E1%84%89%E1%85%A1%E1%84%8B%E1%85%B5%E1%84%90%E1%85%B3%20%E1%84%92%E1%85%A9%E1%84%89%E1%85%B3%E1%84%90%E1%85%B5%E1%86%BC,%20HTTP%20Refer%2036c8887008d3443d916d89bfe0d9874b/Untitled%205.png)

> index.html을 정적 웹사이트 호스팅을 설정한 버킷에 올린다.
> 

![Untitled](AWS%20S3%20%E1%84%89%E1%85%B5%E1%86%B7%E1%84%92%E1%85%AA%20(%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%A8%20%E1%84%8B%E1%85%B0%E1%86%B8%E1%84%89%E1%85%A1%E1%84%8B%E1%85%B5%E1%84%90%E1%85%B3%20%E1%84%92%E1%85%A9%E1%84%89%E1%85%B3%E1%84%90%E1%85%B5%E1%86%BC,%20HTTP%20Refer%2036c8887008d3443d916d89bfe0d9874b/Untitled%206.png)

> 이후 S3 웹사이트 엔드포인트의 링크를 확인할 수 있다.
> 

![Untitled](AWS%20S3%20%E1%84%89%E1%85%B5%E1%86%B7%E1%84%92%E1%85%AA%20(%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%A8%20%E1%84%8B%E1%85%B0%E1%86%B8%E1%84%89%E1%85%A1%E1%84%8B%E1%85%B5%E1%84%90%E1%85%B3%20%E1%84%92%E1%85%A9%E1%84%89%E1%85%B3%E1%84%90%E1%85%B5%E1%86%BC,%20HTTP%20Refer%2036c8887008d3443d916d89bfe0d9874b/Untitled%207.png)

> 웹 브라우저의 새 창에서 방금 만들어서 올린 index.html 파일의 내용이 표시된다.
> 

<aside>
💡 **S3 정적 웹사이트 호스팅 활용**

요즘 많이 쓰는 jQuery, jQuery UI, Bootstrap 등의 JavaScript 라이브러리 파일은 
EC2에서 웹 서버를 구축하여 서비스하는 것보다, S3 정적 웹사이트 호스팅을 활용하여 
서비스하는 것이 훨씬 유리합니다. 
또한, 이미지 파일이나, CSS 파일 등도 S3 정적 웹사이트 
호스팅을 활용하는 것이  효과적입니다.

</aside>

## HTTP Referer로 S3 접근 제한하기

---

S3에 올려진 그림 파일을 원하는 도메인에서만 보여줄 수 있도록 설정해보겠다.

S3는 데이터 전송량에 따라 요금을 책정하기 때문에

원하지 않는 도메인에서의 링크를 막는다면 그만큼 비용을 절감할 수 있다.

### *HTTP Referer?*

HTTP Referer는 HTTP 헤더값으로서 웹 브라우저에서 생성하는 데이터이다.

링크를 어디서 클릭했느냐, 그림 파일을 어디서 보여줬느냐를 알 수 있다.

S3에서는 이 Referer 값을 판단해서 파일을 보여주지 말지 결정할 수 있다.

이번에는 버킷을 2개 사용하겠다.

버킷 하나에 웹사이트를 구축하고 HTTP Referer를 설정한다면 

정작 웹사이트의 HTML 파일을 볼 때는 HTTP Referer 설정에 막혀버리기 대문에

의미가 없다.

새로 생성한 버킷(cw-origin123) 을 그림 저장 전용 버컷으로 사용하고, HTTP Referer 설정을 할 것이며 정적 웹사이트 호스팅을 설정했던 버킷(cws3test-123)에서 그림 파일을 링크해보겠다.

즉, cw-origin123 버킷을 HTTP Referer 설정하고, cws3test-123 버킷에서 그림 파일을 링크해보겠다. 

![Untitled](AWS%20S3%20%E1%84%89%E1%85%B5%E1%86%B7%E1%84%92%E1%85%AA%20(%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%A8%20%E1%84%8B%E1%85%B0%E1%86%B8%E1%84%89%E1%85%A1%E1%84%8B%E1%85%B5%E1%84%90%E1%85%B3%20%E1%84%92%E1%85%A9%E1%84%89%E1%85%B3%E1%84%90%E1%85%B5%E1%86%BC,%20HTTP%20Refer%2036c8887008d3443d916d89bfe0d9874b/Untitled%208.png)

> S3 버킷 목록에서 이미지파일이 있는 버킷(cw-origin123)을 선택하고 
버킷 정책 편집을 진행한다.
> 
- Effect: 지정한 도메인만 허용할 것이므로 허용(Allow) 입니다.
- Principal: 정책을 적용할 대상입니다. 인터넷 전체에 공개할 것이므로 *입니다..
- Action: 파일을 보여주는(다운로드) 상황이므로 s3:GetObject 입니다.
- Condition: 조건절입니다. 이곳에 설정한 조건에 맞으면 허용(Allow) 또는 거부(Deny) 합니다.
- StringLike: 조건절 안에 사용하는 조건문입니다. 뜻은 문자열을 포함하고 있을 때 입니다.
- aws:Referer: Referer 값을 지정합니다. 보통 도메인을 설정하며 맨 뒤에 **/*를 붙여주어**, 도메인 이하 모든 경로에 대해 허용합니다. /hello.html 처럼 특정 파일의 경로를 지정할 수도 있습니다. 여러 도메인을 지정하려면 ,(콤마)로 구분하면 됩니다. (위 사진에서 /* 추가해야됨)

<aside>
🚫 버킷 정책의 내용 중에서 aws:Referer의 [http://examplewebhosting.s3-website-ap-northeast-1.amazonaws.com/*](http://cws3test-123.s3-website-ap-southeast-2.amazonaws.com)[에서](http://examplewebhosting.s3-website-ap-northeast-1.amazonaws.com/*%EC%97%90%EC%84%9C) examplewebhosting 부분은 여러분이 생성한 정적 웹사이트 호스팅 버킷의 주소를 설정해야 한다. 저와 같이 그대로 입력하면 안된다.

</aside>

![Untitled](AWS%20S3%20%E1%84%89%E1%85%B5%E1%86%B7%E1%84%92%E1%85%AA%20(%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%A8%20%E1%84%8B%E1%85%B0%E1%86%B8%E1%84%89%E1%85%A1%E1%84%8B%E1%85%B5%E1%84%90%E1%85%B3%20%E1%84%92%E1%85%A9%E1%84%89%E1%85%B3%E1%84%90%E1%85%B5%E1%86%BC,%20HTTP%20Refer%2036c8887008d3443d916d89bfe0d9874b/Untitled%209.png)

> 테스트를 진행하기 전 그림 파일의 권한에서  모든 사용자로 설정된 것이 없는지 확인한다.
모든 사용자(Everything)으로 권한이 설정되어 있으면
Referer 설정을 무시하게 되므로 주의해야 한다.
> 

![Untitled](AWS%20S3%20%E1%84%89%E1%85%B5%E1%86%B7%E1%84%92%E1%85%AA%20(%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%A8%20%E1%84%8B%E1%85%B0%E1%86%B8%E1%84%89%E1%85%A1%E1%84%8B%E1%85%B5%E1%84%90%E1%85%B3%20%E1%84%92%E1%85%A9%E1%84%89%E1%85%B3%E1%84%90%E1%85%B5%E1%86%BC,%20HTTP%20Refer%2036c8887008d3443d916d89bfe0d9874b/Untitled%2010.png)

> AccessDenied 에러와 함께 그림 파일이 열리지 않는다.
링크를 클릭했던 도메인과 버킷 정책에 설정한 Referer 도메인이 맞기 않기 때문이다.
> 

![Untitled](AWS%20S3%20%E1%84%89%E1%85%B5%E1%86%B7%E1%84%92%E1%85%AA%20(%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%A8%20%E1%84%8B%E1%85%B0%E1%86%B8%E1%84%89%E1%85%A1%E1%84%8B%E1%85%B5%E1%84%90%E1%85%B3%20%E1%84%92%E1%85%A9%E1%84%89%E1%85%B3%E1%84%90%E1%85%B5%E1%86%BC,%20HTTP%20Refer%2036c8887008d3443d916d89bfe0d9874b/Untitled%2011.png)

```html
<html>
<head>
    <title>S3 Example Website Hosting</title>
</head>
<body>
    <p>Hello ChanWoo S3</p>
 <img src="https://cw-origin123.s3.ap-southeast-2.amazonaws.com/%AD%AC%ECEC%EC%EC66.jpg">
 <img src="https://cw-origin123.s3.ap-southeast-2.amazonaws.com/%EC%82%AC%EC%A7%841.jpg" width="320" height="240">
</body>
</html>
```

> 위와 같이 index123.html 파일을 작성한 뒤 S3 버킷에 올리고, 확인해보겠다.
> 

![Untitled](AWS%20S3%20%E1%84%89%E1%85%B5%E1%86%B7%E1%84%92%E1%85%AA%20(%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%A8%20%E1%84%8B%E1%85%B0%E1%86%B8%E1%84%89%E1%85%A1%E1%84%8B%E1%85%B5%E1%84%90%E1%85%B3%20%E1%84%92%E1%85%A9%E1%84%89%E1%85%B3%E1%84%90%E1%85%B5%E1%86%BC,%20HTTP%20Refer%2036c8887008d3443d916d89bfe0d9874b/Untitled%2012.png)

> cws3test-123 버킷의 정적 웹 사이트로 접속한다.
> 

![Untitled](AWS%20S3%20%E1%84%89%E1%85%B5%E1%86%B7%E1%84%92%E1%85%AA%20(%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%A8%20%E1%84%8B%E1%85%B0%E1%86%B8%E1%84%89%E1%85%A1%E1%84%8B%E1%85%B5%E1%84%90%E1%85%B3%20%E1%84%92%E1%85%A9%E1%84%89%E1%85%B3%E1%84%90%E1%85%B5%E1%86%BC,%20HTTP%20Refer%2036c8887008d3443d916d89bfe0d9874b/Untitled%2013.png)

> S3 객체의 URL로 접속했을 때 열리지 않던 이미지 파일이 index.html에서는 잘 열린다.
웹 브라우저로 접속한 도메인
([http://cws3test-123.s3-website-ap-southeast-2.amazonaws.com/](http://cws3test-123.s3-website-ap-southeast-2.amazonaws.com/))
과 버킷 정책에 설정한 Referer 도메인이 일치하기 때문이다.
> 

---