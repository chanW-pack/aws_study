# AWS CloudFront Signed URL 사용 설정

---

## Signed URL 사용 설정하기

---

먼저 Signed URL을 사용하러면 CloudFront 배포에서 Behavior(동작, 행동) 설정이 필요하다.

본인은 실습을 위해 새로 웹 서버를 생성하고 CloudFront로 배포중인 상태이다.

![Untitled](https://user-images.githubusercontent.com/84123877/185778184-f023fb5a-22a5-402f-a6a6-46b03f5d692d.png)

> CloudFront 배포 목록에서 EC2와 연동한 CloudFront 배포를 선택한다.
> 

선택한 CloudFront 배포에서 동작 탭으로 이동하여 defult 동작을 편집하였다.

![Untitled 1](https://user-images.githubusercontent.com/84123877/185778170-555d5869-1044-4b71-98c9-74a1bc48e28c.png)

> CloudFront Restrict Viewer Access 설정
> 

뷰어 엑세스 제한을 Yes로 변경하여 Signed URL을 사용했다.

신뢰할 수 있는 인증 유형으로 키 그룹을 선택할 수 있는 Trusted Key groups,
서명자(사용자)를 직접 선택할 수 있는 Trusted signer 가 있다.

본인은 셀프를 선택하여 진행했다.

![Untitled 2](https://user-images.githubusercontent.com/84123877/185778172-788eaf62-c8c5-4439-ae4a-4098529cd11e.png)

> 설정 저장 후 상태가 활성화로 바뀌고 접속을 테스트하였다.
> 

MissingKey라는 에러가 발생하면서 파일 내용이 표시되지 않는다.
이제 Signed URL에 맞는 파리미터들이 없으면 파일의 내용을 볼 수 없다.

## Signed URL 서명을 위한 CloudFront 키 쌍 생성

---

Signed URL을 생성하려면 먼저 CloudFront 전용 키 쌍과 액세스 키를 생성해야 한다.

이 키 쌍의 개인 키를 사용하여 정책을 서명하게 된다.
(정책을 서명하는 이유는 정책 변조를 확인,검증하기 위해서이다.)

![Untitled 3](https://user-images.githubusercontent.com/84123877/185778173-5098d561-82f8-4ff1-b004-80f285b99587.png)

> AWS 콘솔에서 계정을 선택하여 **보안 자격 증명** 탭으로 이동하여 AWS 액세스 키를 생성 
가능하다. 본인은 생략하겠다.
> 

![Untitled 4](https://user-images.githubusercontent.com/84123877/185778174-049554ae-d5ce-4d9d-bf64-18eb7a2ef23a.png)

> CloudFront 전용 키를 생성하기 위해 키 관리 → 퍼블릭 키로 이동한다.
> 

<aside>
💡 CloudFront에서 Signed URLs 를 사용하려면 ‘루트 사용자가 aws에서 key pair 생성하도록 
하는 방법’과 ‘키 그룹을 생성하는 방법’이 있는데 aws에선 키 그룹을 생성하는 방법을 
권장한다. 본인은 두 가지를 실습해보겠다.

</aside>

<aside>
💡 두 가지를 실습해보려 했는데 루트 계정만 콘솔에서 키 페어를 만들 수 있다고 한다.
본인은 IAM 계정이기 때문에 openSSL로 키 페어를 생성하고 그룹화하겠다.

</aside>

### RSA Key pair 생성

1. OpenSSL을 사용하여 길이가 2048인 RSA 키 페어를 생성하고
private_key.pem 이라는 파일에 저장한다.

```bash
openssl genrsa -out private_key.pem 2048
```

![Untitled 5](https://user-images.githubusercontent.com/84123877/185778175-c0d2017b-f5b6-4274-b742-aa71c5a0ab1e.png)

1. 이렇게 만들어진 파일은 공개키와 개인키를 모두 포함한다.
다음 명령어를 통해 private_key.pem 이라는 파일에서 public_key.pem 라는 파일로
공개키를 추출한다.

```bash
openssl rsa -pubout -in private_key.pem -out public_key.pem
```

![Untitled 6](https://user-images.githubusercontent.com/84123877/185778176-cb0fac31-d96e-4326-a32d-b2ff54c5303c.png)

### AWS CloudFront에 공개키 연결

**CloudFront public key 등록**

AWS의 CloudFront → Public key → Create public key 에서 공개키를 등록할 수 있다.
Name을 지정하고 Key를 입력하는 곳에 public_key를 등록한다.

![Untitled 7](https://user-images.githubusercontent.com/84123877/185778177-dc246ee8-5707-4921-a36e-91fa60dc2066.png)

> Create public key(퍼블릭 키 생성)를 누르면 공개키 등록이 완료되고 
등록된 키를 목록페이지에서 확인 가능하다.
> 

![Untitled 8](https://user-images.githubusercontent.com/84123877/185778179-47da4779-1e0e-4806-b499-bb0eacadec15.png)

### CloudFront Key groups 등록

등록 이후 키 그룹 탭을 선택해서 키 그룹 생성 페이지로 넘어간다.

방금 생성한 공개키를 선택하여 키 그룹 생성을 완료했다.

![Untitled 9](https://user-images.githubusercontent.com/84123877/185778180-4cc2b1be-301d-4111-af7f-6814b293a9ca.png)

![Untitled 10](https://user-images.githubusercontent.com/84123877/185778181-99ae48f8-9462-42dc-a0d8-54306ed2a5ca.png)

### 해당 CloudFront id에 key groups 연결

다시 CloudFront의 동작탭으로 이동하여 뷰어 액세스 제한을 변경한다.

![Untitled 11](https://user-images.githubusercontent.com/84123877/185778182-d5bedcd3-4cbc-4787-84ed-b75e6d59239e.png)

> 뷰어 액세스 제한을 선택하고 방금 만든 키 그룹을 등록한다.
> 

이제 ****Signed URL****를 생성하여 접속하는 실습을 다음 페이지에서 진행하겠다.

---
