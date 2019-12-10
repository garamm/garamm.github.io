---
layout: post
title: "[Linux] 맥 터미널에서 brew 사용하여 설치시 Permission 에러 해결하기"
categories: Linux
---

터미널에서 brew를 사용하여 어플리케이션 설치할 때
Error: Permission denied @ dir_s_mkdir - /usr/local/Frameworks
요런 에러가 발생한다면 어플리케이션이 설치 될 Path에 아래와 같이 권한을 부여해줘야 함
```
> sudo install -d -o $(whoami) -g admin /usr/local/Frameworks
Password: 비밀번호
> brew install carthage
```