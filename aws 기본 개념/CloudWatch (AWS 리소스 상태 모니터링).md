# CloudWatch (AWS 리소스 상태 모니터링)

---

CloudWatch는 AWS 리소스의  상태를 모니터링 하는  서비스입니다.

모니터링 뿐만 아니라 측정치(Metric)와 연계하여 다양한 액션(Action)을 사용할 수 있습니다.

<aside>
💡 프리 티어에서 사용 가능
CloudWatch는 프리 티어에서 무료로 사용가능 합니다.
2022년 5월 기준으로 기본 모니터링(5분 간격), 측정치(Metric) 10개, 알람(Alarm) 10개, 
매달 API 요쳥 1백만개 건을 무료로 사용할 수 있습니다.

</aside>

CloudWatch는 EC2 인스턴스가 이상이 있을 경우 알림을 받고자 할 때, 사용량이 급증했을 때 

자동으로 횡적 확장(Auto Scaling)을 하고 부하 분산(Elastic Load Balancing)을 구축할 때 

사용합니다.

![Untitled](CloudWatch%20(AWS%20%E1%84%85%E1%85%B5%E1%84%89%E1%85%A9%E1%84%89%E1%85%B3%20%E1%84%89%E1%85%A1%E1%86%BC%E1%84%90%E1%85%A2%20%E1%84%86%E1%85%A9%E1%84%82%E1%85%B5%E1%84%90%E1%85%A5%E1%84%85%E1%85%B5%E1%86%BC)%203713f6eada7a4b36b738ec66d4e04b3e/Untitled.png)

> EC2 인스턴스와 CloudWatch 액션
> 

CloudWatch에서는 각 AWS 리소스의특징에 따라 다양한 값들을 모니터링할 수 있습니다.

CloudWatch이 제공하는 측정목록 이외에도 사용자가 직접 생성한 커스텀 측정치(Custum Metric)

도 사용할 수 있습니다.

기본 모니터링 간격은 5분이며 세부 모니터링 간격은 1분입니다.

기본 모니터링은 프리 티어에서 무료로 사용할 수 있으며 

세부 모니터링은 추가 요금을 지불해야 합니다.

- EC2 인스턴스: 
CPU 사용률, 데이터 전송량, 디스크 사용량 등을 모니터링할 수 있습니다. 측정치와 연계하여 알림(Notification), 자동 횡적 확장(Auto Scaling), EC2 인스턴스 제어 액션을 사용할 수 있습니다.

- EBS 볼륨: 
읽기/쓰기 사용량, 지연 시간 등을 모니터링할 수 있습니다. 측정치와 연계하여 알림(Notification), 자동 횡적 확장(Auto Scaling) 액션을 사용할 수 있습니다.

- ELB(Elastic Load Balancing): 
요청 수 및 지연 시간 등을 모니터링할 수 있습니다. 측정치와 연계하여 알림(Notification), 자동 횡적 확장(Auto Scaling) 액션을 사용할 수 있습니다.

- RDS(Relational Database Service): 
CPU 사용률, DB 연결 수, 사용 가능한 메모리 및 스토리지 공간, 읽기/쓰기 지연 시간 등을 모니터링할 수 있습니다. 측정치와 연계하여 알림(Notification), 자동 횡적 확장(Auto Scaling) 액션을 사용할 수 있습니다.

- DynamoDB: 
테이블 인덱스와 글로벌, 로컬 보조 인덱스에서 소모한 읽기/쓰기 용량 유닛, 스캔, 쿼리, 아이템 추가 및 수정(PutItem), 아이템 삭제(Delete)를 모니터링 할 수 있습니다. 측정치와 연계하여 알림(Notification), 자동 횡적 확장(Auto Scaling) 액션을 사용할 수 있습니다.

- ElastiCache: 
CPU 사용률, 데이터 읽기/쓰기, 네트워크 사용량, 캐시 엔진의 각 명령어 사용량 등을 모니터링 할 수 있습니다. 측정치와 연계하여 알림(Notification), 자동 횡적 확장(Auto Scaling) 액션을 사용할 수 있습니다.

