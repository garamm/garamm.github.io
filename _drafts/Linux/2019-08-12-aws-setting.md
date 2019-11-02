---
layout: posts
title: "[AWS] 아마존 웹 서버 생성 후 설정하기"
comments: true
categories: [AWS]
---

1) 서버 생성 후 '실행 중인 인스턴스' 클릭
![img1](/img/2019-08-12-aws-setting-1.png)

2) 생성된 인스턴스 선택
![img2](/img/2019-08-12-aws-setting-2.png)

3) 서버 정보 조회
![img3](/img/2019-08-12-aws-setting-3.png)

4) 탄력적 IP 설정: 공인 IP 할당
![img4](/img/2019-08-12-aws-setting-4.png)

5) 보안 그룹 설정: 서버에 접근할 수 있는 IP, Port 범위 설정
![img5](/img/2019-08-12-aws-setting-5.png)

6) 인바운드 설정
![img6](/img/2019-08-12-aws-setting-6.png)
![img7](/img/2019-08-12-aws-setting-7.png)

7) 아웃바운드 설정
![img8](/img/2019-08-12-aws-setting-8.png)


// 참고
1) visual studio code에서 ftp-simple.json을 사용하여 접속할 때 pem 파일 경로를 추가해줘야 함
```json
"privatekey": "/Users/hanul/Desktop/aws-key/aws-ubuntu.pem"
```

2) mac의 터미널에서 접속하고 싶을 때
키 파일의 권한을 변경해준다.
```
chmod 400 ~/Desktop/aws-key/aws-ubuntu.pem
```
다음 명령어를 통해 접속한다.
```
ssh -i ~/Desktop/aws-key/aws-ubuntu.pem ubuntu@[서버 아이피 또는 도메인]
```