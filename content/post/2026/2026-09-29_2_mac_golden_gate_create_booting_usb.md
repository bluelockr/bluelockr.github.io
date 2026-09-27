---
title: "Mac OS 골든게이트 설치 USB 생성"
summary: "How to Create Golden Gate Booting USB"
description: "How to Create Golden Gate Booting USB"
draft: false
date: 2026-09-29T00:00:00.000Z
slug: "2026-09-29_2_mac_golden_gate_create_booting_usb"
tags:
  - OS
---

```bash
# 환경(Environment)
OS : MacOS Golden Gate 27.0
```
# 기본
https://support.apple.com/ko-kr/101578[https://support.apple.com/ko-kr/101578]

위 링크가 이번 글의 모든 기초입니다.

# 1. Mac OS 최신 버전 확인
https://support.apple.com/ko-kr/109033#latest[https://support.apple.com/ko-kr/109033#latest]

여기서 최신 버전을 확인합니다. 현재 골든게이트의 최신 버전은 2026.09.27 기준 `27.0`입니다.

# 2. 맥 터미널에서 Mac OS 다운로드
```bash
softwareupdate --fetch-full-installer --full-installer-version 27.0
```

# 3. USB 플래시 드라이브 포맷
맥의 디스크 유틸리티를 이용하여 포맷하되 `JHFS+`로 포맷하고 이름은 `MyVolume`으로 설정합니다.

# 4. 설치 USB 제작
```bash
sudo /Applications/Install\ macOS\ 27\ Golden\ Gate.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume
```

# 5. 재부팅할 때 전원 버튼 꾸욱 누르기