- SNS(Simple Notification Service): 
게시(Published) 및 전송(Delivered) 메시지 수 등을 모니터링할 수 있습니다. 측정치와 연계하여 알림(Notification), 자동 횡적 확장(Auto Scaling) 액션을 사용할 수 있습니다.

- SQS(Simple Queue Service): 
전송(Send) 및 수신(Received) 된 메시지 수 등을 모니터링할 수 있습니다. 측정치와 연계하여 알림(Notification), 자동 횡적 확장(Auto Scaling) 액션을 사용할 수 있습니다.

## CloudWatch 알람 생성하기

---

EC2 인스턴스의 CPU 사용률을 모니터링하고 설정한 측정치에 도달하면

알림 메일을 전송하도록 알람을 생성해 보겠습니다.

먼저 인스턴스를 1분 단위로 모니터링 가능하도록 세부 모니터링 설정을 합니다.

<aside>
🚫 세부 모니터링 설정은 프리 티어에서 사용할 수 없으며 추가 요금을 지불해야 합니다.
추가 요금을 지불하고 싶지 않다면 이 부분은 건너뛰어도 되고
기본 모니터링 만으로 실습이 가능합니다. (단, 알람 동작을 확인하러면 5분 이상 대기)

</aside>

EC2 인스턴스 목록에서 EC2 인스턴스를 선택하고

오른쪽 클릭으로 팝업 메뉴를 연 뒤 Enable Detailed Monitoring을 클릭한다.

![Untitled](CloudWatch%20(AWS%20%E1%84%85%E1%85%B5%E1%84%89%E1%85%A9%E1%84%89%E1%85%B3%20%E1%84%89%E1%85%A1%E1%86%BC%E1%84%90%E1%85%A2%20%E1%84%86%E1%85%A9%E1%84%82%E1%85%B5%E1%84%90%E1%85%A5%E1%84%85%E1%85%B5%E1%86%BC)%203713f6eada7a4b36b738ec66d4e04b3e/Untitled%201.png)

> 세부 모니터링 활성화 완료
> 

![Untitled](CloudWatch%20(AWS%20%E1%84%85%E1%85%B5%E1%84%89%E1%85%A9%E1%84%89%E1%85%B3%20%E1%84%89%E1%85%A1%E1%86%BC%E1%84%90%E1%85%A2%20%E1%84%86%E1%85%A9%E1%84%82%E1%85%B5%E1%84%90%E1%85%A5%E1%84%85%E1%85%B5%E1%86%BC)%203713f6eada7a4b36b738ec66d4e04b3e/Untitled%202.png)

> AWS 콘솔로 접속한 뒤 메인 화면에서 Delpyment & Management의 CloudWatch에 접속한다.
> 

![Untitled](CloudWatch%20(AWS%20%E1%84%85%E1%85%B5%E1%84%89%E1%85%A9%E1%84%89%E1%85%B3%20%E1%84%89%E1%85%A1%E1%86%BC%E1%84%90%E1%85%A2%20%E1%84%86%E1%85%A9%E1%84%82%E1%85%B5%E1%84%90%E1%85%A5%E1%84%85%E1%85%B5%E1%86%BC)%203713f6eada7a4b36b738ec66d4e04b3e/Untitled%203.png)

![Untitled](CloudWatch%20(AWS%20%E1%84%85%E1%85%B5%E1%84%89%E1%85%A9%E1%84%89%E1%85%B3%20%E1%84%89%E1%85%A1%E1%86%BC%E1%84%90%E1%85%A2%20%E1%84%86%E1%85%A9%E1%84%82%E1%85%B5%E1%84%90%E1%85%A5%E1%84%85%E1%85%B5%E1%86%BC)%203713f6eada7a4b36b738ec66d4e04b3e/Untitled%204.png)

> 이전에 생성한 CW 인스턴스에서 CPUUtilization을 선택하고,
아래 그래픽 부분에서 모니터링 간격을 1 Minute(1 분)으로 변경한다.
> 

