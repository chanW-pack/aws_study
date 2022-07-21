# AWS S3 심화 (정적 웹사이트 호스팅)

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

![Untitled](https://user-images.githubusercontent.com/84123877/179191107-766dd800-8f55-4251-bfde-09283a29da43.png)

> S3 버킷 목록에서 위쪽의 Create Bucket 버튼을 선택한다.
> 

![Untitled 1](https://user-images.githubusercontent.com/84123877/179191070-37cb65cc-f687-49cd-a335-ce76eb8e1f94.png)

> 시드니 리전으로 버킷을 생성하였다.
> 

![Untitled 2](https://user-images.githubusercontent.com/84123877/179191072-9963458f-4c0c-4d58-9c22-dfe4f80b5bc7.png)

> 이후 웹사이트이므로 버킷에 올라가는 모든 파일을 인터넷에 공개하도록 권한 설정을
해주어야 한다.
> 

![Untitled 3](https://user-images.githubusercontent.com/84123877/179191075-9e2c00c1-5761-4a93-a2f5-9074f76737ad.png)

> 메모장으로 간단한 웹 페이지를 작성하였다. (index.html)
> 

![Untitled 4](https://user-images.githubusercontent.com/84123877/179191078-c2767a29-4398-4f43-9427-e598ada670f7.png)

> 정적 웹사이트 호스팅 탭을 선택한다. 활성화로 변경하고
인덱스 페이지의 파일명을 설정하였다. index.html로 지정했다.
> 

![Untitled 5](https://user-images.githubusercontent.com/84123877/179191080-634857c0-e3c0-4da3-9fdc-913caf648636.png)

> index.html을 정적 웹사이트 호스팅을 설정한 버킷에 올린다.
> 

![Untitled 6](https://user-images.githubusercontent.com/84123877/179191083-cc003c97-4de5-47d0-bf85-0bc8238322fc.png)

> 이후 S3 웹사이트 엔드포인트의 링크를 확인할 수 있다.
> 

![Untitled 7](https://user-images.githubusercontent.com/84123877/179191085-2289ddd8-7c86-44aa-acca-45b82af42057.png)

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


---
