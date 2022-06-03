# 가상 스토리지를 제공하는 EBS

**EBS(Elastic Block Store)**는 EC2 인스턴스에 장착하여 사용할 수 있는 가상 저장 장치이다.

EBS는 EC2 인스턴스에서 제공하는 기본 용량보다 더 사용해야 할 때, 

운영체제를 중단시키지 않고 용량을 자유롭게 늘리고 싶을 때,

영구적인 데이터 보관이 필요할 때,

RAID 등의 고급 기능이 필요할 때 사용합니다.

<aside>
💡 **프리 티어에서 사용 가능**
EBS는 프리 티어에서 무료로 사용가능 합니다. 30GB 스토리지, 2백만 I/O 및 1GB 스냅샷 스토리지를 무료로 사용할 수 있다.

</aside>

EBS는 EC2에 설치된 OS에서 그냥 일반적인 하드디스크 또는 SSD 처럼 인식된다.

원하는 크기로 만들 수 있고 성능(IOPS)또한, 원하는 수치로 설정할 수 있다.

또한, 사용자가 삭제하기 전까지는 데이터가 안전하게 유지된다.

### !! TIP

여기서  Elastic Block Store의 Block은 블록 장치(Block Device)라고 하여 Unix/Linux 계열 OS에서 일정한 크기(Block) 단위로 읽고 쓰는 저장 장치를 부르는 말이다.

자기테이프, 플로피디스크, 하드디스크, 광학디스크, SSD 등의 플래시 메모리가 대표적이다.

## EBS 기본 개념

---

- **볼륨(Volume**) : EBS의 가장 기본적인 형태로 OS에서 바로 사용 가능한 형태입니다.

- **이미지(Image)** : AMI(Amazon Machine Image)를 줄여 부르는 말입니다. OS가 설치된 형태이며 이 AMI로 EC2 인스턴스를 생성합니다.

- **스냅샷(Snapshot)** : EBS 볼륨의 특정 시점을 그대로 복사하여 저장한 파일을 뜻합니다. 이 스냅샷을 이용하여 EBS 볼륨과 AMI를 생성할 수 있습니다.

- **IOPS(Input/Output Operation Per Second)** : 저장 장치의 성능 측정 단위입니다. AWS에서는 추가 비용을 지불하고 높은 성능(IOPS)의 EBS를 생성할 수 있습니다. 설정할 수 있는 값은 최소 100IOPS에서 4000 IOPS까지 입니다.
    - IOPS는 16kb 단위로 처리됩니다. 따라서 크기가 작은 파일이 있다면 16KB 단위로 묶어서 처리하면 높은 성능을 낼 수 있습니다.
    

EC2 인스턴스를 생성할 때 기본적으로 OS가 설치된 EBS 볼륨을 함께 생성하게 됩니다.

이번에는 OS가 설치된 기본 EBS 볼륨 이외에 추가로 EBS 볼륨을 생성하여 사용하겠습니다.

## EBS 볼륨 생성하여 장착하기

---

![Untitled](https://user-images.githubusercontent.com/84123877/171626752-e68d3ef9-79fd-4697-81c1-6ed2dd35f0d5.png)

> AWS 콘솔의 EC2 페이지에서 왼쪽 ELASTIC BLOCK STORE → Volumes 선택
> 

![Untitled 1](https://user-images.githubusercontent.com/84123877/171626723-d8d316d7-3b3a-41e5-a693-ab22f1b98b04.png)

> 현재 생성된 EBS 볼륨의 목록이 표시된다. 
우측 상단 Create Volume 버튼을 클릭힌다.
> 

![Untitled 2](https://user-images.githubusercontent.com/84123877/171626730-8e6359a8-36fc-4aa4-8a13-641ef798a409.png)

> EBS 볼륨을 생성한다.
> 

- Type : EBS 볼륨 형태이다. 기본값 그대로 사용한다.
- Size : EBS 볼륨 크기이다. 10GIB를 생성할 것이므로 10을 입력한다.
- IOPS : TYpe을 General Purpose 로 설정했기 때문에 IOPS를 설정할 수 없다.
Type을 provisionend IOPS로 선택하야 이 값을 설정할 수 있다.
- Availability Zone : 볼륨이 생성될 가용 영역이다. EC2 인스턴스가 생성된 가용 영역과 같은
곳에 위치해야 EC2 인스턴스를 사용할 수 있다.
- Snapshot ID : 생성해놓은 EBS 스냅샷이 있다면 여기서 선택할 수 있다.
- Encryption : 볼륨 암호화 옵션이다.

![Untitled 3](https://user-images.githubusercontent.com/84123877/171626731-d28659ac-8288-4a6c-8689-ef540c3d1857.png)

> EBS 볼륨 목록에 10GIB 용량의 EBS 볼륨이 생성된것을 확인할 수 있다.
> 

![Untitled 4](https://user-images.githubusercontent.com/84123877/171626734-1a5aee61-1250-479b-8764-55e15dd8746a.png)

![Untitled 5](https://user-images.githubusercontent.com/84123877/171626737-89ffa93c-6085-4d69-85cc-e493920cdbe5.png)

> 이후 볼륨 상태가 준비완료가 되었다면 우클릭 후 볼륨 연결로 EC2에 연결한다.
> 

![Untitled 6](https://user-images.githubusercontent.com/84123877/171626739-e8045aa0-dd86-4f87-9f44-b29d1a684767.png)

> 하단 /dev/xvdf 10GIB 로 잘 연결되어 있다.
> 

## EBS 볼륨 포맷/마운트/제거

---

![Untitled 7](https://user-images.githubusercontent.com/84123877/171626740-f74620c3-404e-41fa-9ac5-45e21dde95df.png)

> SSH로 EC2 인스턴스에 접속한 뒤 
sudo mkfs -t ext4 /deb/xvdf 를 입력하여 EBS 볼륨을 포맷하였다.
> 

EBS 볼륨이 ext4 파일시스템으로 포맷 완료되었다.

포맷 완료했으므로, 마운트만 하면 바로 사용할 수 있다.

![Untitled 8](https://user-images.githubusercontent.com/84123877/171626744-7ca605f7-5f92-43f2-a671-a3b9159434a9.png)

> 디렉터리(cwgogo)를 생성 후 마운트 진행하였다. 
df -h 로 마운트된 저장 장치의 목록을 확인 가능하다.
> 

![Untitled 9](https://user-images.githubusercontent.com/84123877/171626746-3e10d677-a9eb-44bb-9e67-5fc7e6222734.png)

> 삭제를 위해  장치를 umount 진행하였다.
> 

![Untitled 10](https://user-images.githubusercontent.com/84123877/171626747-73fa027a-171b-4165-9b56-42868f2c4f85.png)

 

> 이후 해당 볼륨의 작업창에서 볼륨을 분리하고 사용 중지,삭제를 진행하면 된다.
> 

![Untitled 11](https://user-images.githubusercontent.com/84123877/171626749-aca084e5-4f36-4015-b996-04c0a6e6fc3e.png)

> EC2 인스턴스에서 EBS 볼륨이 완전히 제거되었다.
> 

<aside>
💡 **EBS 볼륨과 RAID**
EBS 볼륨은 OS에서 봤을 때 하드디스크 또는 SSD와 똑같다. 따라서 EBS도 RAID 구성을 할
수 있다. EC2와 EBS 볼륨이 지원하는 RAID 타입은 RAID 0, RAID 1, RAID 10 이다.
RAID 5와 RAID 6은 충분한 성능이 나오지 않아 AWS에서는 권장하지 않는다..

</aside>

---
