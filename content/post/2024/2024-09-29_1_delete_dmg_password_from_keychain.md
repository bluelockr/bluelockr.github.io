---
title: "키체인에 등록된 dmg 파일 비밀번호 제거하는 법"
summary: "How to delete dmg file password from Keychain"
description: "delete_dmg_password_from_keychain.md"
draft: false
date: 2024-09-29T00:00:00.000Z
slug: "delete_dmg_password_from_keychain.md"
tags:
  - Application
---

```bash
# 환경(Environment)
OS : MacOS Sequoia 15.0
```

<img style='border-radius: 10px' src="/../../images/2024/2024-09-29_1_delete_dmg_password_from_keychain/1.png" width="400">  
<br>

보시는 바와 같이 dmg 파일에 비밀번호를 설정해놓고, 키체인에 이를 저장해둘 수 있습니다.  
하지만 이 비밀번호를 다시 키체인에서 제거하고 싶다면 어떻게 해야할까요.  
Keychain Access에서 dmg를 검색한 뒤 찾아서 삭제해주면 됩니다.  

(As you can see image above, you can set password to dmg file and save it to keychain.  
But what if you wanna get rid of that password from keychain?  
All you need to do is find 'dmg' from Keychain Access and delete it.)  

<img style=' border-radius: 10px' src="/../../images/2024/2024-09-29_1_delete_dmg_password_from_keychain/2.png" width="600">  
<br>

먼저 Spotlight에서 'KeyChain Access'를 검색하여 실행한 뒤
(주로 CMD+SpaceBar 또는 F3키를 누르면 됩니다.)
(First of All, PRESS CMD+SpaceBar or F3 to run Spotlight and search for 'Keychain Access'.)

<img style='border-radius: 10px' src="/../../images/2024/2024-09-29_1_delete_dmg_password_from_keychain/3.png" width="400">  
<br>

<img style='border-radius: 10px' src="/../../images/2024/2024-09-29_1_delete_dmg_password_from_keychain/4.png" width="400">  
<br>

<img style='border-radius: 10px' src="/../../images/2024/2024-09-29_1_delete_dmg_password_from_keychain/5.png" width="800">  
<br>

dmg라고 검색해 해당 패스워드를 찾아낸 뒤 삭제해줍니다.  
(And search for 'dmg' in the Keychain Access App and delete it.)

Reference :   
[https://www.youtube.com/watch?v=Z9IMetrM2SQ](https://www.youtube.com/watch?v=Z9IMetrM2SQ)