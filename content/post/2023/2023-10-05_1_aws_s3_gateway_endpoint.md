---
title: AWS S3 게이트웨이 엔드포인트 설정하기
description: How to set up S3 Gateway Endpoint
slug: aws_s3_gateway_endpoint_configuration
date: 2023-10-05 00:00:00+0000
image: 
tags:
    - Network
    - AWS
published: true
---

[https://tech.cloud.nongshim.co.kr/2023/03/16/%EC%86%8C%EA%B0%9C-vpc-endpoint%EB%9E%80/](https://tech.cloud.nongshim.co.kr/2023/03/16/%EC%86%8C%EA%B0%9C-vpc-endpoint%EB%9E%80/)
[https://www.youtube.com/watch?v=He1DL3WiL_E](https://www.youtube.com/watch?v=He1DL3WiL_E)

## ⭐IAM 사용자 생성

IAM > 사용자 > 사용자 생성
- 사용자 이름 : student
- AWS Management Console에 대한 사용자 액세스 권한 제공 : 체크
- IAM 사용자를 생성하고 싶음 : 선택
- 콘솔 암호 : 자동 생성된 암호
- 사용자는 다음 로그인 시 새 암호를 생성해야 합니다 : 체크
- 권한 옵션 : 직접 정책 연결
- 권한 정책 : AmazonS3FullAccess
- .csv 파일 다운로드

IAM > 사용자 > (student 클릭) > 보안 자격 증명 > MFA 디바이스 할당
- 디바이스 이름 : app

IAM > 사용자 > (student 클릭) > 보안 자격 증명 > 액세스 키 만들기
- 사용 사례 : Command Line Interface(CLI)
- 위의 권장 사항을 이해했으며 액세스 키 생성을 계속하려고 합니다. : 체크
- .csv 파일 다운로드

## ⭐VPC 생성

VPC > VPC > VPC 생성
- VPC 등
- 이름 태그 : practice (practice-vpc로 자동 설정됨)
- IPv4 CIDR 블록 : 10.0.0.0/16
- 가용 영역(AZ) 수 : 1
- 퍼블릭 서브넷 수 : 0
- 프라이빗 서브넷 수 : 1
- NAT 게이트웨이($)  없음
- VPC 엔드포인트 : 없음

이 과정을 통해 생성되는 건 다음과 같습니다.
- VPC
- 프라이빗 서브넷 1개
- 프라이빗 라우팅 테이블 1개
- 보안 그룹 1개

## ⭐프라이빗 서브넷 인스턴스 생성

EC2 > 인스턴스 > 인스턴스 시작
<ul>
  <li>이름 : private-server<li>
  <li>Quick Start : Amazon Linux</li>
  <li>키 페어 : (설정 안 함)</li>
  <li>네트워크 설정 > 편집</li>
    <ul>
      <li>vpc : practice-vpc</li>
      <li>서브넷 : practice-subnet-private어쩌고</li>
      <li>퍼블릭 IP 자동할당 : 비활성화</li>
      <li>보안 그룹 생성</li>
        <ul>
          <li>보안 그룹 이름: practice-sec</li>
          <li>인바운드 보안 그룹 규칙</li>
            <ul>
              <li>SSH, 소스 : 0.0.0.0/0</li>
            </ul>
        </ul>
    </ul>
    <li>키 페어 없이 계속 진행</li>
</ul>

## ⭐VPC S3 Gateway Endpoint 생성

VPC > 엔드포인트 > 엔드포인트 생성
- 이름 태그 : s3-gateway-endpoint
- 서비스 범주 : AWS 서비스
- 서비스 : com.amazonaws.ap-northeast-2.s3 (유형 : Gateway)
- VPC : practice-vpc
- 라우팅 테이블 : practice-rtb-private어쩌고 체크
- 정책 : 전체 액세스

## ⭐S3 버킷 만들기

S3 > 버킷 > 버킷 만들기
- 버킷 이름 : (사용자 맘대로. 다른 사용자의 버킷명과 중복 불허)
- 객체 소유권 : ACL 비활성화됨(권장)
- AWS 리전 : 아시아 태평양(서울)
- ACL 비활성화됨
- 모든 퍼블릭 액세스 차단 : 체크
- 버킷 버전 관리 : 비활성화
- 암호화 유형 : Amazon S3 관리형 키(SSE-S3)를 사용한 서버측 암호화
- 버킷 키 : 비활성화

위에서 설정했다시피 퍼블릭 액세스가 차단당한 상황입니다.  
이런 상황에서 과연 파일을 업로드할 수 있을까요?

## ⭐S3 버킷에 인스턴스 내의 파일 업로드

### EC2 Instance Connect Endpoint 설정

VPC > 엔드포인트 > 엔드포인트 생성
- 이름 태그 : ec2-conn-endpoint
- 서비스 범주 : EC2 인스턴스 연결 엔드포인트
- VPC : practice-vpc
- 보안 그룹 : practice-sec
- 서브넷 : practice-subnet-private어쩌고

### 프라이빗 서브넷 인스턴스 접속

EC2 > 인스턴스 > (private-server 체크) > 연결 > EC2 인스턴스 연결
- 연결 유형 : EC2 인스턴스 연결 엔드포인트를 사용하여 연결
- 사용자 이름 : ec2-user
- EC2 인스턴스 연결 엔드포인트 : ec2-conn-endpoint

참고로 EC2 인스턴스 연결 엔드포인트를 생성하기까지는 몇 분 정도 걸립니다.

### S3에 파일 업로드

```bash
aws configure
(액세스 키 ID 입력)
(시크릿 액세스 키 입력)
Default region name : ap-northeast-2
Default output format : json

aws s3 ls
```
액세스 키와 스크릿 액세스 키를 입력할 때는 ctrl+shift+v를 사용하시면 편합니다.

정상적으로 조회되면 다음 명령어를 통해 s3 버킷에 파일을 업로드 해봅니다.

```bash
touch test
aws s3 cp test s3://버킷명
```
위에서 버킷명에는 아까 생성해준 버킷 이름을 집어넣으면 됩니다.  
파일이 제대로 업로드되었는지 확인해보려면 다음 경로로 가보시면 됩니다.

s3 > 버킷 > (버킷명 클릭) > 객체

S3 버킷의 퍼블릭 액세스를 차단했고,  
NAT게이트웨이와 인터넷 게이트웨이가 없어 외부 인터넷에 ping이 가지 않음에도 S3 버킷에 파일이 업로드 되었습니다.