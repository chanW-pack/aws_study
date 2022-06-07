# Security Group으로 방화벽 설정하기

---

Sercurity Group은 EC2 인스턴스에 적용할 수 있는 방화벽 설정입니다.

서버 보안의 기본은 방화벽 설정에서 출발합니다.

예로, Linux 서버의 SSH 접속 포트인 22번만 여는 것은 기본이고, 

여기에 접속 가능한 IP 대역까지 설정하면 공격 위협이 상당수 줄어듭니다.

![Untitled](Security%20Group%E1%84%8B%E1%85%B3%E1%84%85%E1%85%A9%20%E1%84%87%E1%85%A1%E1%86%BC%E1%84%92%E1%85%AA%E1%84%87%E1%85%A7%E1%86%A8%20%E1%84%89%E1%85%A5%E1%86%AF%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%92%E1%85%A1%E1%84%80%E1%85%B5%207b92deba99964adeb08a99fe889b8349/Untitled.png)

> EC2 인스턴스와 방화벽 개념도
> 

방화벽 설정의 기본요소는 다음과 같습니다.

- Inbound : 외부에서 EC2 인스턴스로 들어오는 트래픽입니다. 대표적인 것들로느
HTTS, HTTPS, SSH, RDP 등이 있습니다.
- Outbound : EC2 인스턴스에서 외부로 나가는 트래픽입니다. EC2 인스턴스 안에서 인터넷을 사용할 경우 Outbound라 할 수 있습니다. 파일을 다운로드하거나 외부 SSH로 접속 등이 있습니다.
- Type : 프로토콜 형태입니다.크게 TCP, UDP, ICMP로 나눌 수 있습니다.
- Port, Port Range : 포트 번호입니다. TCP, UDP 프로토콜은 0~65535 사이의 포트 번호를 사용합니다. (ICMP은 포트 번호를 사용하지 않습니다.)
- Source/Desination : 연결 혹은 접속 가능한 IP 대역을 뜻합니다. Inbound 일 경우 Source,
Outbound 일 경우 Destination 이라 부릅니다. IP 주소 하나만 지정할 수도 있고 CIDR 표기 방법을 이용하여 일정한 대역을 설정 할 수 있습니다.
- Rule : 지금까지 설명한 Inbound, outbound, Type, Port, Source/Destination 을 조합한 것을 
Rule(규칙) 이라고 합니다.

![Untitled](Security%20Group%E1%84%8B%E1%85%B3%E1%84%85%E1%85%A9%20%E1%84%87%E1%85%A1%E1%86%BC%E1%84%92%E1%85%AA%E1%84%87%E1%85%A7%E1%86%A8%20%E1%84%89%E1%85%A5%E1%86%AF%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%92%E1%85%A1%E1%84%80%E1%85%B5%207b92deba99964adeb08a99fe889b8349/Untitled%201.png)

> AWS 콘솔에 EC2 페이지에서 네트워크 및 보안 → 보안 그룹 을 선택하면 목록이 표시된다.
> 

Group name 이 default인 것은 기본적으로 제공되는 보안 그룹이며,

인바운드는 동일한 보안 그룹인 default에만 허용되어 있고,

아웃바운드는 모든 접속이 허용되어 있습니다.

![Untitled](Security%20Group%E1%84%8B%E1%85%B3%E1%84%85%E1%85%A9%20%E1%84%87%E1%85%A1%E1%86%BC%E1%84%92%E1%85%AA%E1%84%87%E1%85%A7%E1%86%A8%20%E1%84%89%E1%85%A5%E1%86%AF%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%92%E1%85%A1%E1%84%80%E1%85%B5%207b92deba99964adeb08a99fe889b8349/Untitled%202.png)

> Inbound 규칙을 설정하는 창이다. 본인은 SSH(22.port)를 허용하는 규칙을 생성했다.
> 
- Type : HTTP를 선택하면 자동으로 Protocol은 TCP, Port Range는 80으로 설정된다.
- Source : 기본값 그대로 Anywhere를 사용한다. Coustom IP를 선택하면 IP 주소 대역을 설정
할 수 있고, MY IP를 선택하면 현재 내가 사용하는 공인 IP로 자동으로 설정한다.

![Untitled](Security%20Group%E1%84%8B%E1%85%B3%E1%84%85%E1%85%A9%20%E1%84%87%E1%85%A1%E1%86%BC%E1%84%92%E1%85%AA%E1%84%87%E1%85%A7%E1%86%A8%20%E1%84%89%E1%85%A5%E1%86%AF%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%92%E1%85%A1%E1%84%80%E1%85%B5%207b92deba99964adeb08a99fe889b8349/Untitled%203.png)

> CW 보안 그룹의 Inbound 목록을 보면 HTTP 규칙이 추가된 것을 확인 가능하다.
> 

<aside>
💡 **CIDR 표기법**
CIDR은 Classless Inter-Domain Routing의 약어로 IP 주소 할당 방법입니다. 급격히 부족해지는 IPv4 주소를 보다 효율적으로 사용하기 위해 CIDR 표기법을 사용합니다. xxx.xxx.xxx.xxx/yy 형태로 표기하는데 맨 뒤의 yy는 Subnet Mask를 2진수로 바꾸었을 때 1의 개수입니다.

255.255.255.0을 2진수로 바꾸면 11111111.11111111.11111111.00000000이 됩니다. CIDR 표기법으로는 xxx.xxx.xxx.xxx/24가 됩니다. 192.168.0.0/24라면 192.168.0.1부터 192.168.0.254까지 라는 의미입니다(192.168.0.0는 네트워크 192.168.0.255는 브로드캐스트). 192.168.0.15/32이면 1이 32개이고 Subnet Mask가 255.255.255.255가 되므로 192.168.0.15 한 개의 IP만 표현하게 됩니다.

</aside>

<aside>
💡 **방화벽 설정은 항상 체크**

EC2 인스턴스를 생성하고 열심히 여러 가지 서버들을 설치했는데 외부에서 접속이 되지 않을 때가 많습니다. 이럴 때에는 먼저 Security Group 설정을 확인합니다. 설치된 서버들이 사용하는 포트가 Inbound 설정에서 열려 있는지, 포트 번호가 잘못 설정되지는 않았는지, 프로토콜이 잘못 설정되지는 않았는지, Inbound 설정을 Outbound에다가 하지는 않았는지 살펴봅니다.

</aside>

---