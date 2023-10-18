---
title: Virtualbox에서 Linux 클립보드(복사-붙여넣기) 사용하는 법
description: How to Share Clipboard between hosts and guests in Virtualbox
slug: virtualbox_clipboard
date: 2023-09-17 00:00:00+0000
image: 
tags:
    - Windows
    - Linux
    - Virtualbox
---
보통 윈도우에서 버추얼박스를 사용해 그 안에 있는 가상머신에 우분투를 돌리게 되는데,

이 때 윈도우에서 복사한 내용이 가상머신 안의 우분투에 붙여넣기가 안됩니다.

이를 해결하려면 다음과 같이 진행합니다.

![Alt text](/../../images/2023/2023-09-17_virtualbox_clipboard/1.png)  

먼저 **게스트 확장 CD 이미지 삽입**을 해줍니다.  

그 다음 삽입된 CD 이미지 드라이브를 파일 관리자로 열어준 뒤 폴더 내에서 우클릭한 뒤 터미널로 열어줍니다.

그리고 명령어를 실행해야 하는데요,

우분투의 경우는 다음과 같이 수행합니다.

```bash
sudo su
apt install gcc make perl -y
sh VBoxLinuxAdditions.run
reboot now
```


레드햇 계열의 리눅스에서는 다음과 같이 수행합니다.

```bash
sudo su
yum groupinstall "Development Tools"
yum install dkms kernel-devel kernel-headers
yum update
reboot now
```

![Alt text](/../../images/2023/2023-09-17_virtualbox_clipboard/2.png)  

![Alt text](/../../images/2023/2023-09-17_virtualbox_clipboard/3.png)  

이후 버추얼박스 메인 화면에서 위와 같이 진행합니다.  
다시 가상머신을 켜보면 복붙이 잘 되는 걸 확인하실 수 있습니다.