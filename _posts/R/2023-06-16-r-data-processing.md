---
layout: post
title: "[R] 데이터 처리 (unique)"
categories: [R]
tags: []
date: "2023-06-16 23:13"
---

**unique: 중복된 행 제거**<br>
unique(x, incomparables = FALSE, …)
```r
x <- c(1:12, 9:11)
# [1]  1  2  3  4  5  6  7  8  9 10 11 12  9 10 11
uniqueX <- unique(x)
# [1]  1  2  3  4  5  6  7  8  9 10 11 12
```
<br>
<br>
\* 참고<br>
[rdocumentation: unique](https://www.rdocumentation.org/packages/base/versions/3.6.2/topics/unique){:target="_blank"}
