---
title: Linux RAID 설정하는 법
description: How to Configure RAID in Linux
slug: linux_raid
date: 2023-09-05 00:00:00+0000
image: 
tags:
    - Linux
    - RAID
---
Virtualbox를 이용하여 실습해보았습니다.

## Virtualbox에서 디스크 추가
[https://www.bearpooh.com/190](https://www.bearpooh.com/190)

디스크 추가하는 법은 위 글을 참고하면 됩니다.

저는 RAID 5 기준으로 진행하였기에 vdi 유형으로 디스크를 3개 추가해주었습니다.


## 디스크 파티션 확인

```bash
sudo fdisk -l
```

위 명령어를 실행하면 /dev/sdb, /dev/sdc, /dev/sdd 같은 이름으로 디스크가 등록된 걸 볼 수 있습니다.


## 디스크 파티션 만들기

저는 평소에 루트 권한을 요구하는 명령어를 짧게 치기 위해서 sudo su를 먼저 해주는데,  
RAID 구성을 할 때 그러면 디스크에 접근하려고 할 때 패스워드를 요구하더라구요.  
그래서 그냥 루트 계정에서 명령어를 실행하지 않고 sudo로만 진행합니다.

```bash
sudo fdisk /dev/sdb
n
엔터를 4번 누릅니다.(디폴트 값으로 적용하는 과정입니다.)
t
fd
w
```
  
new : new. 새 파티션 생성.
t : type. 파티션 타입 변경.
fd : RAID 적용.
w : write. 변경 내용 기록.

이 과정을 sdc, sdd에도 똑같이 반복해줍니다.


## mdadm 설치

```bash
# 우분투는 apt, 레드햇 계열은 yum 사용.
sudo apt install mdadm -y
sudo yum install mdadm -y
```


## RAID 설정

```bash
sudo mdadm --create /dev/md5 --level=5 --raid-devices=3 /dev/sdb /dev/sdc /dev/sdd
y
```


## RAID 디스크 포맷

```bash
sudo mkfs.ext4 /dev/md5
```
  
  
## 자동마운트 설정

```bash
sudo blkid /dev/md5
```

위 명령어를 실행하면 " " 안에 UUID가 들어가 있는 게 보일 겁니다. 그걸 Ctrl+Shift+C를 눌러 복사해놓으세요.

```bash
sudo mkdir /mnt/md5
```
(마운트할 디렉터리를 만드는 과정입니다.)

```bash
sudo vi /etc/fstab
```
/etc/fstab이 vi편집기로 열리게 됩니다.(vi대신 gedit같은 GUI텍스트편집기를 사용해도 무방합니다.)

맨 밑에 다음과 같이 추가해주세요.

```bash
/dev/disk/by-uuid/uuid명 /mnt/md5 auto nosuid,nodev,nofail,x-gvfs-show 0 0
```
uuid명에는 아까 Ctrl+Shift+C로 복사해넣었던 UUID를 Ctrl+Shift+V로 붙여넣으면 됩니다.

```bash
sudo reboot
```