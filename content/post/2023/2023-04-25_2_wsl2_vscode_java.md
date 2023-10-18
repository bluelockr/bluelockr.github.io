---
title: VSCODE Java 개발 환경 구축 (WSL2 사용)
description: How to Set up Vscode Java Dev Env using WSL2
slug: vscode_wsl2_java_dev_env
date: 2023-04-25 00:00:00+0000
image: 
tags:
    - Windows
    - VSCODE
    - JAVA
    - WSL
---
비주얼 스튜디오 코드를 사용하는데 Compile Run, 속도가 너무 느려서 결국 WSL2를 이용하기로 했습니다.

VSCODE에서 자체적으로 제공하는 터미널로는 답이 없더라구요.

비주얼 스튜디오 코드를 사용하는데 빌드 후 실행 속도가 너무 느리다! 이런 분들은 이 글을 봐주세요.

## ⭐Visual Studio Code 설치

[https://code.visualstudio.com/](https://code.visualstudio.com/)

위 링크에서 Visual Studio Code를 다운받아 설치합니다.

![Alt text](/../../images/2023/2023-04-25_1_wsl2_vscode_c/1.png)

이 때 주의할 점이 하나 있는데, 'PATH에 추가' 체크박스에 꼭 체크해주셔야 한다는 점입니다.  
안 그러면 귀찮아집니다.

## ⭐Windows 기능 켜기/끄기

![Alt text](/../../images/2023/2023-04-25_1_wsl2_vscode_c/2.png)

제어판 -> 프로그램 -> 프로그램 및 기능 -> Windows 기능 켜기/끄기에 들어가 **Linux용 Windows 하위 시스템** 과 **가상 머신 플랫폼**의 체크박스를 누르고 확인 버튼을 누릅니다.(재부팅 필요)

## ⭐최신 WSL2 Linux커널 업데이트 패키지 설치

[https://learn.microsoft.com/ko-kr/windows/wsl/install-manual](https://learn.microsoft.com/ko-kr/windows/wsl/install-manual)

위 링크에서 **x64 머신용 최신 WSL2 Linux커널 업데이트 패키지**를 다운로드 후 실행하여 설치합니다.


## ⭐Ubuntu 앱 설치 및 실행

Microsoft Store에서 ubuntu를 설치하고 실행합니다.

![Alt text](/../../images/2023/2023-04-25_1_wsl2_vscode_c/3.png)

계정명과 비밀번호는 자신이 원하는 대로 해줍니다.


## ⭐wsl 설정

Powershell을 관리자 권한으로 실행 후,  

```bash
wsl -l -v
```

을 입력하고 엔터 키를 누릅니다.  
VERSION이 1로 설정되어있으면 다음 명령어를 입력해 2로 변경합니다.  

```bash
wsl --set-default-version 2
```

그리고 VSCODE에서 Extension 중에 **WSL**를 설치합니다.  

[https://learn.microsoft.com/ko-kr/power-pages/configure/vs-code-extension](https://learn.microsoft.com/ko-kr/power-pages/configure/vs-code-extension)  
🔼VSCODE Extension 설치법
  

## ⭐소스 파일 생성

우분투 앱으로 돌아가서 다음 명령어를 입력하여 프로그래밍을 위한 폴더를 생성합니다.

```bash
mkdir Coding
cd Coding
code .
```

VSCODE가 실행되면 우분투 앱에서 다음 명령어를 실행하여 OpenJDK를 설치해줍니다.

```bash
sudo apt-get update
sudo apt install openjdk-17-jdk openjdk-17-jre -y
```

(2023년 4월 현재 기준으로 17이 LTS버전이기 때문)


[https://docs.aws.amazon.com/corretto/latest/corretto-17-ug/generic-linux-install.html](https://docs.aws.amazon.com/corretto/latest/corretto-17-ug/generic-linux-install.html)

다음 명령어를 대신 사용해도 됩니다. 아마존에서 제공하는 Amazon Corretto를 설치하는 명령어입니다.

```bash
wget -O- https://apt.corretto.aws/corretto.key | sudo apt-key add -   
sudo add-apt-repository 'deb https://apt.corretto.aws stable main'
sudo apt-get update; sudo apt-get install -y java-17-amazon-corretto-jdk

java --version
javac --version

code .
```

이렇게 하면 VSCODE가 열립니다.

소스 코드 파일을 생성하고 내용을 작성한 뒤,  
VSCODE에서 Extension 중에 **Extension Pack for Java**를 설치합니다.

이제 자바 코드를 작성하고 실행할 수 있습니다.

참고한 글 :

[WSL & WSL2 설치와 VSCode 연동하기 (velog.io)](https://velog.io/@gidskql6671/WSL-WSL2-%EC%84%A4%EC%B9%98-VSCode-%EC%97%B0%EB%8F%99)  

[VS Code에서 WSL 2와 C++ 환경설정 하기 :: BEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEM (tistory.com)](https://skyqnaqna.tistory.com/entry/VS-Code%EC%97%90%EC%84%9C-WSL-2%EC%99%80-C-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)  

[VS Code에서 WSL Remote환경을 이용하여 Java개발 환경 구성하기 (tistory.com)](https://zianlog.tistory.com/entry/VS-Code%EC%97%90%EC%84%9C-WSL-Remote%ED%99%98%EA%B2%BD%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%98%EC%97%AC-Java%EA%B0%9C%EB%B0%9C-%ED%99%98%EA%B2%BD-%EA%B5%AC%EC%84%B1%ED%95%98%EA%B8%B0)  