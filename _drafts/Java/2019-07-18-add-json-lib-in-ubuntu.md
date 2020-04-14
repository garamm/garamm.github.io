---
layout: post
title: "[Java] Ubuntu에서 json 라이브러리 적용하기"
categories: Java
---

1. jar 파일 다운로드
```
wget https://storage.googleapis.com/google-code-archive-downloads/v2/code.google.com/json-simple/json-simple-1.1.1.jar
```

2. 다음 디렉토리 안에 jar 파일 넣기(java 프로젝트가 아니라 해당 경로에 추가해야 정상 작동됨)
```
/usr/lib/jvm/java-8-openjdk-amd64/jre/lib/ext
```