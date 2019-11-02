---
layout: posts
title: "[Java] 처리 시간 구하기"
comments: true
categories: [Java]
---


```java
long start = System.currentTimeMillis();

// 처리시간을 측정할 기능 작성

long end = System.currentTimeMillis();
Log.i("tagssss", "처리 시간 : " + (end - start) / 1000.0);
```