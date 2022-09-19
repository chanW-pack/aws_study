# Canned Policy / Custom Policy 를 사용한 Signed URL 생성

---

CloudFront 키 페어 생성을 완료했다.

이제 본격적으로 Signed URL을 직접 생성해보겠다.

Signed URL은 보통 애플리케이션에서 자체적으로 생성하여 사용한다.
따라서 Java, PHP, C# 등의 프로그래밍 언어로 생성하는 것이 편리하다.

하지만 프로그래밍 언어로 하나하나 실습하기에는 내용이 길어지므로 Linux에서 커맨드라인
생성으로 간단하게 생성해보도록 하겠다.

## Canned Policy를 이용하여 Signed URL 생성

---

**Canned Policy**에서 **Canned**는 **미리 준비되어 있는 상태**를 이야기한다.
따라서 CloudFront 배포 서버에서 정책을 이미 알고 있기 때문에 URL에 정책 내용이 
포함되지 않는다.

Linux OS에서 텍스트 편집기를 열고 아래와 같이 작성한 뒤 canned_policy.js로 저장했다.

```bash
{
  "Statement": [{
    "Resource":"https://d3g6t0mzbstk6q.cloudfront.net",
    "Condition": {
      "DateLessThan": {
        "AWS:EpochTime": 1654233920
      }
    }
  }]
}
```

