# AWS S3 객체 메타데이터 설정

---

S3 객체에 다양한 기능을 하는 메타데이터(데이터의 데이터)를 설정할 수 있다.

메타데이터는 HTTP 1.1 표준에 정의된 메타데이터와 S3 전용 메타데이터로 나뉜다.

S3 객체 목록에서 파일을 선택한 뒤 속성 → 메타데이터 편집으로 이동한다.

![Untitled](https://user-images.githubusercontent.com/84123877/180602751-6570cb0d-0a16-4f0e-bc35-c3c223edab80.png)

> S3 객체 메타데이터 설정
> 

### S3 객체에 HTTP 헤더를 설정할 수 있다. 이 헤더는 HTTP 1.1에 정의된 것이다.

- **Cache-Control**
: 브라우저 캐시 정책을 설정한다. 
Value에 max-age-3600과 같은 식으로 데이터 만기 시간을 초 단위로 지정할 수 있다.
만기 시간이 지나기 전에 다시 요청을 할 경우 브라우저에서는 서버에서 데이터를 가져오지 
않고 로컬에 있는 데이터를 사용한다.

(이 설정은 AWS CDN 서비스인 CloudFront와도 연계된다.)
(Cache-Control에 대한 내용은 상당히 방대하므로 전용 페이지에서 상세히 다뤄보도록 하겠다.)

- **Content-Disposition** 
: Value에 attachment로 설정하면 JPG 파일 같은 경우 웹 브라우저에서 그림 파일을 
보여주지 않고 바로 다운로드한다. 
그림 파일 뿐만 아니라 다른 형식의 파일에도 지정할 수 있다.

- **Content-Type** 
: 웹 브라우저에서 파일을 어떻게 처리해야 하는지 알려주는 메타데이터이다.
보통 확장자에 따라 자동으로 설정되고, 확장자가 없는 파일은 이 메타데이터를 통해 강제로
형식을 지정해 줄 수 있다.

(예를 들어 HTML를 담고 있는 hello라는 파일을 올리고, 웹 브라우저에서 열면 내용이 보이지
않고 바로 다운로드가 되어버린다. 그래서 hello라는 파일의 Content-Type을 text/html로 
설정해주게 되면 웹 브라우저에서 HTML파일로 인식하여 바로 내용을 보여주게 된다.)

- **Content-Laguage**
: 텍스트로 된 파일(HTML 등)의 언어를 지정한다.
en, ko, JP 혹은 en-us, ko-kr, ja-jp 등으로 지정한다.

- **Expires**
: Cache-Control과 같은 동작을 하지만 초 단위가 아닌 특정 날짜와 시간을 지정해서
만기 시간을 설정한다. Tue, 22 Apr 2014 20:00:00 GMT 와 같은 형식으로 지정해야 한다.

- **Content-Encoding**
: 데이터 인코딩 방식이다. Value에 gzip을 지정할 수 있으며
이때에는 데이터를 압축하여 전송한 뒤 웹 브라우저에서는 압축을 해제하여 사용하게 된다.
텍스트 데이터가 많고 용량이 큰 경우 이 메타데이터를 사용하면 데이터 전송 비용을 절감할 수 있다. (JPG나 PNG 등의 그림 파일은 이미 포맷 자체가 압축 데이터이므로, gzip을 사용해도 크게 줄어들지 않는다. 따라서 그림파일에서는 효율 x) gzip을 가장 많이 사용하며 defate도 지정 가능

### 다음은 S3 전용 메타데이터이다.

- Website Redirect Location
: 웹 브라우저로 해당 파일에 접속했을 때 다른 파일이나 URL로 리다이렉션하는 기능이다.
/hello.html 처럼 S3에 올라가 있는 파일명을 지정할 수 있다. (이 때는 /로 시작해야 함)
또는 [http://chanwoocool.html](http://chanwoocool.html) 처럼 외부 도메인 또는 URL를 지정할 수도 있다.
이 설정은 버킷을 정적 웹사이트 호스팅으로 설정한 뒤 S3 Website Endpoint로 접속해야
동작한다.

- 사용자 정의 메타데이터

- x-amz-meta- 
: 사용자 마음대로 사용할 수 있는 메타데이터이다.
객체에 추가적인 설명을 넣고 싶을 때 사용한다.
x-amz-meta- 뒤에 임의로 이름을 붙여 x-amz-meta-user-id 등과 같이 사용하면 된다.

- S3 시스템 정의 메타데이터
: 일부 S3 세부 설정들을 이 메타데이터로 대신할 수 있다.
그리고 S3 콘솔에서 설정한 세부 설정들은 HTTP Response Header에 아래 키로 표현된다.

- x-amz-server-side-encryption
: 데이터 암호화 옵션이다. Properties의 Details에서 Server Side Encryption 설정을
대신할 수 있으며 AES256을 값으로 가진다.

- x-amz-version-id
: S3 버킷에 버저닝 기능을 켯을 때, 파일의 버전을 표시한다.
(사용자가 편집할 수 없는 메타데이터)
- x-amz-delete-marker 
: S3 버킷에 버저닝 기능을 켜고, 파일을 삭제하면 파일이 완전히 삭제되지 않고
삭제 표시(delete-marker) 설정이 된다. true 값으로 가진다.
(사용자가 편집할 수 없는 메타데이터)

- x-amz-storage-class
: 스토리지 클래스 옵션이다. Properties의 Details에서 스토리지 클래스(Storage Class)
설정을 대신할 수 있으며 STANDARD와 REDUCED_REDUNDANCY를 값으로 가진다.

---
