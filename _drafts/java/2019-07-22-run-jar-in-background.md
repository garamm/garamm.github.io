---
layout: post
title: "[Java] jar 백그라운드에서 실행하기"
category: java
date: "2019-07-22"
---

```
nohup java -jar test-server-0.0.1.war &
```

비고)
프로세스 찾기 : ps –ef | grep 'java -jar test-server-0.0.1.war'
전체 프로세스 보기 : ps -efc
프로세스 종료 : kill -9 (pid)