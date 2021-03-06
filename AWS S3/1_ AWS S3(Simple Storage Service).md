# AWS S3(Simple Storage Service)

### AWS S3 란?

Simple Storeage Service의 약자로 업계 최고의 확장성과 데이터 가용성 및
보안과 성능을 제공하는 **온라인 오브젝트(객체) 스토리지 서비스**이다.

(파일 서버의 역할을 하는 서비스라고 보면 쉽다.)
(앞에 온라인이 붙는 이유는 데이터조작에 HTTP/HTTPS를 통한 API가 사용되기 때문이다.)

일반적인 파일서버는 트래픽이 증가함에 따라서 장비를 증설하는 작업을 해야 하는데

S3는 이와 같은 것을 대행한다.

트래픽에 따른 시스템적인 문제는 걱정할 필요가 없어진다.

또 파일에 대한 접근 권한을 지정할 수 있어서 서비스를 호스팅 용도로

사용하는 것을 방지할 수 있다.

## AWS S3 객체 스토리지의 특징

---

객체 스토리지란 객체로 된 파일을 다루는 저장소 라는 말이다.
(반대되는 말로 EBS(Block Sotrage Service)가 있다.)

보통 우리는 OS나 프로그램들을 본체에 하드라는 스토리지에 저장하고 구동시킨다.

하지만 S3 객체 스토리지에서는 **불가능**하다.
S3에 파일을 설치하는 행위는 할 수 없고, 그냥 이미지, 동영상 파일 등만을 저장할 수 있다.

다음 S3의 특징으로는

- **많은 사용자가 접속**을 해도 이를 감당하기 위해서 **시스템적인 작업을 하지 않아도 된다**.
- 저장할 수 있는 **파일의 수가 제한이 없다**.
- 최소 1바이트에서 최대 5TB의 데이터를 저장할 수 있고 서비스 할 수 있다.
- 파일에 인증을 붙여서 무단으로 액세스 하지 못하도록 할 수 있다.
- HTTP와 BitTorrent 프로토콜을 지원한다.
- **REST, SOAP** 인터페이스를 제공한다.
- 데이터를 여러 시설에서 중복으로 저장해 **데이터의 손실**이 발생할 경우 **자동으로 복원**한다.
- 버전관리 기능을 통해서 사용자에 의한 실수도 복원이 가능하다.
- 정보의 중요도에 따라서 **보호 수준을 차등** 할 수 있고, 이에 따라서 **비용을 절감** 할 수 있다. (RSS)

