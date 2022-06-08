# 고정 IP를 제공하는 Elastic IP

---

Elastic IP는 고정된 공인 IP를 제공합니다. 

EC2 인스턴스를 생성하면 기본적으로 공인 IP가 부여됩니다.

하지만 이 IP 주소는 EC2 인스턴스가 실행되고 있는 동안에만 유효하며

EC2 인스턴스가 중단, 종료되면 IP 주소는 반납됩니다.

따라서 EC2 인스턴스를 다시 실행하면 바뀔 수 있는, 말하자면 유동 IP입니다.

<aside>
💡 **프리 티어에서 사용 가능**
Elatsic IP는 프리 티어에서 사용가능 합니다.

</aside>

![Untitled](https://user-images.githubusercontent.com/84123877/172585850-0a3c5bac-c4e2-49b6-9711-1813b089870b.png)

> EC2 인스턴스 재부팅 후 공인 IP 변경
> 

IPv4 체계에서는 IP 주소가 매우 부족하기 때문에 사용하지 않는 EC2 인스턴스에는 IP 주소를 할당하지 않는 것입니다.

DNS 서버를 통해 도메인에 IP 주소를 연결해놓았는데 IP 주소가 바뀌면 매우 곤란해집니다.

그래서 AWS에서는 Elastic IP라는 것을 제공해주고 있습니다.

특별한 기능이 있는 IP 주소는 아니며, 한번 할당 받으면 절대 바뀌지 않는 IP 주소 입니다.

사용하지 않을 경우 반납 할 수 있습니다.

![Untitled 1](https://user-images.githubusercontent.com/84123877/172585858-63bc5842-04c8-4dd7-85bb-17fe36d1afd8.png)

> Elastic IP 사용
> 

주의할 점은 Elastic IP를 할당 받고, EC2 인스턴스에 연결하여 사용해야지만
요금이 부과되지 않습니다.

아무 EC2 인스턴스에도 사용하지 않고, Elastic IP를 할당만 받아서 가지고 있을 경우

매달 요금이 부과됩니다.

(이것은 앞서 설명한 것처럼 IPv4 주소가 매우 부족하기 때문에 낭비를 방지하기 위해서임)

Elastic IP는 DNS 서버에서 도메인과 IP 주소를 연결해야 할 때, 

또는 IP 주소가 바뀌지 않고 계속 유지해야 할 때 사용합니다.

<aside>
💡 **EC2 인스턴스 재부팅 후 SSH 접속**
EC2 인스턴스를 생성한 뒤 SSH로 접속하여 한참 작업을 한 뒤, EC2 인스턴스를 재부팅
했는데 SSH로 접속이 안될 때가 있다. 이런 경우에는 IP 주소가 바뀌지 않았는지 확인한다.

</aside>

## Elastic IP 할당 및 연결

---

![Untitled 2](https://user-images.githubusercontent.com/84123877/172585860-0bcdd900-db2f-4558-b122-b557f6e34759.png)

> AWS EC2 → 네트워크 및 보안 → 탄력적 IP 할당
> 

할당 버튼을 클릭한다.

![Untitled 3](https://user-images.githubusercontent.com/84123877/172585862-7523c7b4-9399-4604-8fef-261410ab393e.png)

> 새 IP 주소 할당 요청을 진행하였다.
> 

![Untitled 4](https://user-images.githubusercontent.com/84123877/172585865-15aa266c-05aa-4b89-aa2a-c7adb64c8b2b.png)

> 이제 Elastic IP 목록에서 새로 할당된 IP 주소를 확인할 수 있다.
> 

이 상태로는 아무것도 할 수 없고, 그대로 두면 요금이 계속 부과된다.

EC2 인스턴스 등 AWS 리소스에 곡 연결해야 Elastic IP를 사용할 수 있다.

![Untitled 5](https://user-images.githubusercontent.com/84123877/172585866-5d4e5adf-ff18-40dd-837f-28f33d3fbcce.png)

> 릴리스, 연결, 연결 해제 등이 나타난다. 연결을 진행한다.
> 

![Untitled 6](https://user-images.githubusercontent.com/84123877/172585868-3787127a-4468-4790-a1bb-d3c34481cf42.png)

> Elastic IP 연결
> 

EC2 인스턴스에 Elastic IP 연결 설정이 완료되었으면 연결 버튼을 클릭한다.

그럼 이제 이 공인 IP를 DNS 서버에서 도메인과 연결하거나 HTTP, SSH, RDP 등의 접속을 할 수 있다.

---
