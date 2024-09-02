---
title: "패스워드 관리자 Bitwarden 설치 및 기본 사용법"
summary: "How to install and use Bitwarden"
description: "How to install and use Bitwarden"
draft: false
date: 2024-09-02T00:00:00.000Z
slug: "bitwarden_installation_and_userguide"
tags:
  - Application
  - Bitwarden
---

```bash
# 환경(Environment)
웹브라우저 : Microsoft Edge
스마트폰 : Samsung Galaxy A8 2018(Google Android)
```

## ⭐ Bitwarden 계정 만들기

[https://vault.bitwarden.com/](https://vault.bitwarden.com/)

위 링크를 클릭하여 열어줍니다.
<br>
<br>

<img style='border:1px solid #000000; border-radius: 10px' src="/../../images/2024/2024-09-02_1_bitwarden_installation_and_userguide/1.png" width="400">

'계정 만들기'로 들어갑니다.
<br>
<br>

<img style='border:1px solid #000000; border-radius: 10px' src="/../../images/2024/2024-09-02_1_bitwarden_installation_and_userguide/2.png" width="400">

필요한 항목들을 모두 입력하여 계정을 만들어 줍니다.
<br>
<br>
<br>

## ⭐ 2차 인증 설정

[https://vault.bitwarden.com/](https://vault.bitwarden.com/)

위 링크로 다시 들어가 아까 만든 계정에 로그인해줍니다.
<br>
<br>

<img style='border:1px solid #000000; border-radius: 10px' src="/../../images/2024/2024-09-02_1_bitwarden_installation_and_userguide/3.png" width="600">

복구 코드 :   
계정에 접근할 수 있는 마지막 수단입니다.  
꼭 안전한 곳에 안전하게 보관해두도록 합니다.  
따로 적어두거나 보관하기 귀찮다고 사용 안 하시는 건 추천하지 않습니다.  
계정에 로그인할 수 없는 상황에서 복구 코드마저 없으면 방법이 없는 경우가 많습니다...

이메일 : 
편리한 2차 인증 수단입니다.  
하지만 그만큼 보안상 좋지는 않아서 개인적으로는 추천하지 않는 방식입니다.

인증 앱 :  
Google OTP, Microsoft Authenticator 등의 2FA 앱을 사용하여 다른 사람들이 함부로 Bitwarden 계정에 접근하는 것을 막을 수 있습니다.  
꼭 설정해두시는 걸 추천드립니다.
<br>
<br>
<br>

## ⭐ Bitwarden 확장 프로그램 설치

Microsoft Edge 기준으로 설명합니다.

[https://chromewebstore.google.com/category/extensions?hl=ko](https://chromewebstore.google.com/category/extensions?hl=ko)

위 링크를 클릭하여 Chrome 웹 스토어로 들어갑니다.  
여기서 확장 프로그램을 설치할 수 있습니다.

<img style='border:1px solid #000000; border-radius: 10px' src="/../../images/2024/2024-09-02_1_bitwarden_installation_and_userguide/4.png" width="600">

이렇게 확장 프로그램을 설치하고 나서는 위처럼 도구 모음에 표시해주는 것이 좋습니다.
<br>
<br>

<img style='border:1px solid #000000; border-radius: 10px' src="/../../images/2024/2024-09-02_1_bitwarden_installation_and_userguide/5.png" width="400">
<br>
🔼 Microsoft Edge에서 도구 모음에 표시해주는 방법
<br>
<br>
<br>

<img style='border:1px solid #000000; border-radius: 10px' src="/../../images/2024/2024-09-02_1_bitwarden_installation_and_userguide/6.png" width="400">
<br>
🔼 Google Chrome에서 도구 모음에 표시해주는 방법
<br>
<br>
<br>
<br>

## ⭐ Bitwarden 확장 프로그램 사용법

### 💧 사이트 추가

<img style='border:1px solid #000000; border-radius: 10px' src="/../../images/2024/2024-09-02_1_bitwarden_installation_and_userguide/7.png" width="400">

아까 도구모음에 추가한 확장 프로그램 아이콘을 클릭해주고 로그인합니다.
<br>
<br>

<img style='border:1px solid #000000; border-radius: 10px' src="/../../images/2024/2024-09-02_1_bitwarden_installation_and_userguide/8.png" width="400">

보관함으로 이동해주고 '+' 버튼을 눌러서 보관함에 사이트를 추가해줍니다.
** 보관함 : Bitwarden 내에서 사이트의 아이디와 비밀번호가 저장되어있는 공간

참고로, 추가하고자 하는 사이트에 먼저 가신 다음에 하시는 걸 추천드립니다.  

예를 들어 쿠팡에 Bitwarden을 사용하여 로그인하고자 한다면,  
먼저 주소표시줄에 coupang.com을 입력하고 엔터를 눌러 쿠팡 홈페이지에 들어가신 후,  
그 다음에 Bitwarden 확장 프로그램 아이콘을 클릭한 뒤 보관함으로 이동한 뒤 '+'버튼을 눌러주는 것이 좋다는 뜻입니다.  
그래야 별도로 사이트명과 URL을 입력할 필요가 없습니다.  
<br>
<br>

<img style='border:1px solid #000000; border-radius: 10px' src="/../../images/2024/2024-09-02_1_bitwarden_installation_and_userguide/9.png" width="400">

위 사진을 참고하여 원하시는 사이트를 보관함에 추가하면 됩니다.
<br>
<br>
<br>

### 💧 사이트에 로그인하는 방법

<img style='border:1px solid #000000; border-radius: 10px' src="/../../images/2024/2024-09-02_1_bitwarden_installation_and_userguide/10.png" width="400">

위 사진대로 따라하면 마스터 비밀번호를 입력하라고 나올 겁니다.  
입력해줍니다.  
그러면 이제 Bitwarden을 사용해 로그인할 준비가 된 겁니다.
<br>
<br>

<img style='border:1px solid #000000; border-radius: 10px' src="/../../images/2024/2024-09-02_1_bitwarden_installation_and_userguide/11.png" width="400">


저는 쿠팡을 예시로 들어보겠습니다.  
위와 같이 따라하면 Bitwarden 보관함에 저장해둔 아이디와 비밀번호로 로그인할 수 있습니다.  
비밀번호 입력하는 란을 클릭해야 잘 되더라구요.
<br>
<br>
<br>

### 💧 추가했던 사이트 편집/삭제

<img style='border:1px solid #000000; border-radius: 10px' src="/../../images/2024/2024-09-02_1_bitwarden_installation_and_userguide/12.png" width="400">

보관함 목록에서 원하는 사이트를 위와 같이 클릭합니다.
<br>
<br>

<img style='border:1px solid #000000; border-radius: 10px' src="/../../images/2024/2024-09-02_1_bitwarden_installation_and_userguide/13.png" width="400">

탭 메뉴에서는 조금 다르게 뜨는데, 위와 같이 다른 버튼을 눌러주면 됩니다.
<br>
<br>

<img style='border:1px solid #000000; border-radius: 10px' src="/../../images/2024/2024-09-02_1_bitwarden_installation_and_userguide/14.png" width="400">

편집 버튼을 눌러 편집하거나 보관함에 저장했던 사이트의 정보를 삭제할 수 있습니다.  
<br>
<br>
<br>

## ⭐ Bitwarden 모바일 앱 사용법

스마트폰에서는 구글 플레이스토어(Google Playstore)나 애플 앱스토어(AppStore)에서 Bitwarden을 설치합니다.

그리고 나서 아이폰에서는 '키체인', 안드로이드(혹은 갤럭시 스마트폰)에서는 '자동입력서비스(Autofill)'을 사용해주면 됩니다.

[https://rororo3.tistory.com/7](https://rororo3.tistory.com/7)  
🔼 아이폰 키체인에 Bitwarden 등록하는 법

[https://blog.naver.com/PostView.naver?blogId=gkrwja88&logNo=221852484171](https://blog.naver.com/PostView.naver?blogId=gkrwja88&logNo=221852484171)  
🔼 갤럭시 스마트폰에서 Bitwarden 등록하는 법  
(링크에서는 Google 비밀번호 관리자를 사용했지만 우리는 대신에 Bitwarden을 사용해주면 됩니다.)