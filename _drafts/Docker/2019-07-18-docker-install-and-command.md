---
title: "[Docker] Docker 설치 및 기본 명령어"
categories:
  - Docker
---

설치
-------------

도커 설치
$ curl -fsSL https://get.docker.com/ | sudo sh

도커 버전 확인
$ docker version

동작중인 컨테이너 확인
$ docker ps

정지된 컨테이너 확인
$ docker ps -a

컨테이너 종료
$ docker stop container_name

컨테이너 재시작
$ docker restart container_name

컨테이너 삭제
$ docker rm container_name

컨테이너 여러개 삭제
$ docker rm container_name1, container_name2

모든 컨테이너 삭제
$ docker stop 'docker ps -a -q'
$ docker rm 'docker ps -a -q'

모든 이미지 확인
$ docker images

모든 이미지 삭제
$ docker rmi 'docker images -q'

컨테이너를 삭제하기 전에 이미지 삭제
-f 옵션을 추가하면 컨테이너도 강제 삭제
$ docker rmi -f image_name

도커허브에서 이미지 가져오기
$ docker pull image_name

도커 이미지 실행
--net=host : 아이피 설정
-i(interactive), -t(Pseudo-tty) : 실행된 Bash 셸에 입력 및 출력
-d : 컨테이너를 백그라운드로 실행
-p : 호스트 포트와 컨테이너 포트를 연결
--name : 컨테이너의 이름을 지정
$ docker run --net=host -i -t -d -p 1234:1234 --name container_name image_name

# 도커 컨테이너 접속
$ docker attach container_name

# 도커 컨테이너 접속 종료
$ exit
cf. attach로 실행한 경우 exit로 종료하면 컨테이너 자체가 종료됨
ctrl+p+q로 정리해야 컨테이너가 종료되지 않음

도커 컨테이너 접속 후 vi 안될 때
$ apt-get update
$ apt-get install vim

도커 컨테이너의 ip정보 얻기
$ docker inspect container_name