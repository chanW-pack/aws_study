# AWS S3 기타 설정

# **S3 버킷 기타 설정**

---

S3 버킷은 앞서 설명한 것 이외에도 다양한 설정이 가능하다. 아래 설정들은 간략한 설명으로 대신하겠다.

- Notifications: 낮은 중복 스토리지(Reduced Redundancy Storage)를 사용하는 객체가 유실되었을 때 SNS(Simple Notification Service)로 알림을 전송해주는 기능이다. </br>
- Tags: 버킷에 태그를 설정한다. AWS의 Cost Allocation Reports에서 S3의 비용 내역을 볼 수 있으며 이 보고서에는 여기서 설정한 태그 별로 집계된 사용 내역 및 비용이 표시된다. </br>
- Requester Pays: 기본적으로 S3 버킷 소유자는 사용 공간 및 데이터 전송량에 따른 요금을 지불하게 된다. </br> 
이 설정을 사용하면 버킷 소유자는 사용 공간에 대해서만 요금을 지불하면 되고, </br>데이터 전송량에 따른 요금은 데이터를 다운로드한 사용자가 지불하게 된다. </br>
이 설정을 사용하면 버킷에 대한 익명 접근은 모두 차단된다

---
