---
layout: post
title: "[Redis] 기본 명령어"
date: "2019-06-17 20:24"
updated: []
categories: [Redis]
tags: [Command]
---

**설치**
```bash
sudo apt-get install redis-server
```
<br>
**기본 명령어**<br>
서비스 실행, 중지, 재시작
```bash
brew services start redis
brew services stop redis
brew services restart redis
```
<br>
**서버 실행**
```bash
redis-server
```
<br>
**클라이언트 접속**
```bash
redis-cli
```
<br>
**모든 키 조회**
```bash
keys *
또는
scan 0
```
<br>
**특정 키 패턴으로 scan**<br>
ex. 키 패턴이 code:code1_type:y_time:2019-06-13 인 경우
```bash
scan 0 match code:*_type:*_time:*
```
<br>
**키 저장/확인/삭제**
```bash
set key_name value_name
get key_name
del key_name
```
<br>
**리스트 데이터 저장/추가**
```bash
lpush key value
```
<br>
**리스트 데이터 전체 값 불러오기**
```bash
lrange key 0 -1
```
<br>
**모든 키 삭제**
```bash
flushall
```