- Name: 알람의 이름입니다. CPU Watch로 입력합니다.
- Description: 알람의 설명 입니다(입력하지 않아도 상관없습니다).
- Whenever is: 측정치를 숫자로 입력합니다. 여기서는 CPU 사용률(%)입니다. >= 30으로 설정합니다(CPU 사용률 30% 이상에서 알람 동작). 30를 입력하면 오른쪽 그래프에서 30에 해당하는 위치에 빨간색으로 선이 표시됩니다.
- Whenever for: 설정한 측정치가 몇 번 도달했을 때 알람이 동작할지 설정합니다. 이 값을 3으로 설정했다면 설정한 측정치에 연속으로 3번 도달해야 알람이 동작하게 됩니다 (현재 모니터링 간격이 1분이면 3분 연속으로 측정치에 도달). 1번이나 2번 도달했을 경우 알람은 동작하지 않습니다. 기본값 그대로 사용합니다.

![Untitled](CloudWatch%20(AWS%20%E1%84%85%E1%85%B5%E1%84%89%E1%85%A9%E1%84%89%E1%85%B3%20%E1%84%89%E1%85%A1%E1%86%BC%E1%84%90%E1%85%A2%20%E1%84%86%E1%85%A9%E1%84%82%E1%85%B5%E1%84%90%E1%85%A5%E1%84%85%E1%85%B5%E1%86%BC)%203713f6eada7a4b36b738ec66d4e04b3e/Untitled%205.png)

테스트 설정이기 때문에 무조건 알람이 발생하도록 CPU 사용률 측정치를 낮게 잡았다.

실제로 Auto Scaling과 연계할 때에는 보통 60% 또는 80%로 설정합니다.

액션은 이메일 알림(Notification)을 사용하겠습니다.

즉, CPU 사용량이 30%와 같거나 넘어서게 되면 

이메일 알림 액션이 진행됩니다.

![Untitled](CloudWatch%20(AWS%20%E1%84%85%E1%85%B5%E1%84%89%E1%85%A9%E1%84%89%E1%85%B3%20%E1%84%89%E1%85%A1%E1%86%BC%E1%84%90%E1%85%A2%20%E1%84%86%E1%85%A9%E1%84%82%E1%85%B5%E1%84%90%E1%85%A5%E1%84%85%E1%85%B5%E1%86%BC)%203713f6eada7a4b36b738ec66d4e04b3e/Untitled%206.png)

> 새 주제를 생성한 뒤, 이메일을 적용시켰다.
> 

![Untitled](CloudWatch%20(AWS%20%E1%84%85%E1%85%B5%E1%84%89%E1%85%A9%E1%84%89%E1%85%B3%20%E1%84%89%E1%85%A1%E1%86%BC%E1%84%90%E1%85%A2%20%E1%84%86%E1%85%A9%E1%84%82%E1%85%B5%E1%84%90%E1%85%A5%E1%84%85%E1%85%B5%E1%86%BC)%203713f6eada7a4b36b738ec66d4e04b3e/Untitled%207.png)

> EC2 인스턴스에 대한 CPU 사융률 알림이 생성되었다.
> 

![Untitled](CloudWatch%20(AWS%20%E1%84%85%E1%85%B5%E1%84%89%E1%85%A9%E1%84%89%E1%85%B3%20%E1%84%89%E1%85%A1%E1%86%BC%E1%84%90%E1%85%A2%20%E1%84%86%E1%85%A9%E1%84%82%E1%85%B5%E1%84%90%E1%85%A5%E1%84%85%E1%85%B5%E1%86%BC)%203713f6eada7a4b36b738ec66d4e04b3e/Untitled%208.png)

> 테스트를 위해 EC2 인스턴스 CPU를 과부화 시켜주겠다.
> 

