# AWS AMI (생성 및 이용하여 인스턴스 생성, AWS Marketplace, 다른 리전으로 복사)

---

AMI(Amazon Machine Images)는 EC2 인스턴스를 생성하기 위한 기본 파일입니다.

AWS에서는 빈 EC2 인스턴스에 직접 OS를 설치할 수 없습니다.

그렇게 때문에 미리 OS가 설치된 AMI를 이용하여 EC2 인스턴스를 생성하게 됩니다.

이 AMI는 단순히 OS만 설치되는 것이 아닌, 각종 서버 애플리케이션, 데이터베이스,

방화벽 등의 네트워크 솔류션, 각종 비즈니스 솔류션 등도 함께 설치될 수 있습니다.

<aside>
💡 **프리 티어에서 사용 가능**
AMI는 프리 티어에서 무료로 사용가능 합니다.

</aside>

또한, 모든 설치와 설정이 완료된 AMI를 이용하여 EC2 인스턴스를 신속하게 늘려나가는

작동 횡적 확장(Auto Scaling)을 가능하게 합니다.

![Untitled](https://user-images.githubusercontent.com/84123877/175532705-0836943c-dc45-4fc2-8827-2cfc53eb9c88.png)

> AMI와 Auto Scaling
> 

즉, AMI는 설치 및 설정이 완료된 EC2 인스턴스를 신속하게 생성해야 할 때,

Auto Scaling 등으로 자동화 할 때, EC2 인스턴스를 다른 리전으로 이전해야 할 때,

상용 솔류션을 사용하고자 할 때 사용합니다.

<aside>
💡 **VM Import/Export**
빈 EC2 인스턴스에 직접 OS를 설치할 수 없지만 가상화 소프트웨어(Vmware 등)를 이용하여 OS를 설치한 뒤에 VM 이미지를 AMI로 변환할 수 있습니다.
자세한 내용은 링크를 참조하기 바랍니다.
[http://aws.amazon.com/ko/ec2/vm-import/](http://aws.amazon.com/ko/ec2/vm-import/)

</aside>

## AWS MarketPlace

---

애플의 앱스토어나 구글의 구글 플레이처럼 AWS에도 AMI를 사고 팔 수 있는

AWS Marketplace가 있습니다.

AWS 콘솔의 메인 화면 오른쪽 아래에 AWS Marketplace 링크가 있습니다.

![Untitled 1](https://user-images.githubusercontent.com/84123877/175532687-999270c9-a940-4bdc-ad5f-e512d8cfe8ac.png)

> AWS Marketplace 메인 화면
> 

대부분OS만 설치된 AMI는 무료이며, 특별한 애플리케이션이 설치된 AMI를 사용할 경우

시간당 혹은 월 사용 요금이 과금되는 방식입니다.

대표적인 AWS Marketplace AMI

- 워드프레스(WordPress): Ubuntu Linux에 웹 서버 및 WordPress가 미리 설치, 설정된 AMI 입니다.
- 우분투 서버(Ubuntu Server): Ubuntu Linux를 서버용으로 최적화 한 Linux 배포판입니다.
- 데비안 리눅스(Debian GNU/Linux): 역사가 깊고 유명한 Linux 배포판 입니다. Ubuntu Linux가 이 Debian Linux를 기반으로 하고 있습니다.
- CentOS: Red Hat Enterprise Linux를 무료로 사용할 수 있도록 만든 Linux 배포판입니다.
- FreeBSD: BSD 계열의 오픈소스 운영체제 입니다.
- MongoDB: MongoDB는 대표적인 NoSQL 데이터베이스이며 MongoDB 개발사에서 공식적으로 제공하는 AMI 입니다. Provisioned IOPS가 설정된 EBS를 사용하여 고성능을 내도록 설정되어 있습니다.
- OpenVPN Access Server: 가상 사설망(VPN)을 구축할 수 있는 오픈소스 소프트웨어인 OpenVPN이 미리 설치된 AMI 입니다.

이외에도 수많은 AMI가 AWS Marketplace에 올라와 있으니 각자 용도에 맞게 사용하면 됩니다.

## EC2 인스턴스로 AMI 생성하기

---

이전 **EBS 스냅샷 활용 페이지**에서 EBS 스냅샷으로 AMI를 생성해보았습니다.

이번에는 EC2 인스턴스를 바로 AMI로 생성하는 방법을 알아보겠습니다.

인스턴스 목록에서 AMI를 생성할 EC2 인스턴스를 선택하고,

마우스 오른쪽 버튼을 클릭하여 팝업 메뉴를 연 뒤 Create Image를 클릭합니다.

![Untitled 2](https://user-images.githubusercontent.com/84123877/175532697-c6d6cd9b-df74-40ba-961c-796d1552318c.png)

> EC2 인스턴스로 AMI 생성하기
> 

위 사진처럼 EC2 인스턴스로 AMI를 생성할 수 있습니다.

- Image name : AMI 이름입니다.
- Image Description : AMI 설명입니다. 입력하지 않아도 무관합니다.
- No reboot : AMI를 생성할 때 EC2 인스턴스를 재부팅 하지 않고 생성하는 옵션입니다.
EC2 인스턴스를 재부팅 하지 않고 생성하면 파일시스템의 무결성을 보장하지 않습니다.
- Instance Voluume : 기본 장착될 EBS 볼륨 설정입니다. 기본값으로 사용했습니다.

설정이 완료되었다면 Create Image 버튼을 클릭합니다.

![Untitled 3](https://user-images.githubusercontent.com/84123877/175532699-267bbb1f-16fb-4c18-b7cb-7df8fdac401b.png)

> AMI 생성 완료
> 

AMI 목록으로 이동하여 방금 생성된 AMI가 표시되는지 확인합니다.

Launch 버튼을 클릭하면 이 AMI로 EC2 인스턴스를 생성할 수 있습니다.

(생성하는 방법은 EC2 생성과 동일하므로 생략하겠음)

## AMI를 다른 리전으로 복사

---

생성한 EBS 스냅샷을 다른 리전으로 복사할 수 있는 것처럼 

AMI도 다른 리전으로 복사할 수 있습니다.

AMI는 리전별로 관리되기 때문에 AMI를 다른 리전에서 사용하러면 AMI를 해당 리전으로

복사해 주어야 합니다.

![Untitled 4](https://user-images.githubusercontent.com/84123877/175532702-87cab5ee-7af7-41bc-8b59-b9337eaeb60b.png)

![Untitled 5](https://user-images.githubusercontent.com/84123877/175532703-fbcdcaaa-32c3-46c0-96a1-484ad114d6e6.png)

> AMI 복사 메뉴
> 
- Destination Region : 복사할 리전을 설정합니다. 오사카를 선택했습니다.
- Name : 새로 만들어질 AMI의 이름입니다.
- Description : 새로 만들어질 AMI의 설명입니다.

![Untitled 6](https://user-images.githubusercontent.com/84123877/175532704-5f5e28bf-4d9c-45f7-b8ff-0075bade0800.png)

> 시드니 리전에 있던 AMI가 오사카 리전으로 복사된것을 확인 가능합니다.
>
