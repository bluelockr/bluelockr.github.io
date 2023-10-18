+++
title = "NAT 인스턴스로 VPC 간 연결하기"
summary = "VPC Connection using NAT Instance"
date = 2023-09-29T00:00:00+00:00
cover = ""
slug = "vpc_connection_using_nat_instance"
tags = ['Network', 'AWS']
draft = false
+++
[https://docs.aws.amazon.com/vpc/latest/userguide/extend-intro.html](https://docs.aws.amazon.com/vpc/latest/userguide/extend-intro.html)

오늘은 NAT 인스턴스를 사용해 프라이빗 서브넷에 있는 인스턴스에서 외부 인터넷과 통신해보도록 하겠습니다.

[https://docs.aws.amazon.com/vpc/latest/userguide/VPC_NAT_Instance.html](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_NAT_Instance.html)

아마존에서는 NAT 게이트웨이를 사용할 걸 권장하고 있지만,  
NAT 인스턴스는 별도로 요금이 들지 않아 공부하는 입장이라면 NAT 게이트웨이 대신 NAT 인스턴스를 사용하는 것도 방법입니다.  

저는 NAT 게이트웨이 생성해서 사용하는 게 더 간편해서 그냥 NAT 게이트웨이를 사용합니다만 이런 게 있다는 걸 알아서 나쁠 건 없으니까요.

## ⭐VPC 생성

VPC > VPC > VPC 생성
- 생성할 리소스 : VPC 등
- 이름 태그 자동 생성 : vpc-kr
- IPv4 CIDR 블록 : 10.0.0.0/16
- 가용 영역(AZ) 수 : 1
- 퍼블릭 서브넷 수 : 1
- 프라이빗 서브넷 수 : 1
- NAT 게이트웨이($) : 없음
- VPC 엔드포인트 : 없음

이렇게 하면 퍼블릭 서브넷과 프라이빗 서브넷이 자동으로 1개씩 생성됩니다.  
VPC > 서브넷 으로 가서 vpc-kr 로 시작하는 서브넷의 IPv4 CIDR을 확인합니다.

저는 다음과 같았습니다.  
- vpc-kr-subnet-public1-ap-northeast-2a : 10.0.0.0/20
- vpc-kr-subnet-private1-ap-northeast-2a : 10.0.128.0/20

## ⭐EC2 인스턴스 생성

### EC2 인스턴스 생성

EC2 > 인스턴스 > 인스턴스 시작
<ul>
  <li>이름 : nat-instance</li>
  <li>Quick Start : Amazon Linux</li>
  <li>키 페어(로그인) : 새 키 페어 생성</li>
    <ul>
        <li>키 페어 이름 : mykeypair</li>
        <li>프라이빗 키 파일 형식 : *.pem</li>
    </ul>
  <li>네트워크 설정 > 편집</li>
    <ul>
      <li>VPC : vpc-kr-vpc</li>
      <li>서브넷 : vpc-kr-subnet-public1으로 시작하는 서브넷</li>
      <li>퍼블릭 IP 자동 할당 : 활성화</li>
      <li>방화벽(보안 그룹) : 보안 그룹 생성(보안 그룹 이름: sec-kr)</li>
    </ul>
  </li>
</ul>

## ⭐보안 그룹 편집

VPC > 보안 그룹 > (sec-kr 체크) > 작업 > 인바운드 규칙 편집 > 규칙 추가  
-유형 : HTTP 소스 10.0.128.0/20</li>  
-유형 : HTTPS, 소스 10.0.128.0/20</li>  
-유형 : SSH, 소스 0.0.0.0/0</li>  
-모든 ICMP-IPv4, 소스 0.0.0.0/0</li>

VPC > 보안 그룹 > (sec-kr 체크) > 작업 > 아웃바운드 규칙 편집 > 규칙 추가  
-유형 : HTTP 목적 10.0.128.0/20</li>  
-유형 : HTTPS, 목적 10.0.128.0/20</li>  
-유형 : SSH, 목적 0.0.0.0/0</li>  
-모든 ICMP-IPv4, 목적 0.0.0.0/0</li>

저는 어차피 그냥 실습 목적이고 바로 삭제해줄거라서 이렇게 했지만 여러분은 이러시면 안됩니다 ㅇㅅㅇ;;

## ⭐NAT 인스턴스 설정

EC2 > 인스턴스 > (nat-instance 체크) > 연결 > SSH 클라이언트
- 맨 밑에 예 :   
ssh -i "mykeypair.pem" ec2-user@xxxxxxxx 형식으로 되어있는 명령어 복사

이제 *pem 키 파일을 다운받은 곳으로 이동하여 해당 폴더에서 터미널을 열어주고 복붙한 명령어를 실행합니다.

인스턴스 SSH 접속에 성공했으면 다음과 같이 명령어를 실행합니다.

```bash
sudo yum install iptables-services -y
sudo systemctl enable iptables
sudo systemctl start iptables

sudo vi /etc/sysctl.d/custom-ip-forwarding.conf
#구성 파일에 다음 줄 추가 후 저장
net.ipv4.ip_forward=1

sudo sysctl -p /etc/sysctl.d/custom-ip-forwarding.conf

sudo iptables -t nat -A POSTROUTING -j MASQUERADE
sudo /sbin/iptables -F FORWARD
sudo service iptables save
```

진짜 다 해봐도 안 되다가 이거 보고 해결했습니다.  
[https://serverfault.com/questions/406351/how-to-configure-a-custom-nat-for-use-in-amazon-vpc/406508#406508](https://serverfault.com/questions/406351/how-to-configure-a-custom-nat-for-use-in-amazon-vpc/406508#406508)

아마존 문서에서 써 있던 /sbin/iptables 어쩌고가 문제였던가 봅니다.

아마존 공식 문서에서는 AMI를 만들라고 하지만 저는 스냅샷에 들어가는 비용이 싫어서 그냥 진행하겠습니다.  

### 소스 변경/확인

EC2 > 인스턴스 > (nat-instance 체크) > 작업 > 네트워킹 > 소스/대상 확인 변경
- 소스/대상 확인 : 중지

## ⭐네트워크 관련 설정

### 프라이빗 서브넷 라우팅 테이블 업데이트

먼저 nat-instance의 인스턴스 ID를 복사해놔야 합니다.  
(EC2 > 인스턴스에 가보면 나와있습니다.)

VPC > 라우팅 테이블 > (vpc-kr-rtb-private1 라우팅 테이블 선택) > 작업 > 라우팅 편집
- 대상(왼쪽) : 0.0.0.0/0, 대상(오른쪽) : 아까 복사한 인스턴스 ID

## ⭐프라이빗 서브넷 인스턴스 작업

### 프라이빗 서브넷에 EC2 인스턴스 생성

EC2 > 인스턴스 > 인스턴스 시작
<ul>
  <li>이름 : private-instance</li>
  <li>Quick Start : Amazon Linux</li>
  <li>키 페어(로그인) : mykeypair</li>
  <li>네트워크 설정 > 편집</li>
    <ul>
      <li>VPC : vpc-kr-vpc</li>
      <li>서브넷 : vpc-kr-subnet-private1으로 시작하는 서브넷</li>
      <li>퍼블릭 IP 자동 할당 : 비활성화</li>
      <li>방화벽(보안 그룹) : 기존 보안 그룹 선택(sec-kr)</li>
    </ul>
  </li>
</ul>

프라이빗 인스턴스기 떄문에 퍼블릭 IP는 할당하지 않습니다.

## ⭐핑 테스트

### 인터넷-NAT 인스턴스 간 통신

> 인터넷 -> nat instance -> private instance  

위와 같은 구조로 통신이 되게 되는데,  
우선 인터넷과 NAT Instance 간에 통신이 되는지 확인하겠습니다.

EC2 > 인스턴스 > (nat-instance 체크) > 연결 > SSH 클라이언트
- 맨 밑에 예 : ssh -i "mykeypair.pem" ec2-user@xxxxxxxx 형식으로 되어있는 명령어 복사

이제 *pem 키 파일을 다운받은 곳으로 이동하여 해당 폴더에서 터미널을 열어주고 복붙한 명령어를 실행합니다.

nat-instance에 SSH 접속이 성공하면 핑 테스트를 진행합니다.

> ping 8.8.8.8

핑이 가면 연결 성공입니다.

### NAT 인스턴스-Private 인스턴스 간 통신

이제 nat-instance에서 private-instance로 ssh 접속을 해야 합니다.  
그러러면 먼저 윈도우에 있는 키를 nat-instance로 복사해야 합니다.

EC2 > 인스턴스 > (nat-instance 체크) > 작업 > 세부 정보 보기
- 퍼블릭 IPv4 DNS : (여기 주소를 복사합니다.)

이제 윈도우에서 pem 키가 있는 폴더로 가서 우클릭 후 터미널을 연 뒤,  
다음과 같은 형식의 명령어를 본인에게 맞게 수정하여 입력합니다.

```bash
scp -i mykeypair.pem -r mykeypair.pem ec2-user@ec2-xx-xxx-xxx-xxx.ap-northeast-2.compute.amazonaws.com:mykeypair.pem
```

이제 nat-instance 쪽으로 가서 home/ec2-user 디렉터리에서 ls 명령어를 쳐보면 mykeypair.pem 파일이 있는 것을 보실 수 있습니다.

여기에 권한을 설정해줍니다.

```bash
chmod 400 mykeypair.pem
```

EC2 > 인스턴스 > (private-instance 체크) > 연결
- (다음과 같은 형식의 명령어를 본인에게 맞게 수정하여 실행)

```bash
ssh -i "mykeypair.pem" ec2-user@xx.x.xxx.xxx
```

이렇게 되면  
윈도우 -> SSH 연결 -> NAT Instance -> SSH 연결 -> 프라이빗 서브넷 내 인스턴스  
이렇게 연결한 것입니다.

ping 8.8.8.8을 입력하여 핑이 제대로 가면 모든 설정을 다 완료한 겁니다.