![Untitled](CloudWatch%20(AWS%20%E1%84%85%E1%85%B5%E1%84%89%E1%85%A9%E1%84%89%E1%85%B3%20%E1%84%89%E1%85%A1%E1%86%BC%E1%84%90%E1%85%A2%20%E1%84%86%E1%85%A9%E1%84%82%E1%85%B5%E1%84%90%E1%85%A5%E1%84%85%E1%85%B5%E1%86%BC)%203713f6eada7a4b36b738ec66d4e04b3e/Untitled%209.png)

![Untitled](CloudWatch%20(AWS%20%E1%84%85%E1%85%B5%E1%84%89%E1%85%A9%E1%84%89%E1%85%B3%20%E1%84%89%E1%85%A1%E1%86%BC%E1%84%90%E1%85%A2%20%E1%84%86%E1%85%A9%E1%84%82%E1%85%B5%E1%84%90%E1%85%A5%E1%84%85%E1%85%B5%E1%86%BC)%203713f6eada7a4b36b738ec66d4e04b3e/Untitled%2010.png)

> 경고가 발생하고 지정한 SNS 주제(이메일)로 알람 메일이 발송되었다.
> 

## CloudWatch 커스텀 측정치 사용하기

---

CloudWatch에서는 기본적으로 제공하는 측정치 이외에도 

사용자가 측정한 값을 사용할 수도 있습니다.

이것을 커스텀 측정장치(Custom Metric)이라 합니다. 

커스텀 측정장치는 서버 애플리케이션, 로그 파일, 언어 레벨에서 측정치를 생성하고,

이 값들을 모니터링 하거나 CloudWatch 액션을 제어하고 싶을 때 사용합니다.

이번 예제는 Amazom Linux에서 실행을 권장한다. (AWS CLI 가 미리 설치되어 있기 때문)

하지만 본인은 UbuntuLinux로 AWS CLI이 설치되어 있지 않으므로  설치하고 진행하겠다.

