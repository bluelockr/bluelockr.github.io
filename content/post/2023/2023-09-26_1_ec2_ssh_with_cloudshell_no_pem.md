+++
title = "CloudShell로 EC2 인스턴스 pem키 없이 SSH 접속하기"
summary = "SSH Connection to EC2 Instance without pem key using Cloudshells"
date = 2023-09-26T00:00:00+00:00
cover = ""
slug = "ec2_ssh_with_cloudshell_no_pem"
tags = ['Network', 'AWS']
draft = false
+++
AWS EC2 인스턴스에 SSH 접속하려고 드니까 보안 관련된 파일들이 csv 파일로 내려받아지길래 기겁했습니다.  
CSV 파일은 심지어 패스워드도 안 걸리더라구요.

하지만 AWS CLI를 이용하려면 어쩔 수 없는 부분이라 보안상 좀 더 좋은 방법은 없을까 싶어 여러가지 방법을 강구해보는데, 
마침 AWS에서 Cloudshell이라는 온라인 쉘 기능을 제공하길래 AWS 관리 콘솔에서 모든 걸 다 해결할 수 있다는 점이 매력적이어서 시도해보게 되었습니다.

[https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/cloudshell-ssh.html](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/cloudshell-ssh.html)
  
[https://hoing.io/archives/10257](https://hoing.io/archives/10257)  
  
위 2개 글을 참고했습니다.


## ⭐EC2 인스턴스 생성

**먼저 EC2 인스턴스를 생성해줘야 합니다.**  

EC2 메뉴를 찾아들어가 다음과 같이 진행해줍니다.

* Quick Start : Ubuntu  
* 키 페어 : (안함)  
* 방화벽(보안그룹) : 보안그룹 생성 & SSH 트래픽 허용(위치 무관)  
* '키 페어 없이 계속 진행'  
  
이런 설정을 적용해 EC2 인스턴스를 생성해주면 자동으로 실행됩니다.  
보통은 키 페어를 생성해주지만, 이러면 로컬에 pem 파일이 자동적으로 다운로드되므로 로컬에 흔적이 남게 됩니다.
AWS 내에서만 처리해주려면 다른 방법으로 진행해야 합니다.

## ⭐EC2 Instance Connect를 사용하여 연결

[https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/ec2-instance-connect-methods.html](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/ec2-instance-connect-methods.html)  
▲ AWS Document 공식 자료 참고
  
EC2 -> 인스턴스 -> 연결  
-> EC2 인스턴스 연결    
-> **EC2 Instance Connect을 사용하여 연결**  
-> 사용자 이름 : ubuntu
-> 연결

이렇게 하면 AWS 관리 콘솔에서 EC2 인스턴스로 직접 연결하게 됩니다.  
연결이 성공하면 다음과 같이 입력합니다.

```bash
sudo -i
passwd
(패스워드 입력)
(패스워드 다시 한 번 입력)
#passwd: password updated successfully라는 문구가 나오면 성공
```

## ⭐Cloudshell 실행

이제 웹브라우저에서 새 탭을 열어 AWS 관리콘솔 중 아무 페이지나 들어가보면 상단에 콘솔 같은 아이콘이 보입니다.  
클릭하여 cloudshell을 열어주고 전체 화면으로 키워주면 cloudshell이 새 탭으로 열립니다.  
다음과 같이 입력해줍니다.
```bash
mkdir .ssh
chmod 700 .ssh
ssh-keygen -t rsa -b 4096
엔터 3번
cd .ssh
cat id_rsa.pub
```
마지막 명령어를 치면 공개키 파일의 내용이 보일겁니다.  
Ctrl+C로 복사해두고, **EC2 인스턴스 콘솔 창**으로 돌아가서 다음과 같이 입력해줍니다.  

## ⭐authorized_keys 파일 설정

```bash
cd .ssh
vi authorized_keys
```
이제 아까 Ctrl+C로 복사해뒀던 공개키 파일의 내용을 Ctrl+V로 붙여넣고 저장해줍니다.

## ⭐SSH 연결

EC2 -> 인스턴스 -> 연결  -> SSH 클라이언트 메뉴에 들어가,  
맨 아래에 ssh -i "id_rsa" 어쩌고 하는 명령줄을 복사해서 cloudshell에서 그대로 실행합니다.  
암호 입력 없이도 SSH 접속이 성공적으로 되는 것을 확인하실 수 있습니다.

## ⭐그 외

지금까지는 Cloudshell을 이용한 방법을 알려드렸는데요,  
방법을 더 찾아보니 AWS Session Manager를 사용하는 방법도 있습니다.  
세션 관리자를 사용하는 방법은 아래 유튜브 링크에 잘 나와있으니 이걸 참고하시면 됩니다.  
[https://www.youtube.com/watch?v=2Xb2JXV5Llo](https://www.youtube.com/watch?v=2Xb2JXV5Llo)