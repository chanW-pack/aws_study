### CloudFront 캐싱 확인하기

배포까지 완료되었고 CloudFront에서 캐싱이 되는지 확인하겠다.

```bash
cd /var/log/apache2
$ tail -f access_log
```

> 로그를 띄우고 이제 EC2 DNS로 접속해보고 CloudFront DNS로 접속하여 비교해보았다.
> 

![Untitled 3](https://user-images.githubusercontent.com/84123877/184263404-70d4bf8a-85ac-49e3-83c1-049ad55673d0.png)

> EC2 DNS로 접속하면 막 로그가 뜨는데, CloudFront DNS로 웹서버에 접속하면
로그가 쌓이지 않는다.
이는 캐싱이 되고 있어 사용자에게 CloudFront가 직접 보여주기 때문에 EC2 서버에 
요청 자체가 가지 않았기 때문이다.
> 

이처럼 강력한 캐싱 기능으로 사용자에게 컨텐츠 서비스를 빠르게 제공할 수 있다는 점을 확인했다.

### CloudFront 무효화 생성

생성했던 Index.html의 삽입이미지를 **수정하여 저장**했다.

![Untitled 12](https://user-images.githubusercontent.com/84123877/184263415-4af13844-b5ba-4fc3-8d2e-872bf15722f4.png)

> ←왼쪽 CloudFront DNS |  오른쪽 EC2 DNS 접속 →
> 

이후 CloudFront와 EC2 접속을 비교해보았는데,
CloudFront가 기존의 내용을 이미 캐싱하고 있기 때문에 바로 **수정내용이 반영되지 않았다.**

물론 위에서 설정한 캐시 정책(TTL)에 따라 일정 시간이 지나면 캐싱 내용이 업데이트 되겠지만,
만일 급한 중요한 버그 수정과 같은 상황이라면 마냥 기다리기에는 치명적인 결과가 일어날 수 있다.

**따라서 무효화 기능(invalidation)을 통해 수정된 파일이 캐시로 바로 반영하도록 설정해야 한다.**

<aside>
💡 **파일 무효화(invalidation)**
- TTL이 지나기 전에 강제로 캐시를 삭제 하는 것
- 주로 새로운 파일을 업데이트 한 후 TTL을 기다리지 못할 상황에 사용
- 추가 비용 발생 (한달 1000건은 무료. 초과는 요청 하나당 5-6원 받음)
- CloudFront API 콘솔, third-Party 툴 등을 사용해서 파일 무효화 기능 사용 가능

</aside>

![Untitled 13](https://user-images.githubusercontent.com/84123877/184263417-92ec16b7-53dc-4287-8489-d1e1761bc2b2.png)

> CloudFront 배포 모델을 선택하고 무효화를 생성한다.
> 

![Untitled 14](https://user-images.githubusercontent.com/84123877/184263419-3c5b4bc9-5fba-41c9-afe4-9f21dc474169.png)

![Untitled 15](https://user-images.githubusercontent.com/84123877/184263421-41db3a74-7d6a-4570-8afa-1926bcbe4f3c.png)

> CloudFront 무효화 설정 완료
> 

이렇게 어떤 파일(경로)에 대한 무효화를 진행하게 되면, CloudFront는 등록한 파일이
캐시에 없다고 판단하고 다시 새롭게 파일을 가져오게 된다.

이제 다시 CloudFront DNS로 접속해보면 수정내용이 반영된것을 확인 가능하다.

<aside>
💡 만약에 배포를 하는데, 프로젝트가 처음에 수정이 많을 경우 TTL을 60초로 짧게 맞춘다던지
해서 개발을 진행하고, 나중에 안정화 되면 다시 TTL을 늘리는 식으로 하면 좋을듯 하다.

</aside>

### CloudFront 삭제

CloudFront는 전세계에서 배포되는 서비스이기 때문에 바로 삭제가 불가능하다.

우선 비활성화를 한뒤 삭제를 진행해야 한다.  

이 역시 시간이 오래 걸린다.

![Untitled 16](https://user-images.githubusercontent.com/84123877/184263423-bc91b9e1-8818-435d-8509-b4d3f198426f.png)

---
