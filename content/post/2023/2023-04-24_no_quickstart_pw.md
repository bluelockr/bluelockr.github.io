---
title: 리눅스 민트 빠른 실행 아이콘 기본 키 암호 입력 없애기
description: Disable Quick Launch Password Requirement
slug: no_quick_launch_pw
date: 2023-04-24 00:00:00+0000
image: 
tags:
    - Ubuntu
    - Linuxmint
---
윈도우로 치자면 빠른 실행 아이콘에 해당되는 곳에 크롬(chrome) 아이콘을 등록시켜놨더니,  
열받게 자꾸 암호를 입력하라고 하더라구요.  
  
이걸 해결하려면 일단 keyring 파일을 삭제해줘야 합니다.

#how to disable keyring for quick launch icon in linuxmint or ubuntu
#how to disable password input for keyring in linuxmint or ubuntu
​

## 키 모음 잠금 없애기 (How to disable Keyring)
​
리눅스 민트 21.1 xfce 기준입니다.  
  
우선 터미널을 열고 다음 명령어들을 차례대로 실행합니다.  
(Open Terminal and Run these commands in following order.)  
​
```bash
sudo su
cd /home/username/.local/share/keyrings
rm -rf *.keyring
```
​
여기서 username은 당연히 여러분이 설정한 사용자명을 말하는 겁니다.  
('username' here is your username you set.)  
​
아무것도 입력하지 않고 Enter키를 누르면 클릭만 해도 암호를 묻지 않고 실행하게 됩니다.  
(If you press Enter Key without putting any words, you will not be asked password again.)