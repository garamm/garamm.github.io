---
layout: post
title: "[Window] 하드 안에 있는 파일의 삭제, 수정이 불가능할 때"
date: "2019-04-30 00:00"
updated: []
categories: ["etc"]
tags: ["window"]
---

**[1] 명령 프롬프트를 관리자 권한으로 실행**<br>
키보드의 window키를 누르고 cmd 를 입력하고 우클릭<br>
<p align="center"><img src="/assets/img/posts/cant-change-file-name.png" alt="cant-change-file-name"></p>
<br>
**[2] 다음 명령어를 실행 (ex. 하드가 D 드라이브인 경우 다음과 같이 실행)**
```bash
>> Chkdsk D: /f
```
<br>
\* 비고<br>
실행 시 오래 걸리므로 완전히 처리될 때까지 기다려야 함