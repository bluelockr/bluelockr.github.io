---
title: AWS VPC 피어링 연결하기
description: AWS VPC Peering (feat. Cloudshell)
slug: aws_vpc_peering_using_cloudshell
date: 2023-09-28 00:00:00+0000
image: 
tags:
    - Network
    - AWS
published: true
---

[https://docs.aws.amazon.com/vpc/latest/userguide/extend-intro.html](https://docs.aws.amazon.com/vpc/latest/userguide/extend-intro.html)

오늘은 VPC 피어링을 통해 서로 다른 리전에 있는 VPC끼리 통신이 이루어지도록 해보겠습니다.

전체적인 구조는 다음과 같습니다.
- 두 리전 간 VPC 피어링 연결 수립
- 라우팅 테이블 설정(인터넷 게이트웨이 관련, VPC 연결 관련)
- 보안 그룹 설정(80번 포트, ICMPv4 허용)
- 퍼블릭 서브넷 내 EC2 인스턴스의 퍼블릭 IPv4 주소 사용 (EIP 부여하면 좋음)

이런 작업들이 필요합니다.  
저는 아래와 같이 진행해주었습니다.

## ⭐VPC 피어링

### 서울 쪽 VPC/서브넷 설정

VPC > VPC > 생성  
- 생성할 리소스 : vpc만
- 이름 태그 : vpc-kr  
- IPv4 CIDR : 10.0.0.0/16  

VPC > 서브넷 > 서브넷 생성  
- VPC ID : vpc-kr 선택  
- 서브넷 이름 : kr-public-subnet  
- IPv4 subnet CIDR block : 10.0.1.0/24  

VPC > 서브넷 > (kr-public-subnet 서브넷 체크) > 작업 > 서브넷 설정 편집  
- 퍼블릭 IPv4 주소 자동 할당 활성화

VPC > 서브넷 > 서브넷 생성  
- VPC ID : vpc-kr 선택  
- 서브넷 이름 : kr-private-subnet  
- IPv4 subnet CIDR block : 10.0.2.0/24  

### 도쿄 쪽 VPC/서브넷 설정

VPC > VPC > 생성  
- 이름 태그 : vpc-jp  
- IPv4 CIDR : 20.0.0.0/16  

VPC > 서브넷 > 서브넷 생성  
- VPC ID : vpc-jp 선택  
- 서브넷 이름 : jp-public-subnet  
- IPv4 subnet CIDR block : 20.0.1.0/24  

VPC > 서브넷 > (jp-public-subnet 서브넷 체크) > 작업 > 서브넷 설정 편집  
- 퍼블릭 IPv4 주소 자동 할당 활성화

VPC > 서브넷 > 서브넷 생성  
- VPC ID : vpc-jp 선택  
- 서브넷 이름 : jp-private-subnet  
- IPv4 subnet CIDR block : 20.0.2.0/24

### 도쿄 인터넷 게이트웨이 설정

저는 EC2 Connect를 사용하여 인스턴스에 접속해줄 것이기 때문에 인터넷 게이트웨이가 필요합니다.

[참고 링크](https://rainbound.tistory.com/entry/AWS-EC2-SSH-%EB%B0%8F-%EC%97%B0%EA%B2%B0-%ED%95%A0%EB%95%8C-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EC%84%A4%EC%A0%95)

VPC > 인터넷 게이트웨이 > 인터넷 게이트웨이 생성
- 이름 태그 : igw-jp

VPC > 인터넷 게이트웨이 > (jp-igw 체크) > 작업 > VPC에 연결
- vpc-jp 선택

### 서울 인터넷 게이트웨이 설정

VPC > 인터넷 게이트웨이 > 인터넷 게이트웨이 생성
- 이름 태그 : igw-kr

VPC > 인터넷 게이트웨이 > (kr-igw 체크) > 작업 > VPC에 연결
- vpc-kr 선택

### VPC 피어링 요청

어느 쪽에서 요청을 보내도 상관 없지만, 
저는 서울에서 보내는 걸로 해봤습니다.

우선 도쿄 리전에서 vpc-jp의 VPC-ID를 복사해둡니다.  
(VPC 메뉴에서 잘 찾아보면 나옵니다.)

그리고 서울 리전으로 이동합니다.

VPC > 피어링 연결 > 피어링 연결 생성
- 이름 : vpcpeer
- 피어링할 로컬 VPC 선택 : vpc-kr
- 리전 : 다른 리전, 아시아 태평양(도쿄)(ap-northeast-1)
- VPC ID(수락자) : (아까 복사해둔 vpc-jp의 vpc-id 붙여넣기)

다시 도쿄 리전으로 이동합니다.

### VPC 피어링 요청 수락

VPC > 피어링 연결 > 작업 > 요청 수락

VPC 피어링이 이렇게 연결되었습니다.

## ⭐VPC간 라우팅 설정

인터넷 게이트웨이 관련 라우팅, VPC 피어링 라우팅이 필요합니다.

### 도쿄 VPC 라우팅 테이블 설정

VPC > 라우팅 테이블 > (vpc-jp에 연결된 라우팅 테이블 체크) > 작업 > 라우팅 편집
- 대상(왼쪽) : 0.0.0.0/0, 대상(오른쪽) : 인터넷 게이트웨이(jp-igw)   
- 대상(왼쪽) : 10.0.0.0/16, 대상(오른쪽) : 피어링 연결

### 서울 VPC 라우팅 테이블 설정

VPC > 라우팅 테이블 > (vpc-kr에 연결된 라우팅 테이블 체크) > 작업 > 라우팅 편집
- 대상(왼쪽) : 0.0.0.0/0, 대상(오른쪽) : 인터넷 게이트웨이(kr-igw)   
- 대상(왼쪽) : 20.0.0.0/16, 대상(오른쪽) : 피어링 연결  

## ⭐인스턴스 설정

### 서울 EC2 인스턴스 생성

인스턴스 타입은 가급적 프리티어 적용되는 t2.micro로 해주되,  
만일 안 되면 t3.micro도 무방합니다.

EC2 > 인스턴스 > 인스턴스 시작  
- 이름 : kr-public-server
- Quick Start : Amazon Linux
- 인스턴스 유형 : t2.micro
- 키 페어 : (필요 없음)
<li>네트워크 설정 > 편집</li>
    <ul>
        <li>VPC : vpc-kr</li>
        <li>서브넷 : kr-public-subnet</li>
        <li>퍼블릭 IP 자동 할당 : 활성화</li>
        <li>방화벽(보안 그룹) : 보안 그룹 생성</li>
            <ul>
                <li>보안 그룹 이름 : sec-kr</li>
            </ul>
        <li>인바운드 보안 그룹 규칙</li>
            <ul>
                <li>유형 : SSH, 원본 : 0.0.0.0/0</li>
                <li>유형 : 모든 ICMP - IPv4, 원본 : 0.0.0.0/0</li>
            </ul>
    </ul>
<li>키 페어 없이 계속 진행</li>

EC2 > 인스턴스 > 인스턴스 시작  
- 이름 : kr-private-server
- Quick Start : Amazon Linux
- 인스턴스 유형 : t2.micro
- 키 페어 : (필요 없음)
<li>네트워크 설정 > 편집</li>
    <ul>
        <li>VPC : vpc-kr</li>
        <li>서브넷 : kr-private-subnet</li>
        <li>퍼블릭 IP 자동 할당 : 비활성화</li>
        <li>방화벽(보안 그룹) : 기존 보안 그룹</li>
            <ul>
                <li>보안 그룹 이름 : sec-kr</li>
            </ul>
        <li>인바운드 보안 그룹 규칙</li>
            <ul>
                <li>유형 : SSH, 원본 : 0.0.0.0/0</li>
                <li>유형 : 모든 ICMP - IPv4, 원본 : 0.0.0.0/0</li>
            </ul>
    </ul>
<li>키 페어 없이 계속 진행</li>

### 도쿄 EC2 인스턴스 생성

EC2 > 인스턴스 > 인스턴스 시작  
- 이름 : jp-public-server
- Quick Start : Amazon Linux
- 인스턴스 유형 : t2.micro
- 키 페어 : (필요 없음)
<li>네트워크 설정 > 편집</li>
    <ul>
        <li>VPC : vpc-jp</li>
        <li>서브넷 : jp-public-subnet</li>
        <li>퍼블릭 IP 자동 할당 : 활성화</li>
        <li>방화벽(보안 그룹) : 보안 그룹 생성</li>
            <ul>
                <li>보안 그룹 이름 : sec-jp</li>
            </ul>
        <li>인바운드 보안 그룹 규칙</li>
            <ul>
                <li>유형 : SSH, 원본 : 0.0.0.0/0</li>
                <li>유형 : 모든 ICMP - IPv4, 원본 : 0.0.0.0/0</li>
            </ul>
    </ul>
<li>키 페어 없이 계속 진행</li>

EC2 > 인스턴스 > 인스턴스 시작  
- 이름 : jp-private-server
- Quick Start : Amazon Linux
- 인스턴스 유형 : t2.micro
- 키 페어 : (필요 없음)
<li>네트워크 설정 > 편집</li>
    <ul>
        <li>VPC : vpc-jp</li>
        <li>서브넷 : jp-private-subnet</li>
        <li>퍼블릭 IP 자동 할당 : 비활성화</li>
        <li>방화벽(보안 그룹) : 기존 보안 그룹</li>
            <ul>
                <li>보안 그룹 이름 : sec-jp</li>
            </ul>
        <li>인바운드 보안 그룹 규칙</li>
            <ul>
                <li>유형 : SSH, 원본 : 0.0.0.0/0</li>
                <li>유형 : 모든 ICMP - IPv4, 원본 : 0.0.0.0/0</li>
            </ul>
    </ul>
<li>키 페어 없이 계속 진행</li>

## ⭐EC2 인스턴스 연결 엔드포인트 생성

도쿄 리전의 프라이빗 서브넷 인스턴스에 연결하기 위해 다음과 같이 진행합니다.

VPC > 엔드포인트 > 엔드포인트 생성
- 이름 태그 : jp-instance-connect-endpoint
- 서비스 범주 : EC2 인스턴스 연결 엔드포인트
- VPC : vpc-jp
- 보안 그룹 : sec-jp
- 서브넷 : jp-private-subnet

서울 리전에서도 동일하게 해줍니다.

VPC > 엔드포인트 > 엔드포인트 생성
- 이름 태그 : kr-instance-connect-endpoint
- 서비스 범주 : EC2 인스턴스 연결 엔드포인트
- VPC : vpc-kr
- 보안 그룹 : sec-kr
- 서브넷 : kr-private-subnet

## ⭐핑 테스트

**각 리전에 있는 총 4개의 인스턴스의 프라이빗 IP 주소를 따로 적어둡니다.**  
(VPC > 인스턴스 > 인스턴스 ID 클릭하시면 나옵니다.)

이제 EC2 인스턴스 연결 엔드포인트를 사용하여 연결해봅니다.  
저는 먼저 kr-private-server에 연결해보겠습니다.  

EC2 > 인스턴스 > (kr-private-server 체크) > 연결 > EC2 인스턴스 연결
- 연결 유형 : EC2 인스턴스 연결 엔드포인트를 사용하여 연결
- 사용자 이름 : ec2-user
- EC2 인스턴스 연결 엔드포인트 : (목록에 있는 거 선택)

여기까지 다 완료하고 핑테스트를 진행하면 프라이빗 서브넷에서 나머지 3개의 인스턴스로 핑이 가는 걸 볼 수 있습니다.

ping테스트를 해보고 ping이 안 가면 EC2 인스턴스 연결 엔드포인트가 구성되기까지 시간이 걸려서 안 되는 것일 수도 있으므로 라우팅 테이블, 보안 그룹, 피어링 연결 등의 설정을 검토하면서 기다려줍니다.  

NAT 게이트웨이 없이도 통신이 잘 되는 걸 확인하실 수 있습니다.