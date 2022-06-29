# EBS 스냅샷 활용하기

---

EBS 볼륨의 전체 내용 중 특정 시점을 파일로 저장한 형태가 스냅샷입니다.

따라서 EBS 볼륨의 백업 파일 성격을 가지고 있습니다.

<aside>
💡 **프리 티어에서 사용 가능**
EBS 스냅샷은 프리 티어에서 무료로 사용할 수 있습니다.
2022년 5월 기준으로 EBS 스냅샷 스토리지 1GB를 무료로 사용할 수 있습니다.

</aside>

본인은 Ubuntu Linux가 설치된 8GB EBS 볼륨의 스냅샷을 생성하지만,

EBS 볼륨의 전체 크기가 8GB일 뿐 실제 용량은 1GB가 되지 않으므로 

무료 용량 내에서 실습이 가능하다.

EBS스냅샷은 다양한 용도로 활용이 가능합니다.

- 스냅샷으로 EBS 볼륨 생성(다른 가용 영역에 생성 가능)
- 스냅샷으로 AMI 생성
- 스냡샛을 다른 리전으로 복사

EBS 스냅샷은 EBS 볼륨을 백업하고 이전 내용으로 복원하고 싶을 때, 나만의 AMI를 생성하고 싶을 때, EBS 볼륨을 다른 리전으로 이전하고 싶을 때 사용합니다.

EBS 스냡샷 저장 요금은 S3 데이터 저장 요금에 합산됩니다.

## EBS 스냅샷 생성하기

---

EBS 스냅샷을 생성하는 방법은 2가지가 있습니다.

EBS 볼륨 목록에서 생성하는 방법과 EBS 스냅샷 목록에서 생성하는 방법이 있습니다.

두 방식 모두 위치만 다를 뿐 과정은 완전히 같으므로 EBS 볼륨 목록에서 생성하는 방법으로 

진행하겠습니다.

![Untitled](https://user-images.githubusercontent.com/84123877/171833977-c6b39c3a-655f-4168-a216-f14437bc071f.png)

> EBS 볼륨 목록에서 8GIB EBS 볼륨을 선택하고 메뉴에 들어간다.
Create Snashot을 선택한다.
> 

![Untitled 1](https://user-images.githubusercontent.com/84123877/171833960-29a4fc8b-418a-4e3e-95a1-f2b1de1119b0.png)

> EBS 스냅샷 생성 세부 설정이다.
> 
- Name : EBS 스냅샷 이름입니다. 현 버전의 생성창에서는 보이지 않으므로, 생성 후 네임을 지정하겠습니다.
- Description : EBS 스냅샷의 설명입니다.
- Encrypted : 볼륨을 암호화했다면 YES 로 표시됩니다.

설정이 완료되었으면 Create 버튼을 클릭합니다.

![Untitled 2](https://user-images.githubusercontent.com/84123877/171833962-3a30a3f6-a455-4613-8c1e-67b867d915a3.png)

> EBS 스냅샷 목록으로 이동하여 생성된 스냅샷을 확인했습니다.
완전히 생성되는데까지 약간 시간이 걸립니다.
> 

## EBS 스냅샷으로 EBS 볼륨/AMI 생성하기

---

EBS 스냅샷으로 EBS 볼륨을 생성할 수 있습니다.

EBS 볼륨 자체로는 다른 가용 영역(Avilability Zone)으로 이전할 수 없으므로

꼭 EBS 스냅샷을 활용해야 합니다. 백업 기능도 있으므로 EBS 볼륨의 내용을 되돌리고 싶을 때

EBS 스냅샷을 활용할 수 있습니다.

![Untitled 3](https://user-images.githubusercontent.com/84123877/171833963-821e9b5c-ca00-4b65-8133-4d3ea538a1cb.png)

![Untitled 4](https://user-images.githubusercontent.com/84123877/171833965-1063e818-2054-4c6e-bbd9-741ab0dcfc99.png)

> EBS 스냅샷을 선택한 뒤 Create Volume 기능을 선택하여 진행이 가능하다.
> 

EBS 스냅샷으로 EBS 볼륨 뿐만 아니라 EC2 인스턴스를 생성할 수 있는 

AMI(Amazon Machine Image) 도 생성할 수 있습니다.

EBS 스냅샷으로 AMI를 생성할 때 주의해야 할 점으로 Linux의 경우 Kernel ID를 알아야 합니다.

AMI를 만들 때 이 Kernel ID를 설정하게 되고,

나중에 AMI로 EC2 인스턴스를 생성했을 때 Kernel ID가 맞지 않으면 부팅이 되지 않습니다.

![Untitled 5](https://user-images.githubusercontent.com/84123877/171833968-857ac5ff-477d-4c21-bcb2-9a99a3a579d6.png)

> EC2 인스턴스의 세부 내용에서 Kernel ID를 확인할 수 있다.
사진에 따르면 커널 ID가 없다.
> 

![Untitled 6](https://user-images.githubusercontent.com/84123877/171833971-c49ca31a-427f-4fe9-91af-3bb24d06fd67.png)

> EBS 스냅샷 목록에서 Create Image 기능을 클릭하면
EBS 스냅샷으로 AMI 이미지가 생성이 가능하다.
> 

## EBS 스냅샷을 다른 리전으로 복사하기

---

EBS 볼륨 자체로는 다른 가용 영역으로 이전이 불가능할 뿐만 아니라

다른 리전으로도 이전이 불가능합니다.

따라서 EBS 볼륨을 EBS 스냅샷으로 생성한 뒤 다른 리전으로 복사해야 합니다.

![Untitled 7](https://user-images.githubusercontent.com/84123877/171833972-6467fa65-cfa4-4694-85af-91a62caec42b.png)

> EBS 스냅샷 목록에서 스냅샷을 선택한 뒤 복사를 클릭합니다.
> 
- SnapShot : 선택한 EBS 스냅샷의 ID가 표시됩니다.
- Destination Region : 복사할 리전입니다.
- Description : 새로 만들어질 EBS 스냅샷의 설명입니다.

설정이 완료되었다면 Copy 버튼을 클릭합니다.

![Untitled 8](https://user-images.githubusercontent.com/84123877/171833974-4cfd111e-adce-4f74-97da-3a56690a7025.png)

> 원본 리전(시드니) 에서 복사할 리전(싱가포르)로 이동한 뒤 
EBS 스냅샷 목록으로 이동하여 복사가 진행중인것을 확인했다.
> 

이제 이 스냅샷으로 싱가포르 리전에서 EBS 볼륨이나 AMI를 생성하여 사용하면 된다.

---
