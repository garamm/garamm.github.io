---
layout: post
title: "[Docker] Docker 설치 및 기본 명령어"
date: "2019-07-18 22:00"
updated: []
categories: [Docker]
tags: [Command]
---

**설치**
```bash
curl -fsSL https://get.docker.com/ | sudo sh
```
<br>
**버전 확인**
```bash
docker version
```
<br>
**동작중인 컨테이너 확인**
```bash
docker ps
```
<br>
**정지된 컨테이너 확인**
```bash
docker ps -a
```
<br>
**컨테이너 종료**
```bash
docker stop container_name
```
<br>
**컨테이너 재시작**
```bash
docker restart container_name
```
<br>
**컨테이너 삭제**
```bash
docker rm container_name
```
<br>
**컨테이너 여러개 삭제**
```bash
docker rm container_name1, container_name2
```
<br>
**모든 컨테이너 삭제**
```bash
docker stop 'docker ps -a -q'
docker rm 'docker ps -a -q'
```
<br>
**모든 이미지 확인**
```bash
docker images
```
<br>
**모든 이미지 삭제**
```bash
docker rmi 'docker images -q'
```
<br>
**이미지 삭제**<br>
컨테이너를 삭제하기 전에 작업<br>
-f 옵션을 추가하면 컨테이너도 강제 삭제
```bash
docker rmi -f image_name
```
<br>
**도커허브에서 이미지 가져오기**
```bash
docker pull image_name
```
<br>
**도커 이미지 실행**<br>
--net=host : 아이피 설정<br>
-i(interactive), -t(Pseudo-tty) : 실행된 bash shell에 입력, 출력<br>
-d : 컨테이너를 백그라운드로 실행<br>
-p : 호스트 포트와 컨테이너 포트를 연결<br>
--name : 컨테이너의 이름을 지정
```bash
# example
docker run --net=host -i -t -d -p 1234:1234 --name container_name image_name
```
<br>
**도커 컨테이너 접속**
```bash
docker attach container_name
또는
docker exec -it container_name bash
```
<br>
**도커 컨테이너 접속 종료**
```bash
exit
```
`attach`로 실행한 경우 `exit`로 종료하면 컨테이너 자체가 종료됨<br>
`ctrl + p + q`로 종료해야 컨테이너가 종료되지 않음<br>
<br>
**도커 컨테이너 접속 후 vi 안될 때**<br>
container 안에서 vi 설치 진행
```bash
apt-get update
apt-get install vim
```
<br>
**도커 컨테이너의 ip정보 얻기**
```bash
docker inspect container_name
```