![Untitled](https://user-images.githubusercontent.com/84123877/190934694-d30fff07-b2b8-4ccd-8160-892fbed7856b.png)

- **Resource** 
: 제가 생성한 CloudFront 배포의 도메인은 d38443sd7c5866.cloudfront.net 입니다.. 
여기서 도메인은 여러분들이 생성한 CloudFront 배포의 도메인을 입력해야 합니다. 
(본인 주소로 그대로 하면 안돼~)
https://d3g6t0mzbstk6q.cloudfront.net 과 같이 http://를 포함한 CloudFront 배포 도메인과 파일의 경로를 포함한 파일명을 입력합니다.
- **DateLessThan → AWS:EpochTime** 
: Signed URL에서는 이 만료날짜 값이 필수입니다. 그리고 UTC 형식을 사용해야 합니다. 위의 정책 파일에서 1654233920 값은 이미 지나가버린 시간 값이므로 그대로 사용하면 Signed URL이 동작하지 않습니다. 꼭 미래의 시간 값을 사용하기 바랍니다.
(1654233920 값은 2022/06/03 14:25 값임…)

<aside>
💡 **웹에서 UTC 값 생성하기**
UTC 시간 값을 사람이 직접 계산하는 것은 무리가 있다. 
이 값은 프로그래밍 언어를 통하지 않고도 인터넷에서 쉽게 생성할 수 있다.

http://www.epochconverter.com에 접속한다.

![https://pyrasis.com/assets/images/TheArtOfAmazonWebServicesChapter12/52_.png](https://pyrasis.com/assets/images/TheArtOfAmazonWebServicesChapter12/52_.png)

원하는 날짜와 시간을 입력한 뒤 Human date to Timestamp 버튼을 클릭하면 
Epoch timestamp에 UTC 형식의 값을 얻을 수 있다. 
이 사이트 이외에도 구글에서 utc converter라고 검색하면 같은 기능을 가진 비슷한 사이트가 많이 있습니다.

</aside>

```bash
{"Statement":[{"Resource":"https://d3g6t0mzbstk6q.cloudfront.net","Condition":{"DateLe
ssThan":{"AWS:EpochTime":1654233920}}}]}
```

![Untitled 1](https://user-images.githubusercontent.com/84123877/190934686-6e7046a4-0fd8-48d5-aee4-50034695fe9c.png)

정책 파일의 공백을 모두 제거하고 한 줄로 만든다.
공백과 개행문자(새 줄)이 있으면 Signed URL이 동작하지 않는다.

특히, 파일 맨 끝의 개행문자는 꼭 제거해야 한다.

<aside>
💡 **Vim에서 공백 및 개행문자 제거하기**
공백 삭제하기

`:%s/ //g`

개행문자 제거하기(Windows에서는 \r\n)

`:%s/\n//g`

파일 맨 끝의(EOL) 개행문자 제거하기

`:set binary
:set noeol`

</aside>

공백과 개행문자를 제거했으면 :wq를 입력하여 파일을 저장하고 Vim을 종료한다.

다음과 같이 명령을 입력하여 서명 값(Signeture)를 생성한다.

```bash
root@ip-172-31-1-11:/home# cat canned_policy.js | openssl sha1 -sign private_key.pem | 
openssl base64 | tr '+=/' '-_~'

wrH9XngbvVMOJWozrxqqjF9D9eVLsCdrVzXIcYF7WabDSbBKERvcDNZ98tv-jYmX
zAQSZ19-3XTgyTXUWWk5~ZWBo3CXZLw5zH9fKqX4UeQKVVH7yhYbGSSVrwQSOBYx
fFgYLZw-~5aRVBP9UQMUmbYEoOWQv06GSWzlgp7qxfGsk3TEHTOb82LZljQVS9Or
Q5G2f5cdwyqqOOhufCST~dYs7tS0Qypw593YMpXg~wQgJXwqzLGpIB8FXYkNlesv
afRNIF7Qe8nChIEYVaHvyZ~oj4MgZGcAs0GZtBpiQaHsHXLUuIYOSVFffM6bgfIs
njBH1Pv7VrHuVfIEmxe4Mw__
```

![Untitled 2](https://user-images.githubusercontent.com/84123877/190934688-72afa1b5-6f15-4ab4-b857-9236ee2ec2d3.png)

- **cat canned_policy.json** 
: 방금 작성한 canned_policy.json 파일 이다.
- **openssl sha1 -sign pk-APKAJERVTR4A7EO47UYA.pem** 
: private_key.pem는 CloudFront 키(Key Pair)에서 개인 키 파일이다. 
본인들이 생성한 개인 키 파일을 지정한다.
- **openssl base64** 
: 서명 값(Signature)를 BASE64로 인코딩한다.
- t**r ‘+=/’ ‘-_~’** 
: BASE64로 인코딩 된 값 중에서 URL로 인식될 수 있는 문자를 다른 것으로 바꾼다.

**이후 메모장이나 기타 텍스트 편집기에서 아래와 같이 Signed URL을 직접 생성한다.**

- http://d38443sd7c5866.cloudfront.net/index.html: 접속할 CloudFront 도메인과 파일명입니다. 이 도메인은 여러분들이 생성한 CloudFront 배포 도메인을 입력합니다.
- Expires: Signed URL이 만료되는 날짜와 시간 값 입니다. 여러분이 canned_policy.json에 지정한 DateLessThan → AWS:EpochTime 값과 동일해야 합니다.
- Signature: 생성한 서명 값을 이곳에 붙입니다. 생성할 때는 여러 줄로 출력이 되지만 URL로 사용할 때는 한 줄로 만들어서 붙여야 합니다.
- Key-Pair-Id: CloudFront 액세스 키 값 입니다. 여러분들이 생성한 액세스 키 값을 입력합니다.
(AWS 액세스키 값이 아니다. CloudFront 액세스 키 ID 값이다.)

---

---

---

### 0607 Signed URL이 작동되지 않아 원인 파악중

ㄴ 나머지 URL부분 변경 시 오류가 나타나는데 서명 부분은 아무렇게나 수정을 해도 오류가 같음

→ 서명부분의 오류일 가능성이 커보임

→ 깁협매니저의 도움으로 서명 재생성 진행해보겠음

**EC2가 아닌 S3로 먼저 실습 진행해보겠다.** 

![Untitled 3](https://user-images.githubusercontent.com/84123877/190934690-e4f19a15-2e7e-4809-82cd-95097b111a56.png)

> 위 내용과 다르게  JS 파일을 띄어쓰기, 공백 등을 없애지 않고 그대로 저장한다.
(S3 주소가 아닌 CloudFront 주소에서 html을 추가해주어야 한다.)
> 

서명 생성 명령어도 위 내용과 살짝 변경되었다.
js파일에서 직접 공백, 띄어쓰기 등을 없애지 않고 서명 생성 시에 함께 정리했다.

```bash
$ cat policy | tr -d "\n" | tr -d " \t\n\r" | openssl sha1 -sign private_key.pem 
| openssl base64 -A | tr -- '+=/' '-_~'
```

항목의 내용은 다음과 같다.

1. cat 은 policy 파일을 읽는다.
2. tr -d “\n” | tr -d “ \t\n\r” 은 cat에 의해 추가된 공백과 줄 바꿈 문자를 제거한다.
3. OpenSSL은 SHA-1 함수를 사용하여 파일을 해시하고 RSA 및 프라이빗 키 파일을 사용하여 
서명한다. private_key.pem
4. OpenSSL은 해시, 서명된 정책 설명을 base64로 인코딩한다. 
(-A로 띄어쓰기 없이 한 줄로 서명이 생성된다.)
5. tr은 URL 쿼리 문자열 파라미터에 사용할 수없는 문자를 유효한 문자로 교체한다.

이후 생성된 서명으로 위와 같이 Signed URL을 생성한다.

```bash
접속할CloudFront도메인?Expires=설정한timestamp&Signature=생성된서명값
&Key-Pair-Id=액세스키값
```

```bash
http://d1vsgg7l91u7ol.cloudfront.net/index1234.html?Expires=1654652946&Signature=
AZNKdhFvy3ge73U3AYkAgmGvc~rufkNtEi7IS9fd3CL2eSq08pDZueFrnBrctdODirUElltcXvVpFeuS9
vT1AdrvU3KGxOMLuVssxsIc6UYTB8~6qnXWUmJfYxaf9lULM4~cgcH~LvrTTeCkg09wj~CWsX6izrFJAz
RBUOKPUW6xkl09igw9OEAzYyIydDRbx2Hyqbfmf6~wHi0wM0WEFlU-C1WnTj1IoogBPORt17B6RyF9CVF
NmRxar6j37NqoT08tYhNebQeZziX-eik3uRVeYGkntnx3QTqIlEfRtmQPYEgQrvIHRknigV8~mh1KVAmz
rmezKt2lUO9AeLX8rw__&Key-Pair-Id=K3R4TUUT9YFJCP
```

![Untitled 4](https://user-images.githubusercontent.com/84123877/190934691-34973a5d-09e2-4caa-8040-124129a4551c.png)

> 접속 시도 성공했다… 결국 서명의 문제가 맞는듯했다..
> 

또, URL에 http <s> 를 빼고, index.html/? 에서 /를 제거하고 진행해야한다.

<aside>
💡 **CloudFront Signed URL과 Restrict Bucket Access**
CloudFront를 Signed URL로 제한한다 하더라도 S3의 접근 권한이 열려 있으면 아무런 소용이 없다. 이렇게 되면 Signed URL을 사용하지 않고, S3으로 바로 접근하여 파일을 가져갈 수 있다. 따라서 S3에는 CloudFront만 접근할 수 있도록 꼭 Restrict Bucket Access를 설정한다.

S3가 아닌 EC2 인스턴스나 외부 웹 서버를 오리진으로 사용한다면 웹 서버 레벨에서 CloudFront 만 접근 가능하도록 설정해야 한다. 접근해오는 서버가 CloudFront 인지 판별하는 방법은 HTTP 헤더의 User-Agent의 내용을 확인하면 된다. CloudFront에서 접속해왔다면 헤더에는 "Amazon CloudFront"라고 표시된다.

</aside>

### Custom Policy로 Signed URL 생성도 동일하기 때문에 넘어간다.
(JS 부분만 다르므로 참고할것)

## 무효화로 CloudFront 컨텐츠 갱신하기

---

CloudFront로 배포되는 파일의 캐시가 유지되는 기본 시간은 24시간이며,
오리진 HTTP 헤더의 캐시 설정을 이용하여 캐시가 유지되는 시간을 자유롭게 설정할 수 있다.

지난 페이지에서 무효화 요청을 진행했었기 때문에 넘어가겠다.
([https://www.notion.so/AWS-CloudFront-EC2-9fd58851aff646f680640faec679ee97](https://www.notion.so/AWS-CloudFront-EC2-9fd58851aff646f680640faec679ee97))

<aside>
💡 **무효화 요청 없이 빠르게 파일 갱신하기**

무효화 요청은 아무리 빨라도 10분 이상 걸린다. 
따라서 CloudFront는 에지 로케이션에 파일의 캐시가 없을 때 오리진에서 파일을 새로 
가져온다는 특성을 이용하여 파일의 내용이 바뀔 때마다 디렉터리 경로나 파일명을 바꾸는 방식으로 운영이 가능하다.

예)

/images/hello-1.png → /images/hello-2.png

/css/3f2c527a/hello.css → /css/f8bc2d31/hello.css

/css/1/world.js → /css/2/world.js

</aside>

이렇게 하면 파일의 내용이 바뀔 때 항상 캐시가 없는 새로운 파일이 되므로 무효화 요청 없이
짧은 시간 안에 파일을 갱신할 수 있다.
(빠르면 2~3초, 늦어도 1분 안에 갱신이 가능하다.)

단, 모든 파일의 캐시 유지 시간을 짧게 설정하면 오리진에 접속하여 새 파일을 가져오는 주기가
짧아지므로 전체적인 성능이 느려지게 된다.

즉, 오리진에서 새  파일을 가져오는 속도는 다소 느리므로 이렇게 되면 CloudFront 캐시를 사용하는
의미가 없어지게 된다.

(알고만 있으면 좋을듯하다.)

---
