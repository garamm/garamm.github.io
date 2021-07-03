---
layout: post
title: "[Python 3] 내장 함수 정리"
author: 임가람
date: "2021-06-28 20:57:00"
categories: [Python3]
tags: [내장함수]
---

### Intro

저는 Python을 따로 프로젝트에서 사용하지는 않았는데 알고리즘 공부나 코딩 테스트에서 쓰기 유용해서 이참에 한 번 정리해보려고 합니다.<br>
이번 글은 Python 3에서 제공하는 내장 함수들인데요. 제가 문제 풀면서 유용하게 썼던 함수들 기준으로 차근 차근 정리할 예정입니다 ㅎㅎ

### 내장함수 목록

enumerate()<br>
for문과 자주 쓰이는 함수, for문에서 이 값이 몇 번째 값인지 확인하고 싶을 때 사용
```python
children = ["박해영", "차수현", "이재한"]
for idx, child in enumerate(children):
    print("%d번 학생은 %s 입니다." % (idx, child))
```
<br>

str()<br>
문자열 형태로 객체를 반환
```python
print(str(123456)) # 결과 : '123456'
```
<br>

ord() : 문자를 아스키 코드로 변환<br>
chr() : 아스키 코드 값을 문자로 변환
```python
print(ord("A")) # 출력 : 65
print(chr(65)) # 출력 : 'A'
```
<br>

\# 참고
[python 3 공식 문서](https://docs.python.org/ko/3/library/functions.html){:target="_blank"}<br>

