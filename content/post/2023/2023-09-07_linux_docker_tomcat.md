+++
title = "리눅스에 Docker로 Tomcat 설치하는 법"
summary = "How to install Tomcat in linux using Docker"
date = 2023-09-07T00:00:00+00:00
cover = ""
slug = "linux_tomcat_docker"
tags = ['Linux', 'Docker', 'Tomcat']
draft = false
+++
[https://www.youtube.com/watch?v=vn3aYbkLhmo](https://www.youtube.com/watch?v=vn3aYbkLhmo) 

위 영상을 참고했습니다.  
리눅스 설치 과정은 생략합니다.

<span style="color:darkcyan"> **\[우분투에 Docker 설치\]** </span>  
[https://docs.docker.com/engine/install/ubuntu/](https://docs.docker.com/engine/install/ubuntu/)

<span style="color:darkcyan"> **\[Rocky Linux에 Docker 설치\]** </span>  
[https://docs.docker.com/engine/install/centos/](https://docs.docker.com/engine/install/centos/)

Docker 설치 과정은 위 페이지를 참고해서 해주시면 됩니다.
다 설치했으면 터미널에 다음 명령어를 실행합니다.

```bash
sudo su
docker pull tomcat
docker run -d --name tomcat-container1 -p 8080:8080 tomcat
```

웹브라우저를 열어 주소창에 localhost:8080을 입력하고 엔터를 눌러서 정상적으로 접속되는지 확인합니다.


```bash
docker exec -it tomcat-container1 /bin/bash
```
  
```bash
cd webapps.dist
cp -R * ../webapps
```
(참고로 cp에서 r 옵션은 하위 디렉토리 및 파일까지 모두 복사한다는 의미입니다.)

이제 웹브라우저에 들어가서 주소창에 localhost:8080을 쳐서 엔터를 누르면 정상적으로 접속되는지 확인합니다.