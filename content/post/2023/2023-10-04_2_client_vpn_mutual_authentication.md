+++
title = "AWS Client VPN 상호 인증"
summary = "AWS Client VPN Mutual Authentication"
date = 2023-10-04T00:00:00+00:00
cover = ""
slug = "aws_client_vpn_mutual_authentication"
tags = ['Network', 'AWS']
draft = false
+++
[https://www.youtube.com/watch?v=spBZkk9fuxg](https://www.youtube.com/watch?v=spBZkk9fuxg)  
[https://1mini2.tistory.com/134](https://1mini2.tistory.com/134)

## ⭐IAM 사용자 생성

IAM > 사용자 > 사용자 생성
- 사용자 이름 : student
- AWS Management Console에 대한 사용자 액세스 권한 제공 : 체크
- IAM 사용자를 생성하고 싶음 : 선택
- 콘솔 암호 : 자동 생성된 암호
- 사용자는 다음 로그인 시 새 암호를 생성해야 합니다 : 체크
- 권한 옵션 : 직접 정책 연결
- 권한 정책 : AWSCertificateManagerFullAccess
- .csv 파일 다운로드

IAM > 사용자 > (student 클릭) > 보안 자격 증명 > MFA 디바이스 할당
- 디바이스 이름 : app

IAM > 사용자 > (student 클릭) > 보안 자격 증명 > 액세스 키 만들기
- 사용 사례 : Command Line Interface(CLI)
- 위의 권장 사항을 이해했으며 액세스 키 생성을 계속하려고 합니다. : 체크
- .csv 파일 다운로드

## ⭐VPC 생성

VPC > VPC > VPC 생성
- 생성할 리소스 : VPC 등
- 이름 태그 : client-vpn-vpc (client-vpn까지만 입력하면 -vpc가 뒤에 자동으로 붙음)
- IPv4 CIDR : 10.0.0.0/16
- 가용 영역(AZ) 수 : 1
- 퍼블릭 서브넷 수 : 0
- 프라이빗 서브넷 수 : 1
- NAT 게이트웨이 : 없음
- VPC 엔드포인트 : 없음

## ⭐프라이빗 서브넷 인스턴스 생성 및 연결

EC2 > 인스턴스 > 인스턴스 시작
<ul>
  <li>이름 : private-server<li>
  <li>Quick Start : Amazon Linux 2023</li>
  <li>키 페어 이름 : vpn-key</li>
  <li>네트워크 설정 > 편집</li>
    <ul>
      <li>vpc : client-vpn-vpc</li>
      <li>서브넷 : client-vpn-subnet-private어쩌고</li>
      <li>퍼블릭 IP 자동할당 : 비활성화</li>
      <li>보안 그룹 : vpn-sec</li>
          <li>인바운드 보안 그룹 규칙</li>
            <ul>
              <li>SSH, 소스 : 0.0.0.0/0</li>
            </ul>
        </ul>
    </ul>
</ul>

## ⭐AWS CLI 설치 및 설정

[https://docs.aws.amazon.com/ko_kr/cli/latest/userguide/getting-started-install.html](https://docs.aws.amazon.com/ko_kr/cli/latest/userguide/getting-started-install.html)

위 링크에서 설치 파일(*.msi) 다운받아 설치 후 윈도우 터미널을 열고 다음 명령어 실행

```bash
aws configure
(액세스 ID 입력)
(액세스 시크릿 키 입력)
Default region name : ap-northeast-2
Default output format : json
```

## ⭐EasyRSA 다운로드 및 ACM 설정

[https://docs.aws.amazon.com/ko_kr/vpn/latest/clientvpn-admin/mutual.html](https://docs.aws.amazon.com/ko_kr/vpn/latest/clientvpn-admin/mutual.html)

[https://github.com/OpenVPN/easy-rsa/releases](https://github.com/OpenVPN/easy-rsa/releases)

바로 윗 링크에서 easy-rsa zip 파일을 다운로드합니다.  
저는 윈도우 64비트이므로 EasyRSA-3.1.6-win64.zip 파일을 다운받아 압축해제해줬습니다.  
(반디집의 알아서 풀기 기능을 이용하면 더더욱 좋습니다.)  
다운로드 경로는 기본적으로 '다운로드' 폴더 안입니다.

다운받은 폴더 안쪽으로 들어간 뒤 우클릭해 터미널을 열고 다음 명령어를 실행합니다.

한 줄씩 차근차근 실행해줍니다.

```bash
.\EasyRSA-Start.bat

./easyrsa init-pki
./easyrsa build-ca nopass
(엔터)
./easyrsa build-server-full server nopass
yes
./easyrsa build-client-full client1.domain.tld nopass
yes
exit

mkdir C:\easyrsa
copy pki\ca.crt C:\easyrsa
copy pki\issued\server.crt C:\easyrsa
copy pki\private\server.key C:\easyrsa
copy pki\issued\client1.domain.tld.crt C:\easyrsa
copy pki\private\client1.domain.tld.key C:\easyrsa
cd C:\easyrsa

aws acm import-certificate --certificate fileb://server.crt --private-key fileb://server.key --certificate-chain fileb://ca.crt

aws acm import-certificate --certificate fileb://client1.domain.tld.crt --private-key fileb://client1.domain.tld.key --certificate-chain fileb://ca.crt
```

## ⭐Client VPN 엔드포인트 설정

VPC > Client VPN 엔드포인트 > 클라이언트 VPN 엔드포인트 생성
- 이름 태그 : cvpn-endpoint
- 클라이언트 IPv4 CIDR : 172.0.0.0/22 (VPC CIDR 대역과 겹치지 않아야 함)
- 서버 인증서 ARN : (아까 생성한 ACM server 인증서)
- 상호 인증 사용 : 체크
- 클라이언트 인증서 ARN : (아까 생성한 ACM client 인증서)
<ul>
  <li>기타 파라미터</li>
    <ul>
      <li>DNS 서버 활성화 : 켜기</li>
      <li>전송 프로토콜 : UDP</li>
      <li>분할 터널 활성화 : 켜기</li>
      <li>VPC ID : client-vpn-vpc</li>
      <li>보안 그룹 ID : vpn-sec</li>
      <li>VPN 포트 : 443</li>
    </ul>
</ul>

**분할 터널 활성화를 반드시 해주셔야 합니다!**

VPC > Client VPN 엔드포인트 > 대상 네트워크 연결> 대상 네트워크 연결
- VPC : client-vpn-vpc
- 서브넷 : client-vpn-subnet-private어쩌고

VPC > Client VPN 엔드포인트 > 권한 부여 규칙 > 권한 부여 규칙 추가
- 액세스를 활성화할 대상 네트워크 : 10.0.0.0/16 (클라이언트 VPN 엔드포인트에 연결된 VPC의 CIDR)

VPC > Client VPN 엔드포인트 > 클라이언트 구성 다운로드

다운로드한 클라이언트 구성 파일에 다음과 같은 내용을 추가하고 저장합니다.

```bash
<cert>
(client1.domain.tld.crt 파일의 내용 복붙)
</cert>

<key>
(client1.domain.tld.key 파일의 내용 복붙)
</key>
```

client1.domain.tld.crt나 key 파일의 내용을 복붙할 떄는 아까와 마찬가지로 begin, end절까지 모두 포함시킵니다.

client1.domain.tld.crt나 key 파일은 C드라이브의 easyrsa 폴더 안에 있습니다.

## ⭐AWS Client VPN 설치 및 연결

[https://aws.amazon.com/ko/vpn/client-vpn-vpc-download/](https://aws.amazon.com/ko/vpn/client-vpn-vpc-download/)

위 링크에서 윈도우 64bit 버전용을 다운받아 설치합니다.

AWS VPN Client 프로그램을 실행하고 파일 > 프로필 관리 > 프로필 추가
- 표시 이름 : aws_vpn_client
- VPN 구성 파일 : (아까 전에 다운받은 vpc-client-endpoint 구성 파일)

## ⭐프라이빗 서브넷 EC2 인스턴스 직접 접속

VPN이 활성화 되었으니 이 상태로 프라이빗 인스턴스로 직접 SSH 접속해보겠습니다.

EC2 > 인스턴스 > (private-server 체크) > 연결 > SSH 클라이언트
- 맨 밑에 예 : ssh -i "vpn-key.pem" ec2-user@xxxxxxxx 형식으로 되어있는 명령어 복사

이제 *pem 키 파일을 다운받은 곳으로 이동하여 해당 폴더에서 터미널을 열어주고 복붙한 명령어를 실행합니다.

이렇게 하면 NAT 게이트웨이나 Bastion Host 같은 것 없이도 프라이빗 서브넷에 있는 EC2 인스턴스에 바로 SSH 접속할 수 있습니다.