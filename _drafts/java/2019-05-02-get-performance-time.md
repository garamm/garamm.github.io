---
layout: post
title: "[Java] 처리 시간 구하기"
category: java
date: "2019-05-02"
---


```java
long start = System.currentTimeMillis();

// 처리시간을 측정할 기능 작성

long end = System.currentTimeMillis();
Log.i("tagssss", "처리 시간 : " + (end - start) / 1000.0);
```