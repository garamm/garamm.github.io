---
layout: post
title: "[Redis] 기본 명령어"
author: 임가람
date: "2019-06-17 20:24:00"
categories: [Redis]
tags: [시작하기]
---

### Intro

우리는 데이터를 MySQL, Oracle, MSSQL 등 RDB에 저장해왔습니다.<br>
그러나 데이터가 많아질수록 조회 속도가 현저히 떨어지는데요, 그래서 메모리 기반의 데이터 저장소 Redis를 사용하게 됐습니다.<br>
Redis는 메모리에 데이터를 저장하기 떄문에 조회 속도가 굉장히 빠르지만, 휘발성이기 때문에 실제 데이터를 RDB에 저장하거나 주기적인 백업을 한 후 Redis에서 조회 하는 것이 안전합니다.


### Redis 설치
```shell
sudo apt-get install redis-server
```


### 기본 명령어

Redis 서비스 실행, 중지, 재시작
```shell
brew services start redis
brew services stop redis
brew services restart redis
```

Redis 서버 실행
```shell
redis-server
```

Redis 클라이언트 접속
```shell
redis-cli
```

모든 키 조회
```shell
keys *
```
```shell
scan 0
```

특정 키 패턴으로 scan
ex. 키 패턴이 code:code1_type:y_time:2019-06-13인 경우
```shell
scan 0 match code:*_type:*_time:*
```

키 저장
```shell
set key_name value_name
```

키 확인
```shell
get key_name
```

키 삭제
```shell
del key_name
```

리스트형태 데이터 저장/추가
```shell
lpush key value
```

리스트 데이터 전체값 불러오기
```shell
lrange key 0 -1
```

모든 키 삭제
```shell
flushall
```