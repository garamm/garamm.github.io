---
layout: post
title: "[Window] 파일 이름이 길어서 삭제가 안될 때"
date: "2019-04-30 00:00"
updated: []
categories: ["etc"]
tags: ["window"]
---

**[1] 명령 프롬프트를 관리자 권한으로 실행**<br>
키보드의 window키를 누르고 cmd 를 입력하고 우클릭<br>
<p align="center"><img src="/assets/img/posts/window-long-name.png" alt="window-long-name"></p>
<br>
**[2] 삭제하고자 하는 폴더로 이동한 후 다음과 같이 입력**
```bash
>> cd 삭제할폴더가있는경로
>> mkdir empty_dir
>> robocopy empty_dir 삭제할폴더명 /s /mir
>> rmdir empty_dir
>> rmdir 삭제할폴더명
```
<br>
\* 설명<br>
mkdir empty_dir : empty_dir 라는 이름의 빈 폴더를 생성<br>
robocopy empty_dir 삭제할폴더명 /s /mir : 삭제하고자 하는 폴더의 내용을 empty_dir로 이동<br>
rmdir empty_dir : empty_dir라는 폴더를 삭제<br>
rmdir 삭제할폴더명 : 삭제할 폴더 또한 삭제