---
layout: post
title: "[Java] byte[] + byte[]"
category: java
date: "2019-05-02"
---


```java
byte[] ciphertext = {1, 2, 3};
byte[] mac = {4, 5, 6};
byte[] destination = new byte[ciphertext.length + mac.length];
		
System.arraycopy(ciphertext, 0, destination, 0, ciphertext.length);
System.arraycopy(mac, 0, destination, ciphertext.length, mac.length);
		
System.out.println(Arrays.toString(destination));

// 출력결과 : [1, 2, 3, 4, 5, 6]
```