## 공유와 권한 (S3 객체 권환 관리하기)

---

![Untitled](AWS%20S3(Simple%20Storage%20Service)%207dee98544fa34b10b2098465cf33441d/Untitled%2014.png)

> S3에 올려놓은 폴더나 파일을 다른 사람이 볼 수 있게 할 수 있다.
먼저 사진.jpg를 선택하고 속성탭에 들어간다.
그리고 우측 하단에 객체URL을 복사한다.
> 

![Untitled](AWS%20S3(Simple%20Storage%20Service)%207dee98544fa34b10b2098465cf33441d/Untitled%2015.png)

> 웹에서 실행시켜보면 접근 거부라고 나타난다.
> 

![Untitled](AWS%20S3(Simple%20Storage%20Service)%207dee98544fa34b10b2098465cf33441d/Untitled%2016.png)

> 파일 권한 탭에서 액세스 제어 목록으로 들어간다.
모든 사람 읽기를 체크하였다. (오른쪽은 객체 ACL 읽기인데, 객체만 읽고 싶으면 객체 읽기만)
> 

![Untitled](AWS%20S3(Simple%20Storage%20Service)%207dee98544fa34b10b2098465cf33441d/Untitled%2017.png)

> 다시 해당 링크로 접속해보면 파일 내용이 출력되는것을 확인 가능하다.
> 

---

## SC 버킷 권한 관리하기

---

이번에는 버킷에 권한을 설정하겠다.

버킷에 권한을 설정하면 버킷 안에 든 모든 객체에 적용된다.

![Untitled](AWS%20S3(Simple%20Storage%20Service)%207dee98544fa34b10b2098465cf33441d/Untitled%2018.png)

> S3에 새 파일을 업로드하였다.
> 

![Untitled](AWS%20S3(Simple%20Storage%20Service)%207dee98544fa34b10b2098465cf33441d/Untitled%2019.png)

> 이 URL은 웹 브라우저로 접속해도 그림 파일이 열리지 않는 상태이다.
> 

![Untitled](AWS%20S3(Simple%20Storage%20Service)%207dee98544fa34b10b2098465cf33441d/Untitled%2020.png)

> 버킷 목록으로 돌아가 해당 버킷의 권한탭으로 이동한다.
이후 버킷 정책 편집기를 수정한다.
> 

![Untitled](AWS%20S3(Simple%20Storage%20Service)%207dee98544fa34b10b2098465cf33441d/Untitled%2021.png)

> 버킷 정책 편집기가 표시된다. 우측 상단 정책 생성기를 선택한다.
> 

![Untitled](AWS%20S3(Simple%20Storage%20Service)%207dee98544fa34b10b2098465cf33441d/Untitled%2022.png)

> 브라우저 새 창에서 AWS Policy Generator 페이지가 열렸다.
> 

- Select Type of Policy: SNS Topic Policy, IAM Policy, SQS Queue Policy, S3 Bucket Policy를 선택할 수 있다. 여기서는 S3 Bucket Policy를 선택한다.
- Effect: 허용(Allow), 거부(Deny)를 선택할 수 있다. 기본값 그대로 Allow를 선택한다.
- Principal: 권한을 적용할 사용자다. IAM 계정을 사용할 수 있다. 여기서는 인터넷에 전체 공개할 것이므로 *을 입력한다.
- AWS Service: 처음 Select Type of Policy를 선택하면 그에 맞는 서비스가 자동으로 선택된다. 기본값 그대로 사용한다.
- Actions: S3의 모든 액션이 표시된다. 우리는 파일을 받는 액션만 사용할 것이므로 GetObject를 선택한다.
- Amazon Resource Name(ARN): 아마존의 리소스를 고유하게 표현하는 방식이다. arn:aws:s3:::cws3test1/*을 입력한다. 이것은 AWS의 S3에서 cws3test1 아래 모든 파일을 뜻한다. /* 대신 /cat.gif 등으로 특정 파일을 지정해 줄 수도 있다.

<aside>
💡 Add Conditions에서 UserAgent, SourceIP 등 다양한 조건을 사용할 수 있다. 
본인이 필요할 때 하나하나 사용하면 된다.

</aside>

![Untitled](AWS%20S3(Simple%20Storage%20Service)%207dee98544fa34b10b2098465cf33441d/Untitled%2023.png)

> AWS 정책이 생성되었다. 이 JSON 텍스트를 전체 선택 후 복사한다.
> 

![Untitled](AWS%20S3(Simple%20Storage%20Service)%207dee98544fa34b10b2098465cf33441d/Untitled%2024.png)

> 복사 후 저장하려는데 비킷 정책을 편집할 권한이 없다고한다……….
회사 계정에서 IAM 사용자로 진행하는 터라 권한이 막혔나보다……..
> 

![Untitled](AWS%20S3(Simple%20Storage%20Service)%207dee98544fa34b10b2098465cf33441d/Untitled%2025.png)

![Untitled](AWS%20S3(Simple%20Storage%20Service)%207dee98544fa34b10b2098465cf33441d/Untitled%2026.png)

> 개인계정으로 이어서 진행하였다. 버킷 권한 해제가 잘 마무리 되었다.
> 

![Untitled](AWS%20S3(Simple%20Storage%20Service)%207dee98544fa34b10b2098465cf33441d/Untitled%2027.png)

> 그런데 사실 개인 계정으로도 권한이 막혀있었다.
IAM계정이라 권한이 없던게 아니라
그냥 내가 버킷에 액세스를 차단시켜서 수정이 불가했던것…..
차단을 해제하니 잘 작동되었다.
> 

---
