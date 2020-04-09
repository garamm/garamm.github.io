---
layout: post
title: "[Java] 반올림"
categories: Java
---


```java
int a = 4550;
int b = 4550;
int c = 4440;
int d = 4440;

a = (int) Math.round(a/100.0)*100;
System.out.println(a);
b = (int) Math.round(b/1000.0)*1000;
System.out.println(b);
c = (int) Math.round(c/100.0)*100;
System.out.println(c);
d = (int) Math.round(d/1000.0)*1000;
System.out.println(d);

// 출력 결과
// 4600
// 5000
// 4400
// 4000

```