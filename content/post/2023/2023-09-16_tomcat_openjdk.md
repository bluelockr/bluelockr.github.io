---
title: 리눅스에 Tomcat 설치하는 법(feat.OpenJDK21)
description: How to Install Tomcat in Linux with OpenJDK21
slug: linux_tomcat_openjdk_installation
date: 2023-09-16 00:00:00+0000
image: 
tags:
    - Linux
    - OpenJDK
    - Tomcat
published : false
---

우분투와 Rocky 리눅스에서 Tomcat을 로컬로 설치해보았습니다.


## ⭐OpenJDK 설치

```bash
sudo su
wget https://download.java.net/java/GA/jdk21/fd2272bbf8e04c3dbaee13770090416c/35/GPL/openjdk-21_linux-x64_bin.tar.gz
tar -xzvf openjdk-21_linux-x64_bin.tar.gz -C /
```


## ⭐자바 환경설정

```bash
export JAVA_HOME=/jdk-21
export PATH=$PATH:$JAVA_HOME/bin
java -version
javac -version
```


## ⭐tomcat용 그룹/사용자 생성

```bash
groupadd -g 10000 tomcat
useradd -u 10000 -g 10000 -d /home/tomcat -s /bin/false -mk /etc/skel tomcat
```
  
  
## ⭐tomcat 설치 및 실행

```bash
#여기서 ubuntu는 사용자명으로, 사람마다 다름.
cd /home/ubuntu
wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.13/bin/apache-tomcat-10.1.13.tar.gz
tar -zxvf apache-tomcat-10.1.13.tar.gz
chown -R tomcat:tomcat apache-tomcat-10.1.13
sh /home/ubuntu/apache-tomcat-10.1.13/bin/startup.sh
```

\*여기서 ubuntu는 유저명으로, PC마다 다릅니다.

다 마쳤으면 웹브라우저를 열어서 **localhost:8080**을 입력하고 엔터를 눌러 정상적으로 접속되는지 확인합니다.  
8000번이 아니라 8080번입니다!


참고한 글 :

[https://blog.naver.com/PostView.naver?blogId=oxcow119&logNo=222865174811&categoryNo=0&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=postView](https://blog.naver.com/PostView.naver?blogId=oxcow119&logNo=222865174811&categoryNo=0&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=postView)

[https://tomcat.apache.org/download-10.cgi](https://tomcat.apache.org/download-10.cgi)

[https://jin2rang.tistory.com/entry/%ED%86%B0%EC%BA%A3%EC%84%9C%EB%B2%84-%EC%84%A4%EC%A0%95-%EB%B0%8F-%EC%8B%A4%ED%96%89%ED%95%98%EA%B8%B0-A-to-Z](https://jin2rang.tistory.com/entry/%ED%86%B0%EC%BA%A3%EC%84%9C%EB%B2%84-%EC%84%A4%EC%A0%95-%EB%B0%8F-%EC%8B%A4%ED%96%89%ED%95%98%EA%B8%B0-A-to-Z)