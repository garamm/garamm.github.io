---
layout: post
title: "[Java] jar 백그라운드에서 실행하기"
date: "2019-07-22"
updated: []
categories: [Java]
tags: [run]
---

**백그라운드에서 실행**
```bash
nohup java -jar [파일명] &
ex) nohup java -jar test-server-0.0.1.war &
```
<br>
**프로세스 찾기**
```bash
ps –ef | grep '[파일명]'
ex) ps –ef | grep 'java -jar test-server-0.0.1.war'
```
<br>
**전체 프로세스 보기**
```bash
ps -efc
```
<br>
**프로세스 종료**
```bash
kill -9 [pid]
```