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

![Untitled 8](https://user-images.githubusercontent.com/84123877/179191087-fdf11bef-4907-440d-ac21-afec23be26b4.png)

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

![Untitled 9](https://user-images.githubusercontent.com/84123877/179191090-5e2fb25e-2282-4e34-8a86-d3e6e541adc6.png)

> 테스트를 진행하기 전 그림 파일의 권한에서  모든 사용자로 설정된 것이 없는지 확인한다.
모든 사용자(Everything)으로 권한이 설정되어 있으면
Referer 설정을 무시하게 되므로 주의해야 한다.
> 

![Untitled 10](https://user-images.githubusercontent.com/84123877/179191092-9262e8d6-e28d-4641-81c9-3bcd189d1ad8.png)

> AccessDenied 에러와 함께 그림 파일이 열리지 않는다.
링크를 클릭했던 도메인과 버킷 정책에 설정한 Referer 도메인이 맞기 않기 때문이다.
> 

![Untitled 11](https://user-images.githubusercontent.com/84123877/179191098-a461eb42-6e10-46e5-bd74-7f42dfeb7e4c.png)

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

![Untitled 12](https://user-images.githubusercontent.com/84123877/179191102-73de1548-d3d7-4ba2-a435-1091cc285ac5.png)

> cws3test-123 버킷의 정적 웹 사이트로 접속한다.
> 

![Untitled 13](https://user-images.githubusercontent.com/84123877/179191104-ed09d787-7100-40fb-8639-7d76443a1dd7.png)

> S3 객체의 URL로 접속했을 때 열리지 않던 이미지 파일이 index.html에서는 잘 열린다.
웹 브라우저로 접속한 도메인
([http://cws3test-123.s3-website-ap-southeast-2.amazonaws.com/](http://cws3test-123.s3-website-ap-southeast-2.amazonaws.com/))
과 버킷 정책에 설정한 Referer 도메인이 일치하기 때문이다.
> 

---
