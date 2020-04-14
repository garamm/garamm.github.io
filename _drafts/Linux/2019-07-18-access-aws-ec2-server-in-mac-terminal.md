---
layout: post
title: "[Linux] 맥 터미널로 AWS EC2 서버 접속하기"
categories: Linux
---

```
# 최초 접속시 권한 변경
chmod 400 ~/Desktop/aws-key/aws-ubuntu.pem
```
```
# 해당 키를 가지고 접속 시도
ssh -i ~/Desktop/aws-key/aws-ubuntu.pem ubuntu@아이피
```