![Untitled](https://user-images.githubusercontent.com/84123877/178140821-27d2a47c-ab58-4cc9-8bf7-3d93c3c0d112.png)

## AWS S3에서 사용되는 용어

---

- **객체** : **object**, AWS는 S3에 저장된 데이터 하나 하나를 객체라고 명명하는데, 
하나 하나의 파일이라고 생각 하면 된다.
- **버킷** : **bucket**, 객체가 파일이라면 버킷은 연관된 객체들을 그룹핑한 최상위 디렉토리라고 할 수 있다. 버킷 단위로 지역(region)을 지정 할 수 있고, 또 버킷에 포함된 모든 객체에 대해서
일괄적으로 인증과 접속 제한을 걸 수 있다.
- **버전관리** : S3에 저장된 객체들의 변화를 저장. 예를 들어 A라는 객체를 사용자가 삭제하거나 
변경해도 각각의 변화를 모두 기록하기 때문에 실수를 만회할 수 있다.
- **RSS** : **Reduced Redundancy Storage**의 약자로 일반 S3 객체에 비해서 데이터가 손실될 확률이
높은 형태의 저장 방식. 대신에 가격이 저렴하기 때문에 복원이 가능한 데이터, 이를테면
섬네일 이미지와 같은 것을 저장하는데 적합하다.
그럼에도 물리적인 하드디스크 대비 400배 가량 안전하다는것이 아마존의 주장
- **Glacier** : 영어로는 빙하라는 뜻으로 매우 저렴한 가격으로 데이터를 저장할 수 있는 아마존의
스토리지 서비스

## S3 사용 예

---

- 클라우드 저장소 (개인 파일 보관, 구글 드라이브처럼 사용 가능)
- 서비스의 대용량 파일 저장소 - 이미지, 동영상, 빅데이터 (ex:넷플릭스)
- 서비스 로그 저장 및 분석
- AWS 아데나를 이용한 빅데이터 업로드 및 분석
- 서비스 사용자의 데이터 업로드 서버 (이미지 서버, 동영상 서버)
- 중요한 파일은 EC2의 SSD (EBS)에 저장하지 말고 S3에 저장
- Glacier와 연동으로 비용 절감 및 규정 준수 가능 (자주 쓰지 않는 데이터를 S3에서 자동 변환)

## AWS S3 버킷 생성

---

![Untitled 1](https://user-images.githubusercontent.com/84123877/178140788-0116d444-3cf1-4b6b-bbb3-d0588555621b.png)

> 콘솔에서 S3에 접속 후 버킷 만들기를 선택한다.
> 

![Untitled 2](https://user-images.githubusercontent.com/84123877/178140789-4d481300-0fc3-4be8-bbad-dc519413f0b0.png)

> 일반 구성은 다음과 같이 설정하였다.
bucket 이름은 cw-s3 test로 설정하였고, 지역은 시드니 리전으로 선택하였다.
> 

버킷 이름은 고유값이다.

![Untitled 3](https://user-images.githubusercontent.com/84123877/178140790-9e57f361-0b63-4180-8cce-628d49a541af.png)

> 객체 소유권은 다른 AWS 계정에서도 소유권을 갖거나 접속제어 가능유무에 대한
여부이다.
> 

ACL 비활성화를 선택하여 내가 접속한 계정에서만 소유하도록 한다.

![Untitled 4](https://user-images.githubusercontent.com/84123877/178140791-7ecabd58-06e4-4f60-909d-701c6aafe1ba.png)

> 퍼블릭 액세스를 차단한다는 것은 외부에서도 파일을 읽게 하지 못한다는 의미다.
퍼블릭 액세스를 경우에 따라 차단하고 싶다면 
”모든 퍼블릭 액세스 차단”은 비활성화하고 세부적인 옵션을 선택하면 된다.
> 

1. **새 ACL(액세스 제어 목록)을 통해 부여된 버킷 및 객체에 대한 퍼블릭 액세스 차단**
: 지정된 ACL이 퍼블릭이거나, 요청에 따라 퍼블릭 ACL이 포함되어 있으면
PUT 요청을 거절한다.
2. **임의의 ACL을 통해 부여된 버킷 및 객체에 대한 퍼블릭 액세스 차단**
: 버킷의 모든 퍼블릭 ACL과 그 안에 포함된 Object를 무시하고, 
퍼블릭 ACL를 포함하는 PUT 요청은 허용한다.
3. **새 퍼블릭 버킷 또는 액세스 지점 정책을 통해 부여된 버킷 및 객체에 대한 퍼블릭 
액세스 차단**
: 지정된 버킷 정책이 퍼블릭이면 PUT 요청을 거절한다. 이 설정을 체크하면 버킷 및
객체에 대한 퍼블릭 액세스를 차단하고 사용자가 버킷 정책을 관리할 수 있으며,
이 설정 활성화는 기존 버킷 정책에 영향을 주지 않는다.
4. **임의의 퍼블릭 버킷 또는 액세스 지점 정책을 통해 부여된 버킷 및 객체에 대한
퍼블릭 액세스 차단**
: 퍼블릭 정책이 있는 버킷에 대한 액세스가 권한이 있는 사용자와 AWS 서비스로만
제한되며, 이 설정 활성화는 기존 버킷 정책에 영향을 주지 않는다.

<aside>
💡 만일 버킷을 비공개로, 나만 보고 싶다면 모든 옵션에 체크한다.
퍼블릭 액세스가 차단되었을 경우 IAM에서 AWSAccessKeyId와 AWSSecretKey를 발급
받고 이를 이용해서 S3객체에 접근 할 수 있다.

</aside>

<aside>
💡 실무에서 사용할 경우에는 ‘**모든 엑세스 차단**’ 혹은 **ACL**을 이용하여 액세스를
차단해주는 것이 보안에 좋다.

</aside>

![Untitled 5](https://user-images.githubusercontent.com/84123877/178140792-64565115-7a04-4fd4-9995-3afc7848b4bf.png)

> 보안상 **기본 암호화** 도 활성화를 하는게 좋지만 기본 생성 후 테스트를 위해 
비활성화 하였다.
> 

![Untitled 6](https://user-images.githubusercontent.com/84123877/178140794-47680ebe-e542-4120-8aac-1e7c90634bd0.png)

> 시드니 리전으로 버킷이 생성된것을 확인 가능하다.
(버킷 네임으로 대문자,공백 설정이 불가했다…. 다시만듦…)
> 

![Untitled 7](https://user-images.githubusercontent.com/84123877/178140795-df9f09f1-5b70-41cc-bf08-8064ad1fd268.png)

> 생성한 버킷을 선택하면 여러 태그들이 나타난다.
객체의 경우 업로드한 파일 리스트를 나타낸다.
> 

권한을 바꾸고 싶을 때는 권한 탭을 선택하면 되고,

속성을 바꾸고 싶을 때는 속성 탭을 선택하면 된다.

이어서, 폴더를 생성하도록 하겠다.

bucket을 선택하고 객체 탭에서 폴더 만들기를 선택한다.

![Untitled 8](https://user-images.githubusercontent.com/84123877/178140796-a1bc1003-79d3-4139-8994-28cdee70c3cc.png)

> 폴더 이름을 적어주고 폴더 만들기를 진행한다.
> 

![Untitled 9](https://user-images.githubusercontent.com/84123877/178140797-538c27f0-6feb-4cbd-83ce-336d45973985.png)

> 폴더가 생성되었다. 이번에는 사진 업로드를 진행해보겠다.
> 

![Untitled 10](https://user-images.githubusercontent.com/84123877/178140798-3f5a3926-c4ce-454a-9780-81c04b01d8bb.png)

> 아무 사진을 드래그로 올려주었다.
> 

![Untitled 11](https://user-images.githubusercontent.com/84123877/178140799-181a3c54-b150-45f8-b57a-848a959592da.png)

> 그러면 여러 옵션 사항을 줄 수 있는데, 먼저 ACL 액세스 제어 목록에서는
현재 객체을 읽고 쓰는 권한을 누구에게 줄 것인지 설정할 수 있다.
개별 ACL 권한 지정으로 AWS 계정 소유자만 읽고 쓰기가 가능하도록 선택했다.
> 

![Untitled 12](https://user-images.githubusercontent.com/84123877/178140801-f250a0be-bc7d-479c-806e-6264557fa740.png)

> 스토리지 클래스다. 스토리지 클래스는 가격이나 속도, 안정성에 따라
비용이 달라지게 된다. 현재는 기본값으로 진행하였다.
> 

![Untitled 13](https://user-images.githubusercontent.com/84123877/178140803-07f777b0-e12c-4554-97a0-7102c34b69c2.png)

> 중요한 폴더 안에 사진이 업로드 된 것을 확인할 수 있다.

---
