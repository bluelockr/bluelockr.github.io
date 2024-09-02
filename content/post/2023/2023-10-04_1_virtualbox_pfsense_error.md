+++
title = "Virtualbox에 pfSense 설치 시 오류"
summary = "pfSense installation error in Virtualbox"
date = 2023-10-04T00:00:00+00:00
cover = ""
slug = "pfsense_installation_error_in_virtualbox"
tags = ['Application', 'Linux', 'Virtualbox']
draft = false
+++
## ⭐panic : Could not malloc 163840 bytes with M_WAITOK from 오류

```bash
Consoles: internal video/keyboard  
BIOS CD is cd0  
BIOS drive C: is disk0  
panic: Could not malloc 163840 bytes with M_WAITOK from /var/jenkins/workspace/pfSense-CE-snapshot-2_7_0-main/sources/FreeBSD-src-RELENG_2_7_0/  
sys/contrib/openzfs/module/zstd/zfs_zstd.c line 788  
--press a key on the console to reboot <--  
```

원인 : 램 용량 부족입니다.

## ⭐CPU doesn't support long mode

```bash
can't find '/etc/hostid'  
/boot/kernel/zfs.ko size 0x5ba0d8 at 0x35a9000  
can't find '/boot/entrophy'  
CPU doesn't support long mode  
```

원인 : 운영체제는 64비트인데 VM 설정이 32bit입니다.

[https://lasthackers.com/virtualbox-opnsense-cpu-doesnt-support-long-mode-error/](https://lasthackers.com/virtualbox-opnsense-cpu-doesnt-support-long-mode-error/)

## ⭐ panic: running without device atpic requires a local APIC

```bash
GDC: no debug prots present  
KDB: debugger backends: ddb  
KDB: current backend: ddb  
---<<BOOT>>---  
panic: running without device atpic requires a local APIC  
cpuid = 0  subnet
time = 1  
KDB: enter: panic  
```

원인 : I/O APIC 사용 설정이 안 되어 있습니다.

[http://flummox-engineering.blogspot.com/2014/01/freebsd-running-without-device-atpic.html](http://flummox-engineering.blogspot.com/2014/01/freebsd-running-without-device-atpic.html)

그 외에도 다른 문제점이 발생하거든 VM 네트워크 설정이나 플로피 디스크 사용 해제 여부도 살펴보시기 바랍니다.