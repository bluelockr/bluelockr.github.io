---
title: "1Password 8 PC버전 기본 사용법"
summary: "1Password 8 basic guide for pc version"
description: "1Password 8 basic guide for pc version"
draft: false
date: 2024-09-06T00:00:00.000Z
slug: "1password8_basic_guide_for_pc"
tags:
  - Application
---

```bash
# 환경(Environment)
OS : Windows 11 Home
```

이 글은 복잡해보이는 1Password의 메뉴를 매우 간단하게 설명할 목적으로 작성되었습니다.
<br>
<br>
<br>

## ⭐ 1Password 설치

<details>
<summary>접기/펼치기</summary>

[https://1password.com/ko/downloads/](https://1password.com/ko/downloads/)

1Password에서는 데스크탑 프로그램, 웹 브라우저 확장 프로그램, 스마트폰 앱까지 모두 지원합니다.  
위 링크에서 다운받아주면 됩니다.
<br>
<br>
</details>

## ⭐ 확장 프로그램 메뉴

<details>
<summary>접기/펼치기</summary>

<img style='border:1px solid #000000; border-radius: 10px' src="/../../images/2024/2024-09-06_1_1password8_basic_guide_for_pc/1.png" width="600">  
<br>

일단 확장 프로그램 메뉴 위주로 살펴봅니다.  
여기서 중요한 설정을 할 수 있기 때문입니다.
<br>
<br>

### 💧 확장 프로그램 설정 - 일반

<details>
<summary>접기/펼치기</summary>

<img style='border:1px solid #000000; border-radius: 10px' src="/../../images/2024/2024-09-06_1_1password8_basic_guide_for_pc/2.png" width="600">  
<br>

* 브라우저 기본 비밀번호 관리 프로그램으로 설정
* 1Password 데스크톱 앱과 확장 프로그램 통합
<br>
<br>
</details>

### 💧 확장 프로그램 설정 - 보안

<details>
<summary>접기/펼치기</summary>

<img style='border:1px solid #000000; border-radius: 10px' src="/../../images/2024/2024-09-06_1_1password8_basic_guide_for_pc/3.png" width="600">  
<br>

'1Password 열기' 버튼을 누르면 1Password 데스크탑 앱의 보안 탭이 열립니다.
<br>
<br>

<img style='border:1px solid #000000; border-radius: 10px' src="/../../images/2024/2024-09-06_1_1password8_basic_guide_for_pc/4.png" width="600">  
<br>

* Windows Hello 기능 사용
* 잠금 설정
* 클립보드 설정

<br>
<br>
</details>

### 💧 확장 프로그램 설정 - 자동으로 채우기 및 저장

<details>
<summary>접기/펼치기</summary>

<img style='border:1px solid #000000; border-radius: 10px' src="/../../images/2024/2024-09-06_1_1password8_basic_guide_for_pc/5.png" width="600">  
<br>

* 자동으로 채우기 설정(자동채우기=자동입력=Autofill=Autotype)

<br>
<br>
</details>

### 💧 확장 프로그램 설정 - 계정 및 금고

<details>
<summary>접기/펼치기</summary>

<img style='border:1px solid #000000; border-radius: 10px' src="/../../images/2024/2024-09-06_1_1password8_basic_guide_for_pc/6.png" width="600">  
<br>

* 계정 삭제
* 계정 관리

여기서 '계정 관리'를 누르면 내 프로필 페이지로 이동합니다.
<br>
계정의 중요한 설정(2FA, 백업코드, 시크릿키, 복구코드 등)을 설정할 수 있습니다.
<br>
<br> 
<img style='border:1px solid #000000; border-radius: 10px' src="/../../images/2024/2024-09-06_1_1password8_basic_guide_for_pc/7.png" width="600">  
<br>
내 프로필 페이지의 모습입니다.

* 이메일 변경
* 시크릿 키 다시 설정
* 비밀번호 변경
* 2단계 인증 관리
* 언어 변경
* 이메일 환경 설정 변경

* 이메일 주소 보기
* 시크릿 키 보기(Secret Key)
* 설정 코드 보기
* 복구 코드 보기
* Emergency Kit 제공(마스터 패스워드를 제외한 중요 복구 정보들이 모두 들어있음)

* 다른 멤버 초대
* 청구/결제

<br>
<br>
</details>

### 💧 확장 프로그램 설정 - 외관 및 단축키

<details>
<summary>접기/펼치기</summary>

<img style='border:1px solid #000000; border-radius: 10px' src="/../../images/2024/2024-09-06_1_1password8_basic_guide_for_pc/8.png" width="600">  

* 언어
* 테마
* 키보드 단축키로 활성화
* 키보드 단축키로 잠금

<br>
<br>
<br>
<br>
</details>
</details>


## ⭐ 1Password 데스크탑 앱

<details>
<summary>접기/펼치기</summary>

<img style='border:1px solid #000000; border-radius: 10px' src="/../../images/2024/2024-09-06_1_1password8_basic_guide_for_pc/9.png" width="600">  
<br>
<br>

* 가져오기
* 내보내기

여기서 가장 눈여겨 봐야 할 것은 '가져오기/내보내기', '설정'입니다.
<br>
평소에 안전한 곳에 잘 백업해두길 추천드립니다.
<br>
1PUX 파일로 백업하면 비밀번호를 요구하고,
<br>
CSV 파일로 백업하면 비밀번호 없이도 열 수 있습니다.
<br>
불러오기는 말 그대로 Bitwarden 같은 패스워드 관리자 등에서 불러올 때 사용하는 기능입니다.

저기서 설정(Ctrl+,)을 열면 더 자세한 설정을 할 수 있습니다.
<br>
<br>

### 💧 데스크탑 앱 - 설정 - 일반

<details>
<summary>접기/펼치기</summary>
<img style='border:1px solid #000000; border-radius: 10px' src="/../../images/2024/2024-09-06_1_1password8_basic_guide_for_pc/10.png" width="600">  
<br>

* 알림 영역에 고정
* 키보드 단축키
  * 1Password 표시
  * 빠른 액세스 표시 : 데스크탑 앱 아이디와 비밀번호를 자동채우기 할 때 쓰입니다.
  * 1Password 잠금
  * 자동으로 채우기

<br>
<br>
</details>

### 💧 데스크탑 앱 - 설정 - 스타일

<details>
<summary>접기/펼치기</summary>
<img style='border:1px solid #000000; border-radius: 10px' src="/../../images/2024/2024-09-06_1_1password8_basic_guide_for_pc/11.png" width="600">  
<br>

* 테마
* 밀도 : 넉넉함/빽빽함
* 인터페이스 확대/축소
* 사이드바에 항상 표시 : 카테고리, 태그

<br>
<br>
</details>

### 💧 데스크탑 앱 - 설정 - 보안

<details>
<summary>접기/펼치기</summary>
<img style='border:1px solid #000000; border-radius: 10px' src="/../../images/2024/2024-09-06_1_1password8_basic_guide_for_pc/12.png" width="600">  
<br>

* Windows Hello
* 잠금 설정 : 자동 잠금 등...
* 클립보드 비우기 설정

<br>
<br>
</details>

### 💧 데스크탑 앱 - 설정 - 브라우저

<details>
<summary>접기/펼치기</summary>
<img style='border:1px solid #000000; border-radius: 10px' src="/../../images/2024/2024-09-06_1_1password8_basic_guide_for_pc/13.png" width="600">  
<br>

* 브라우저와 데스크톱 앱 연결
<br>
<br>
</details>

### 💧 데스크탑 앱 - 설정 - 연구실

<details>
<summary>접기/펼치기</summary>
<img style='border:1px solid #000000; border-radius: 10px' src="/../../images/2024/2024-09-06_1_1password8_basic_guide_for_pc/14.png" width="600">  
<br>

<img style='border:1px solid #000000; border-radius: 10px' src="/../../images/2024/2024-09-06_1_1password8_basic_guide_for_pc/15.png" width="600">  
<br>

* 자동 입력

아까 '데스크탑 앱 - 설정 - 일반 - 키보드 단축키'에서 '빠른 액세스' 키로 설정된 키가 자동 입력할 때 쓰입니다.
<br>

데스크탑 앱(PC카톡, 스팀 등등)에 로그인할 때 자동채우기용으로 유용하게 쓰입니다.

이 정도만 알아두어도 사용에는 큰 지장이 없을 겁니다.
</details>
</details>