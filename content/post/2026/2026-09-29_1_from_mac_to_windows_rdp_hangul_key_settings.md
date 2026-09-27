---
title: "맥에서 윈도우 프로 원격접속 시 한영키 문제 해결"
summary: "How to Convert Hangul Key in Remote Windows Pro from Mac"
description: "How to use Sticky Key with Globe Key in Mac"
draft: false
date: 2026-09-29T00:00:00.000Z
slug: "from_mac_to_windows_rdp_hangul_key_settings"
tags:
  - Application
---

```bash
# 환경(Environment)
OS : MacOS Golden Gate 27.0
OS : Windows 11 Pro
기타
- WindowsApp으로 맥에서 RDP 원격접속
- Karabiner 사용 중(중요!)
```
카라비너 프로그램을 통해 맥에서 우측 cmd 키를 f13으로 할당하고 이를 한영키로 사용하고 있는 상황입니다.
<br>

shift+space 등으로 한영키 전환하려고 했지만 되지 않았습니다.
<br>

해결책을 찾다가 다음과 같이 세팅하면 된다는 걸 발견하여 포스트로 남깁니다.
<br>

윈도우 원격접속까지 할 정도라면 어느정도 컴퓨터에 대한 이해도가 있다고 가정하고 글쓰겠습니다.
<br>

# 1. PowerToys 설치
- Microsoft Store에서 검색해서 설치합니다.

# 2. Keyboard Manager 열기
- Powertoys에서 제공하는 기능 중 하나입니다.

# 3. 새 편집기 사용 끄기
- 기존 편집기가 더 세부적인 옵션 설정이 가능하더라고요.

# 4. 한영키 전환 등록

<img style=' border-radius: 10px' src="/../../images/2026/2026-09-29_1_from_mac_to_windows_rdp_hangul_key_settings/1.png" width="800">
<br>

우측 cmd키를 누르면 `Packet`키 등으로 표시가 됩니다. 이를 `IME Hangul`키에 매핑합니다.
`IME Convert` 이런 거 하면 안 먹힙니다.
<br>
<br>
<br>