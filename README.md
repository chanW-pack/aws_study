# Amazon Web Service
<aside>
😁 [당신이 지금 알아야할 AWS] 도서를 참조해 공부한 내용입니다. 감사합니다.
</aside>

<chanwoo pack .2022>

## Week 1

- [AWS 가상 머신 시작](https://github.com/chanW-pack/aws_study/blob/main/Week%201/1_1%20Linux%20%EA%B0%80%EC%83%81%EB%A8%B8%EC%8B%A0%20%EC%8B%9C%EC%9E%91.md)
  - 리전 설정, 인스턴스 네임/태그 생성, OS 이미지 지정, 인스턴스 유형 설정, 키 페어 생성, 네트워크 설정, 스토리지 구성 등 
- [Amazon EC2와 Amazon Lightsail의 차이점](https://github.com/chanW-pack/aws_study/blob/main/Week%201/1_2%20Amazon%20EC2%EC%99%80%20Amazon%20Lightsail%EC%9D%98%20%EC%B0%A8%EC%9D%B4%EC%A0%90.md) 
  - 성능, 편의성, 확장성, 요금 등 주요 차이점
- [Amazon Linux 인스턴스에서 사용자 계정 관리](https://github.com/chanW-pack/aws_study/blob/main/Week%201/1_3%20Amazon%20Linux%20%EC%9D%B8%EC%8A%A4%ED%84%B4%EC%8A%A4%EC%97%90%EC%84%9C%20%EC%82%AC%EC%9A%A9%EC%9E%90%20%EA%B3%84%EC%A0%95%20%EA%B4%80%EB%A6%AC.md)
  - 각 Linux 인스턴스 기본 사용자
- [Linux 기본 개념](https://github.com/chanW-pack/aws_study/blob/main/Week%201/1_4%20AWS%20%EA%B8%B0%EB%B3%B8%EA%B0%9C%EB%85%90.md) 
  - 리전, 가용영역, 엣지 로케이션

## Week 2

- [가상 스토리지를 제공하는 EBS](https://github.com/chanW-pack/aws_study/blob/main/Week%202/2_1%20%EA%B0%80%EC%83%81%20%EC%8A%A4%ED%86%A0%EB%A6%AC%EC%A7%80%EB%A5%BC%20%EC%A0%9C%EA%B3%B5%ED%95%98%EB%8A%94%20EBS.md)
	- EBS 기본 개념, EBS 볼륨 생성 및 장착, EBS 볼륨 포맷, 마운트, 제거
- [EBS 스냅샷 활용하기](https://github.com/chanW-pack/aws_study/blob/main/Week%202/2_2%20EBS%20%EC%8A%A4%EB%83%85%EC%83%B7%20%ED%99%9C%EC%9A%A9%ED%95%98%EA%B8%B0.md)
	- EBS 스냅샷 생성 및 스냅샷으로 EBS 볼륨/AMI 생성, EBS 스냅샷을 다른 리전으로 복사

## Week 3

- [Security Group으로 방화벽 설정](https://github.com/chanW-pack/aws_study/blob/main/Week%203/3_1%20Security%20Group%EC%9C%BC%EB%A1%9C%20%EB%B0%A9%ED%99%94%EB%B2%BD%20%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0.md) 
  - 방화벽 설정의 기본요소
  - Inbound Outbound 규칙, CIDR 표기법	
- [고정 IP를 제공하는 Elastic IP](https://github.com/chanW-pack/aws_study/blob/main/Week%203/3_2%20%EA%B3%A0%EC%A0%95%20IP%EB%A5%BC%20%EC%A0%9C%EA%B3%B5%ED%95%98%EB%8A%94%20Elastic%20IP.md)
	- Elastic IP 개념, Elastic IP 할당 및 연결 
- [EC2 인스턴스 접속을 위한 키 쌍](https://github.com/chanW-pack/aws_study/blob/main/Week%203/3_3%20EC2%20%EC%9D%B8%EC%8A%A4%ED%84%B4%EC%8A%A4%20%EC%A0%91%EC%86%8D%EC%9D%84%20%EC%9C%84%ED%95%9C%20%ED%82%A4%20%EC%8C%8D.md)
	- 키 페어 생성, EC2 - SSH, Putty 
- [AWS AMI](https://github.com/chanW-pack/aws_study/blob/main/Week%203/3_4%20AWS%20AMI%20(%EC%83%9D%EC%84%B1%20%EB%B0%8F%20%EC%9D%B4%EC%9A%A9%ED%95%98%EC%97%AC%20%EC%9D%B8%EC%8A%A4%ED%84%B4%EC%8A%A4%20%EC%83%9D%EC%84%B1).md)
	- 생성 및 이용하여 인스턴스 생성, AWS Marketplace, 다른 리전으로 복사.
- [CloudWatch](https://github.com/chanW-pack/aws_study/blob/main/Week%203/3_5%20CloudWatch%20(AWS%20%EB%A6%AC%EC%86%8C%EC%8A%A4%20%EC%83%81%ED%83%9C%20%EB%AA%A8%EB%8B%88%ED%84%B0%EB%A7%81).md)
	- AWS 리소스 상태 모니터링
	
## AWS S3 (Simple Storage Service)
- [AWS S3 개념 및 특징, 생성](https://github.com/chanW-pack/aws_study/blob/main/AWS%20S3/1_%20AWS%20S3(Simple%20Storage%20Service).md)
- [AWS S3 공유 및 권한](https://github.com/chanW-pack/aws_study/blob/main/AWS%20S3/2_%20AWS%20S3%20%EA%B3%B5%EC%9C%A0%20%EB%B0%8F%20%EA%B6%8C%ED%95%9C.md)
- [AWS S3 정적 웹사이트 호스팅](https://github.com/chanW-pack/aws_study/blob/main/AWS%20S3/3_%20AWS%20S3%20%EC%8B%AC%ED%99%94%20(%EC%A0%95%EC%A0%81%20%EC%9B%B9%EC%82%AC%EC%9D%B4%ED%8A%B8%20%ED%98%B8%EC%8A%A4%ED%8C%85).md)
- [AWS S3 HTTP Referer로 S3 접근 제한](https://github.com/chanW-pack/aws_study/blob/main/AWS%20S3/4_%20AWS%20S3%20%EC%8B%AC%ED%99%94%20(HTTP%20Referer%EB%A1%9C%20S3%20%EC%A0%91%EA%B7%BC%20%EC%A0%9C%ED%95%9C).md)
- [AWS S3 객체 스토리지 클래스, 암호화 설정](https://github.com/chanW-pack/aws_study/blob/main/AWS%20S3/5_%20AWS%20S3%20%EA%B0%9D%EC%B2%B4%20%EC%8A%A4%ED%86%A0%EB%A6%AC%EC%A7%80%20%ED%81%B4%EB%9E%98%EC%8A%A4%2C%20%EC%95%94%ED%98%B8%ED%99%94%20%EC%84%A4%EC%A0%95.md)
- [AWS S3 객체 메타데이터 설정](https://github.com/chanW-pack/aws_study/blob/main/AWS%20S3/6_%20AWS%20S3%20%EA%B0%9D%EC%B2%B4%20%EB%A9%94%ED%83%80%EB%8D%B0%EC%9D%B4%ED%84%B0%20%EC%84%A4%EC%A0%95.md)
- [AWS S3 버킷 로그 설정](https://github.com/chanW-pack/aws_study/blob/main/AWS%20S3/7_%20AWS%20S3%20%EB%B2%84%ED%82%B7%20%EB%A1%9C%EA%B7%B8%20%EC%84%A4%EC%A0%95.md)
- [AWS S3 버킷 버저닝, 수명 주기 설정](https://github.com/chanW-pack/aws_study/blob/main/AWS%20S3/8_%20AWS%20S3%20%EB%B2%84%ED%82%B7%20%EB%B2%84%EC%A0%80%EB%8B%9D%2C%20%EC%88%98%EB%AA%85%20%EC%A3%BC%EA%B8%B0%20%EC%84%A4%EC%A0%95.md)
- [AWS S3 기타 설정](https://github.com/chanW-pack/aws_study/blob/main/AWS%20S3/9_%20AWS%20S3%20%EA%B8%B0%ED%83%80%20%EC%84%A4%EC%A0%95.md)

## AWS CDN CloudFront
- [AWS CloudFront 개념 및 특징](https://github.com/chanW-pack/aws_study/blob/main/AWS%20CloudFront/1_%20AWS%20CDN%20CloudFront.md)
- [AWS CloudFront 실전 구축 세팅 (EC2 연동)](https://github.com/chanW-pack/aws_study/blob/main/AWS%20CloudFront/2_%20AWS%20CloudFront%20%EC%8B%A4%EC%A0%84%20%EA%B5%AC%EC%B6%95%20%EC%84%B8%ED%8C%85%20(EC2%20%EC%97%B0%EB%8F%99).md)
- [AWS CloudFront 캐싱 확인, 무효화 생성, 삭제](https://github.com/chanW-pack/aws_study/blob/main/AWS%20CloudFront/3_%20AWS%20CloudFront%20%EC%BA%90%EC%8B%B1%20%ED%99%95%EC%9D%B8%2C%20%EB%AC%B4%ED%9A%A8%ED%99%94%20%EC%83%9D%EC%84%B1%2C%20%EC%82%AD%EC%A0%9C.md)
- [AWS CloudFront Signed URL 개념 + URL 구조](https://github.com/chanW-pack/aws_study/blob/main/AWS%20CloudFront/4_%20AWS%20CloudFront%20Signed%20URL%20%EA%B0%9C%EB%85%90%20%2B%20URL%20%EA%B5%AC%EC%A1%B0.md)
- [AWS CloudFront Signed URL 사용 설정](https://github.com/chanW-pack/aws_study/blob/main/AWS%20CloudFront/5_%20AWS%20CloudFront%20Signed%20URL%20%EC%82%AC%EC%9A%A9%20%EC%84%A4%EC%A0%95.md)
- [AWS Canned, Custom Policy 사용하여 Signed URL 생성](https://github.com/chanW-pack/aws_study/blob/main/AWS%20CloudFront/6_%20AWS%20Canned%2C%20Custom%20Policy%20%EC%82%AC%EC%9A%A9%ED%95%98%EC%97%AC%20Signed%20URL%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.md)

## AWS RDS (Relational Database Service)
- [AWS RDS 개념 및 특징](https://github.com/chanW-pack/aws_study/blob/main/AWS%20RDS/1_%20AWS%20RDS%20%EA%B0%9C%EB%85%90.md)
- [AWS RDS 구성 인프라 아키텍쳐](https://github.com/chanW-pack/aws_study/blob/main/AWS%20RDS/2_%20AWS%20RDS%20%EC%95%84%ED%82%A4%ED%85%8D%EC%B3%90.md)

