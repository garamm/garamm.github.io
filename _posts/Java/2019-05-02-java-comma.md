---
layout: post
title: "[Java] 숫자에 콤마 찍기(금액표시)"
date: "2019-05-02"
updated: []
categories: [Java]
tags: [timer]
---

```java
int num = 1000;
String numStr = String.format("%,d", num);
System.out.println(numStr); // 1,000
```