(설치 방법은 : [https://aws.amazon.com/ko/cli/](https://aws.amazon.com/ko/cli/) )

pip install awscli

![Untitled](CloudWatch%20(AWS%20%E1%84%85%E1%85%B5%E1%84%89%E1%85%A9%E1%84%89%E1%85%B3%20%E1%84%89%E1%85%A1%E1%86%BC%E1%84%90%E1%85%A2%20%E1%84%86%E1%85%A9%E1%84%82%E1%85%B5%E1%84%90%E1%85%A5%E1%84%85%E1%85%B5%E1%86%BC)%203713f6eada7a4b36b738ec66d4e04b3e/Untitled%2011.png)

> AWS CLI 설치 후 생성한 액세스/시크릿 키 내용
> 

이제 AWS CLI를 사용할 수 있게 되었다.

터미널에 다음과 같이 입력하면 awscloudwatch 명령으로 커스텀 측정치를 생성 할 수 있다.

- put-metric-data : 측정치를 보내겠다는 명령이다.
- —namespace :  측정치 대 분류이다. 이 분류 안에 측정치들이 포함된다.
- —metric-name :  측정치 이름입니다.
- —value : 측정치 값입니다. 숫자로만 입력해야 하며 소수점 사용 가능합니다.

![Untitled](CloudWatch%20(AWS%20%E1%84%85%E1%85%B5%E1%84%89%E1%85%A9%E1%84%89%E1%85%B3%20%E1%84%89%E1%85%A1%E1%86%BC%E1%84%90%E1%85%A2%20%E1%84%86%E1%85%A9%E1%84%82%E1%85%B5%E1%84%90%E1%85%A5%E1%84%85%E1%85%B5%E1%86%BC)%203713f6eada7a4b36b738ec66d4e04b3e/Untitled%2012.png)

그런데 해당 명령이 계속 실행되지 않는 오류가 발생했다.

(해당 리전이 없다는 오류)

그래서 리전의 엔드포인트도 입력하여 보고, 명령에 추가도 해보았지만 무소식이였다.

약 30분가량 찾아본 결과 액세스/시크릿 키가 잘못되었다는것을 깨달았다.

저 aws configure 로 만드는것이 아닌 확인하는 용도였다.

액세스/시크릿 키는 AWS 홈페이지에서 계정 보안 자격 증명에 생성할 수 있다.

생성된 코드들을 적어주면 되는것이였다…….

![Untitled](CloudWatch%20(AWS%20%E1%84%85%E1%85%B5%E1%84%89%E1%85%A9%E1%84%89%E1%85%B3%20%E1%84%89%E1%85%A1%E1%86%BC%E1%84%90%E1%85%A2%20%E1%84%86%E1%85%A9%E1%84%82%E1%85%B5%E1%84%90%E1%85%A5%E1%84%85%E1%85%B5%E1%86%BC)%203713f6eada7a4b36b738ec66d4e04b3e/Untitled%2013.png)

> 새로 액세스,시크릿 키를 입력하였다.
> 

![Untitled](CloudWatch%20(AWS%20%E1%84%85%E1%85%B5%E1%84%89%E1%85%A9%E1%84%89%E1%85%B3%20%E1%84%89%E1%85%A1%E1%86%BC%E1%84%90%E1%85%A2%20%E1%84%86%E1%85%A9%E1%84%82%E1%85%B5%E1%84%90%E1%85%A5%E1%84%85%E1%85%B5%E1%86%BC)%203713f6eada7a4b36b738ec66d4e04b3e/Untitled%2014.png)

> 이후 명령어를 실행하니 리전을 명령어에 첨부하지 않아도 너무너무 잘 진행되는 모습…
~~(내 30분…)~~
> 

![Untitled](CloudWatch%20(AWS%20%E1%84%85%E1%85%B5%E1%84%89%E1%85%A9%E1%84%89%E1%85%B3%20%E1%84%89%E1%85%A1%E1%86%BC%E1%84%90%E1%85%A2%20%E1%84%86%E1%85%A9%E1%84%82%E1%85%B5%E1%84%90%E1%85%A5%E1%84%85%E1%85%B5%E1%86%BC)%203713f6eada7a4b36b738ec66d4e04b3e/Untitled%2015.png)

> CloudWatch 페이지에서도 Custom namespaces에 추가된것을 확인 가능하다!!!!!
> 

![Untitled](CloudWatch%20(AWS%20%E1%84%85%E1%85%B5%E1%84%89%E1%85%A9%E1%84%89%E1%85%B3%20%E1%84%89%E1%85%A1%E1%86%BC%E1%84%90%E1%85%A2%20%E1%84%86%E1%85%A9%E1%84%82%E1%85%B5%E1%84%90%E1%85%A5%E1%84%85%E1%85%B5%E1%86%BC)%203713f6eada7a4b36b738ec66d4e04b3e/Untitled%2016.png)

> 방금 생성한 네임 스페이스인 CW를 선택하면 측정치 가 표시된것을 확인할 수 있다.
> 

실습에서는 이 명령을 손으로 직접 입력하였지만 

실무에서 활용하는 방법은 다음과 같다.

- Linux의 cron에 등록하여 5분 혹은 1분마다 aws cloudwatch 명령 실행
- Windows의 작업 스케쥴러에 등록하여 5분, 혹은 1분마다 awscloudwatch 명령 실행
- Node.js 에서 chlid_process.exec 를 사용해서 aws cloudwatch 명령 실행 또는 
AWS JavaScriot SDK 활용
- 기타 어떠한 방법이든 aws cloudwatch 명령을 실행하면 된다.

이제 Custom Metric 을 이용하여 이메일 알림(Notification), 자동 횡적 확장(Auto Scaling) 등의 액션을 사용할 수 있다.

<aside>
💡 **AWS CLI CloudWatch**

AWS CLI CloudWatch에 대한 더 자세한 내용은 링크를 참조하기 바랍니다.

[http://docs.aws.amazon.com/cli/latest/reference/cloudwatch/index.html](http://docs.aws.amazon.com/cli/latest/reference/cloudwatch/index.html)

</aside>

---