---
layout: post
title: "[R] 변수 (assign, get)"
categories: [R]
tags: [file]
date: "2023-06-17 00:57"
updated: [variable]
---

**assign: 코드로 변수 생성**<br>
assign(x, value)<br>
**get: 변수의 값 조회**<br>
get(x)<br>
**exists: 변수가 존재하는지 확인**<br>
exists(x)
```R
assign('x', 10)
get('x') # [1] 10
exists('x') # [1] TRUE
```
<br>
**ls: 정의된 모든 변수 조회**<br>
ls()<br>
**rm: 정의된 변수 삭제**<br>
rm()
```R
for (i in 1:3) {
    assign(paste0('x', i), i)
}
ls() # [1] "i"  "x1" "x2" "x3"

rm(list=ls())
ls() # character(0)
```
<br>
<br>
\* 참고<br>
[rdocumentation: assign](https://www.rdocumentation.org/packages/base/versions/3.6.2/topics/assign){:target="_blank"}<br>
[Kamang's IT Blog: [R-28]변수 생성(assign)과 변수 사용(get)](https://kamang-it.tistory.com/entry/R-28변수-생성assign과-변수-사용get){:target="_blank"}<br>
[Stackoverflow: How to check if object (variable) is defined in R?](https://stackoverflow.com/questions/9368900/how-to-check-if-object-variable-is-defined-in-r){:target="_blank"}<br>
[R 기초: R 에서 정의된 모든 변수를 한번에 삭제하는 방법](https://rbasall.tistory.com/114){:target="_blank"}<br>