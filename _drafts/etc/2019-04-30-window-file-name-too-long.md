---
layout: post
title: "Window 파일 이름이 길어서 삭제가 안될 때"
category: "etc"
date: "2019-04-30"
---


1. 관리자 모드로 cmd를 연다   

2. 다음과 같이 입력한다.   
```
>> mkdir empty_dir
>> robocopy empty_dir 삭제할폴더명 /s /mir
>> rmdir empty_dir
>> rmdir 삭제할폴더명
```


* 설명  
mkdir empty_dir : empty_dir 라는 이름의 빈 폴더를 생성합니다.  
robocopy empty_dir 삭제할폴더명 /s /mir : 삭제하고자 하는 폴더의 내용을 empty_dir로 이동시킵니다.  
rmdir empty_dir : empty_dir라는 폴더를 삭제합니다.  
rmdir 삭제할폴더명 : 삭제할 폴더 또한 삭제합니다.  