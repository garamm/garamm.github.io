---
layout: post
title: "[MySQL] MySQL 설치 및 접근 권한 설정"
category: mysql
date: "2019-07-18"
---

---
설치
------------
```
# sudo su
# apt-get update
# apt-get install mysql-server
```

---
기본 명령어
------------

MySQL 서버 시작
```
service mysql start
```

MySQL 서버 종료
```
service mysql stop
```

접속
```
mysql -u root -p
```

---
권한 변경하기
------------
> grant all privileges on *.* to root@'host명 또는 IP주소' identified by '패스워드' with grant option;
또는
> grant all privileges on *.* to root@'192.168.0.%' identified by '패스워드' with grant option;
또는
> grant all privileges on *.* to root@'%' identified by '패스워드' with grant option;

변경한 권한 적용
> flush privileges;