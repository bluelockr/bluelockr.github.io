+++
title = "VSCODE Python 개발 환경 구축 (WSL2 사용)"
summary = "How to Set up Vscode Python Dev Env using WSL2"
date = 2023-04-25T00:00:00+00:00
cover = ""
slug = "vscode_wsl2_python_dev_env"
tags = ['Windows', 'VSCODE', 'Python', 'WSL']
draft = false
+++
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
  

## ⭐소스 코드 생성

우분투 앱으로 돌아가서 다음 명령어를 입력하여 프로그래밍을 위한 폴더를 생성합니다.

```bash
mkdir Coding
cd Coding
code .
```
  
우분투 앱에서 다음 명령어를 실행하여 파이썬 관련 프로그램을 설치합니다.
  
```bash
sudo apt-get update
sudo apt-get install python3 python3-pip -y
```
  
다음 명령어를 실행하여 제대로 설치되었는지 확인합니다.
  
```bash
python3 --version
pip --version
```
  
다음 명령어를 실행하여 VSCODE를 엽니다.
  
```bash
code .
```
  
VSCODE에서 Extension 중에 **Python**을 설치합니다.  
  
파이썬 소스 파일을 생성 후 내용을 작성한 뒤 실행합니다.  
만일 실행 버튼이 나타나지 않는다면 Extension 중에서 **Run++**를 설치하면 됩니다.