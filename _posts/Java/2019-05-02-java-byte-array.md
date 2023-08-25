---
layout: post
title: "[Java] byte[] + byte[] byte 배열 더하기"
date: "2019-05-02"
updated: []
categories: [Java]
tags: [byte]
---

```java
byte[] firstArr = {1, 2, 3};
byte[] secondArr = {4, 5, 6};
byte[] resultArr = new byte[firstArr.length + secondArr.length];
		
System.arraycopy(firstArr, 0, resultArr, 0, firstArr.length);
System.arraycopy(secondArr, 0, resultArr, firstArr.length, secondArr.length);
		
System.out.println(Arrays.toString(resultArr));

// 출력결과 : [1, 2, 3, 4, 5, 6]
```