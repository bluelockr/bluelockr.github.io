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

터미널이 열리면 다음 명령어들을 실행합니다.

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

이후 버추얼박스 메인 화면에서 위와 같이 진행합니다.
다시 가상머신을 켜보면 복붙이 잘 되는 걸 확인하실 수 있습니다.