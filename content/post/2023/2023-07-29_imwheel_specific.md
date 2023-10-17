---
title: Ubuntu에서 imwheel 설정 특정 프로그램만 적용하기
description: How to apply imwheel settings to specific program in Ubuntu
slug: imwheel_specific_in_ubuntu
date: 2023-07-29 00:00:00+0000
image: 
tags:
    - Ubuntu
    - Imwheel
---
[https://gist.github.com/Xarkam/4b48ae5bc77876f859c0d8121bdeca84](https://gist.github.com/Xarkam/4b48ae5bc77876f859c0d8121bdeca84)

위 글에서 방법을 찾았습니다. 우분투 기준으로 서술합니다.
(This instruction is for Ubuntu.)

먼저 터미널에서 다음 명령어를 실행합니다.
(Run these commands in terminal.)

```bash
vi ~/.imwheelrc
```

그리고 다음 내용을 붙여넣어줍니다. 여기서 gwenview는 프로그램 이름입니다.
(And Paste following Lines.)

```
"^gwenview$"  
@Exclude
  
".*"  
None, Up, Button4, 3  
None, Down, Button5, 3  
None, Thumb1, Alt_L|Left  
None, Thumb2, Alt_L|Right
```

여기서 ".\*"는 모든 프로그램에 적용한다는 의미이고,
@는 imwheel 설정을 적용받지 않을 프로그램을 지정하는 것입니다.
저는 gwenview에서 한 번 스크롤 할 때마다 한 장씩 다음 이미지로 넘기려고 일부러 이렇게 설정했습니다.  
(".\*" means applying imwheel settings to all programs.
@Exclude means Not applying imwheel settings to specific application.
so in this explanation above, **gwenview** will be excluded from imwheel settings.)

이후 터미널에서 imwheel -k 명령을 실행해주면 정상적으로 적용된 것을 확인하실 수 있습니다.  
(Run imwheel -k command and check whether it works or not.)