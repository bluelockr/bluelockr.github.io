+++
title = "GNS3에서 Alpine Linux 노드에서 라우터로 텔넷 접속하기"
summary = "How to Telnet to GNS3 Node from Alpine Linux Node"
date = 2023-10-29T00:00:00+00:00
cover = ""
slug = "gns3_alpine_node_telnet"
categories = ['network']
tags = ['Network', 'GNS3']
draft = false
+++

GNS3 설치 과정과 IOU 불러오는 과정 등은 생략하겠습니다.

## ⭐템플릿 불러오기

![Alt text](/../../images/2023/2023-10-29_1_gns3_alpine_node/1.png)  

![Alt text](/../../images/2023/2023-10-29_1_gns3_alpine_node/2.png)  

![Alt text](/../../images/2023/2023-10-29_1_gns3_alpine_node/3.png)  

![Alt text](/../../images/2023/2023-10-29_1_gns3_alpine_node/4.png)  

![Alt text](/../../images/2023/2023-10-29_1_gns3_alpine_node/5.png)  

![Alt text](/../../images/2023/2023-10-29_1_gns3_alpine_node/6.png)  

![Alt text](/../../images/2023/2023-10-29_1_gns3_alpine_node/7.png)  

![Alt text](/../../images/2023/2023-10-29_1_gns3_alpine_node/8.png)  

![Alt text](/../../images/2023/2023-10-29_1_gns3_alpine_node/9.png)  

## ⭐알파인 리눅스 노드 텔넷 설정

### 💧패키지 설치

```bash
vi /etc/network/interfaces
```

아래 파일과 같이 주석(#)을 삭제하여 DHCP를 활성화 시킵니다.

![Alt text](/../../images/2023/2023-10-29_1_gns3_alpine_node/10.png)  

저장한 뒤 아래 명령어를 사용하여 네트워크를 활성화 시킨 뒤 텔넷 관련 프로그램을 설치합니다.

```bash
ifup eth0
ping 8.8.8.8

apk add busybox-extras

vi /etc/network/interfaces
```

아래 파일과 같이 주석(#)을 추가하고 삭제하여 DHCP를 비활성화 시키고 정적 IP를 활성화 시킵니다.

저는 192.168.0.10/24 에 게이트웨이는 192.168.0.1로 설정하였습니다.

![Alt text](/../../images/2023/2023-10-29_1_gns3_alpine_node/11.png)  

```bash
ifdown eth0
ifup eth0
```

## ⭐라우터 텔넷 설정

먼저 알파인 리눅스와 NAT 간에 연결되어있던 케이블을 클릭한 뒤 Delete 키를 눌러 케이블을 삭제해줍니다.  
그리고 알파인 리눅스와 라우터 간에 케이블을 연결해줍니다.

저는 라우터의 FastEthernet0/0 포트와 알파인 리눅스 노드를 서로 연결해주겠습니다.

케이블 연결이 끝나면 라우터를 'start'한 뒤 더블 클릭하여 터미널로 접속하고 아래 명령어를 입력하여 텔넷 접속 설정합니다.

[https://jeongzzang.com/39](https://jeongzzang.com/39) 
🔼라우터에 텔넷, SSH 접속하는 법

```bash
R1> enable
R1# configure terminal
R1(config)# line vty 0 4
R1(config-line)# transport input telnet
R1(config-line)# password cisco
# 전역 설정 모드 진입을 위해 필요함
R1(config-line)# enable password
R1(config-line)# exit
R1(config)# interface fastEthernet0/0
R1(config-if)# no shutdown
# 아까 설정한 알파인 리눅스의 게이트웨이가 192.168.0.1/24이었으므로.
R1(config-if)# ip address 192.168.0.1 255.255.255.0
R1(config-if)# end
R1# copy running-config startup-config
```

## ⭐텔넷 접속

알파인 리눅스 터미널 창을 가서 다음과 같이 입력합니다.

![Alt text](/../../images/2023/2023-10-29_1_gns3_alpine_node/12.png)  

```bash
telnet 192.168.0.2
```

암호를 입력하면 접속 가능합니다.