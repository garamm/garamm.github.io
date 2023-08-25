---
layout: post
title: "[Java] 처리 시간 구하기"
date: "2019-05-02"
updated: []
categories: [Java]
---

```java
long start = System.currentTimeMillis();

/*
 * 처리 시간을 측정할 기능 작성
*/

long end = System.currentTimeMillis();
System.out.println((end - start) / 1000.0);
```