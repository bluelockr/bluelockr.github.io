+++
title = "Rocky Linux 9.2 서버 네트워크 설정 실습"
summary = ""
date = 2023-11-17T00:00:00+00:00
cover = ""
slug = "rocky_server_network_configuration"
categories = ['linux']
tags = ['RHEL', 'Rocky']
draft = false
+++

*** 이 글은 공부 중 실습용으로 작성한 것으로, 실제 서비스에 적용하지 마시기 바랍니다. ***

## ⭐로그인
Rocky Linux 설치 장면은 생략하겠습니다.

서버 부팅에 성공하면 우선 로그인해줘야 합니다.
```bash
ID : root
비번 : (Rocky Linux 설치할 때 지정했던 비밀번호)
```

## ⭐네트워크 설정

[https://docs.rockylinux.org/guides/network/basic_network_configuration/](https://docs.rockylinux.org/guides/network/basic_network_configuration/)  
🔼록키 리눅스 네트워크 설정법

먼저 터미널에 다음 명령어를 입력하여 네트워크 관리자를 엽니다.

```bash
nmtui
```

다음으로는 아래와 같이 진행합니다.
IP는 본인에게 적합하게 설정합니다. 버추얼박스 네트워크 설정의 경우 어떻게 설정해야할지 모르겠다면 다음 글을 참고해주세요

[https://bluelockr.github.io/post/2023/ssh_to_virtualbox_nat_network_common_rsa/](https://bluelockr.github.io/post/2023/ssh_to_virtualbox_nat_network_common_rsa/)  
🔼버추얼박스 NAT 네트워크 설정하는 법

방법은 똑같지만, 

이번에는 NAT 네트워크 설정은 저 같은 경우,  
호스트(버추얼박스를 돌리는 윈도우 10) IP 대역이 192.168.0.0/24 대역이니까 똑같이 192.168.0.0/24 대역으로 맞춰줄겁니다.

![Alt text](/../../images/2023/2023-11-17_1_rocky_server_network/1.png)  
🔼버추얼박스 NAT 네트워크 설정
  
![Alt text](/../../images/2023/2023-11-17_1_rocky_server_network/2.png)  
🔼로키 리눅스 가상머신 네트워크 설정

로키 리눅스 서버 내 네트워크 설정은 다음과 같이 진행할 겁니다.

IP : Manual(수동)
IP : 192.168.0.10
GW : 192.168.0.1
DNS : 8.8.8.8

![Alt text](/../../images/2023/2023-11-17_1_rocky_server_network/3.png)  
  
![Alt text](/../../images/2023/2023-11-17_1_rocky_server_network/4.png)  
  
![Alt text](/../../images/2023/2023-11-17_1_rocky_server_network/5.png)  
  
![Alt text](/../../images/2023/2023-11-17_1_rocky_server_network/6.png)  
  
![Alt text](/../../images/2023/2023-11-17_1_rocky_server_network/7.png)  
  
```bash
nmcli con down enp0s3 && nmcli con up enp0s3
ip add
```

![Alt text](/../../images/2023/2023-11-17_1_rocky_server_network/8.png)  

IP가 192.168.0.10으로 바뀌었으면 성공입니다.

## ⭐SSH 접속

### 💧SSH 설치

```bash
yum install openssh-server openssh-clients -y
systemctl start sshd
systemctl enable sshd
```

### 💧SSH 접속

이제 윈도우에서 PowerShell이나 CMD를 열고 SSH 접속합니다.

다음 두 명령어 중 하나는 먹혀야 정상입니다.

```bash
ssh root@localhost
ssh root@192.168.0.10
```

만일 안된다면 방화벽(firewalld)가 켜져있고 SSH(22번 포트)가 불허되어있는 상태인지 확인합니다.

[https://linuxconfig.org/how-to-open-ssh-port-22-on-rehdat-7-linux-server](https://linuxconfig.org/how-to-open-ssh-port-22-on-rehdat-7-linux-server)  
🔼Firewall SSH 설정하는 법  

```bash
systemctl status firewalld
firewall-cmd --zone=public --permanent --add-service=ssh
firewall-cmd --reload
```

그래도 안 된다면 ROOT SSH 접속이 허용된 상태인지 확인하고 수정합니다.

[https://unix.stackexchange.com/questions/343781/root-password-not-working-but-can-ssh-via-sudo-user-centos](https://unix.stackexchange.com/questions/343781/root-password-not-working-but-can-ssh-via-sudo-user-centos)  
🔼ROOT SSH 접속 설정법

```bash
vi /etc/ssh/sshd_config
```

해당 파일에서 #PermitRootLogin without-password 를 PermitRootLogin yes로 수정하면 됩니다.

```bash
systemctl restart sshd
```

![Alt text](/../../images/2023/2023-11-17_1_rocky_server_network/9.png)

![Alt text](/../../images/2023/2023-11-17_1_rocky_server_network/10.png)

이렇게 SSH 접속에 성공했습니다.