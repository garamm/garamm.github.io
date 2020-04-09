---
layout: post
title: "[Android/Java] 디바이스의 화면 크기 가져오기"
categories: Android Java
---

```java
DisplayMetrics dm = getApplicationContext().getResources().getDisplayMetrics();
int width = dm.widthPixels;
int height = dm.heightPixels;
```