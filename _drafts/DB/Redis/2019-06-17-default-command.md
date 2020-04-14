---
layout: post
title: "[Redis] Redis 기본 명령어"
categories: Redis
---

---
Redis 설치
------------
$ sudo apt-get install redis-server

---
기본 명령어
------------
Redis 서비스 실행, 중지, 재시작
$ brew services start redis
$ brew services stop redis
$ brew services restart redis

Redis 서버 실행
$ redis-server

Redis 클라이언트 접속
$ redis-cli

모든 키 조회
> keys *
또는
> scan 0

특정 키 패턴으로 scan
// 키 패턴이 code:code1_type:y_time:2019-06-13인 경우
> scan 0 match code:*_type:*_time:*

키 저장
> set key_name value_name

키 확인
> get key_name

키 삭제
> del key_name

리스트형태 데이터 저장/추가
> lpush key value

리스트 데이터 전체값 불러오기
> lrange key 0 -1

모든 키 삭제
> flushall