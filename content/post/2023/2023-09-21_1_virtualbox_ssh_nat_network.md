---
title: Virtualbox 가상머신 안의 Linux에 NAT Network인 상태에서 SSH 접속(일반/RSA)하는 법
description: SSH Connection(Common/RSA) to Virtualbox NAT Network
slug: ssh_to_virtualbox_nat_network_common_rsa
date: 2023-09-21 00:00:00+0000
image: 
tags:
    - Windows
    - Linux
    - Virtualbox
---
## SSH 설치

일단 가상 머신 내에 SSH를 설치해야 합니다.

<span style="color:#dd4814"> **\[Ubuntu의 경우\]** </span>

```bash
sudo apt install openssh-server -y
sudo systemctl start ssh
sudo systemctl enable ssh
```
  
<span style="color:darkcyan"> **\[Rocky Linux의 경우\]** </span>

```bash
sudo yum install openssh-server -y
sudo systemctl start sshd
sudo systemctl enable sshd
```
  

## 방화벽 설정

방화벽에서 해당 포트를 열어줘야 합니다.  

<span style="color:#dd4814"> **\[Ubuntu의 경우\]** </span>

```bash
sudo ufw allow 22
sudo ufw reload
```

<span style="color:darkcyan"> **\[Rocky Linux의 경우\]** </span>

```bash
sudo firewall-cmd --permanent --zone=public --add-port=22/tcp
sudo firewall-cmd --reload
sudo firewall-cmd --list-ports
```
  

만일 가상머신에서 실험하는 용도로 사용할 거라면 **아예 방화벽 자체를 비활성하는 방법도 있습니다.**  
  
<span style="color:#dd4814"> **\[Ubuntu의 경우\]** </span>

```bash
sudo systemctl stop ufw
sudo systemctl disable ufw
```
  
<span style="color:darkcyan"> **\[Rocky Linux의 경우\]** </span>

```bash
sudo systemctl stop firewalld
sudo systemctl disable firewalld
```


## 호스트 IP 확인
 
CMD창에서 ipconfig로 IP 대역을 확인합니다.

NAT Network의 기준이 되는 내 컴퓨터의 IP 주소는 **호스트 주소**라고 하고,
Virtualbox 가상 머신(VM) 안의 IP 주소는 **게스트 주소**라고 합니다.

만일 호스트 IP가 **192.168.0.2**고,
서브넷 마스크가 **255.255.255.0**이면,
게스트 IP 192.168.0까지는 같게 설정하고 그 뒤로는 다르게 설정하면 됩니다.

저는 게스트 IP를 **192.168.0.10**으로 설정하기로 하고 진행해보겠습니다.
  

## Virtualbox NAT Network 설정

먼저 Virtualbox를 실행하고, 위와 같이 선택해줍니다.

그런 뒤 NAT Network 탭을 선택해주면 다음과 같은 화면이 나옵니다.

**\[General Options\]**  
이름 : NatNetwork
IP : 192.168.0.0/24
Enable DHCP

/24는 255.255.255.0과 같으며, 이는 192.168.0.1~192.168.0.255를 의미합니다.

따라서 192.168.0까지 같으며, Prefix이기 때문에 나머지는 0으로 설정한 겁니다.

만일 CMD로 확인한 내 컴퓨터의 IP(호스트 IP)가 192.168.50.2 였다면, 192.168.50.0 으로 설정하면 됩니다.
  
  **\[포트포워딩\]**  
호스트 IP : (공란)
호스트 포트 : 2222
게스트 IP : **192.168.0.10**
게스트 포트 : 22

저는 호스트 포트를 2222로 설정해주었습니다만, 22로 해도 상관없습니다.
2222번 포트는 기본적으로 열려있지 않아서 방화벽 설정을 통해 열어주었습니다.

Windows에서 방화벽 설정을 통해 포트를 여는 법은 아래 블로그 글을 참고해주세요.
[https://ansan-survivor.tistory.com/408](https://ansan-survivor.tistory.com/408)
  
  
## 가상머신 네트워크 설정

**\[어댑터 1\]**  
네트워크 어댑터 사용하기
다음에 연결됨 : NAT 네트워크
이름 : NatNetwork
  

## 게스트 OS IP 설정

IP : **192.168.0.10**
SM : 255.255.255.0
GW : 192.168.0.1
DNS : 8.8.8.8
  

## SSH 접속 (일반)

저는 CMD 창을 통해 접속했지만, Putty 등의 프로그램을 사용해도 상관없습니다.

```bash
ssh -p 2222 ubuntu@localhost
```

이라고 입력했는데, -p 2222는 아까 포트포워딩에서 지정했던 2222번 포트로 접속하겠다는 이야기고,

ubuntu는 가상머신(VM) 내 운영체제의 user명,

@뒤에 오는 localhost는 주소를 의미합니다. 127.0.0.1과 같이 숫자로 써도 상관없습니다.
  

## SSH 접속 (RSA 사용)

RSA를 사용하여 SSH 접속하는 방법도 있습니다.  
물론 버추얼박스 내의 리눅스에 SSH RSA 접속하려면, 위에서 쭈욱 설명드렸던 것처럼 먼저 NAT Network 관련 설정들을 모두 마치신 상태에,  
SSH 일반 연결까지 가능한 상태여야 합니다.
  
RSA SSH 접속 시엔 다양한 툴을 사용할 수 있는데, 몇 가지만 설명해보겠습니다.

### Putty Keygen을 사용하여 SSH RSA 접속하는 법

[https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)  
먼저 위 링크에서 putty를 다운받아 설치합니다.

putty key generator를 실행한 뒤, Generate 버튼을 누릅니다.  
그러면 커다란 빈 칸이 나타나는데, 그 위에 대고 커서를 막 움직여줍니다.  
그러면 키 생성이 완료됩니다.

authorized_keys 파일에 들어갈 공개키 내용을 Ctrl+C로 복사해두고,  
Save private key 버튼을 눌러 개인키를 저장합니다.  
(저는 passphrase를 따로 입력하지 않았습니다. 이러면 암호 없이 SSH RSA 로그인이 가능해집니다.)
  
그리고 cmd 창을 열어 다음과 같이 입력해 ssh에 접속합니다.

```bash
#ubuntu : 리눅스 유저명
#localhost : 주소를 의미. localhost는 127.0.0.1과 같음.
ssh -p 2222 ubuntu@localhost
yes
sudo su
mkdir .ssh
cd .ssh
vi authorized_keys
```

파일 내용으로는 아까 Ctrl+C로 복사해뒀던 내용을 Ctrl+Shift+V로 붙여넣고 저장해줍니다.

다시 putty를 열어 SSH로 접속합니다.

이렇게 비밀번호 입력없이 접속에 성공했습니다.

### Git Bash를 사용하여 SSH RSA 접속하는 법

[https://light-tree.tistory.com/232](https://light-tree.tistory.com/232)
[https://light-tree.tistory.com/234](https://light-tree.tistory.com/234)
위 글을 참고하여 진행하였습니다.
  
윈도우에서 ssh-keygen을 하게 되면 윈도우와 리눅스 간의 줄바꿈 설정이 서로 달라서(CRLF vs LF) 접속 에러가 납니다.  
이런 문제를 해결하기 위해서는 Git Bash를 이용해주는 것이 좋다고 하여 Git Bash로 진행하였습니다.

우선 Windows에서 Git Bash를 실행합니다.  
꼭 관리자 권한으로 열지 않아도 됩니다.  

```
ssh-keygen
엔터 3번
```

저는 비밀번호를 설정하지 않을 거기 때문에 엔터를 3번 눌러줬습니다.

```bash
cd .ssh
scp -P 2222 id_rsa.pub ubuntu@localhost:.
yes
```

우분투에 접속하면 현재 디렉터리는 홈 디렉터리입니다.

```bash
#호스트에서 2222번을 사용해 우분투에 접속
ssh -p 2222 ubuntu@localhost
sudo su
cd .ssh
#no such 어쩌고 저쩌고 하면 .ssh 디렉터리 생성 후 .ssh 안으로 이동
mkdir .ssh
cd .ssh
#이미 있으면 .ssh 디렉터리 안으로 이동하게 됩니다.
```

```bash
#홈 디렉터리에 있는 공개키 파일을 .ssh 폴더 안에 저장
cat ../id_rsa.pub >> authorized_keys
exit
exit
```

이제 윈도우에서 RSA 접속을 해봅니다.

```bash
#윈도우에 저장된 개인키로 리눅스에 SSH RSA 접속
ssh -i ~/.ssh/id_rsa -p 2222 ubuntu@localhost
```

비밀번호 없이 입력해야 성공입니다.  
만일 비밀번호를 요구한다면 뭔가 잘못된 것이니 아래 링크 글을 참고해주세요.  
[https://light-tree.tistory.com/234](https://light-tree.tistory.com/234)