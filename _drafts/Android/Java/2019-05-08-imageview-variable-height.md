---
layout: post
title: "[Android/Java] ImageView의 가로는 고정해놓고, 가로의 길이에 따라 세로 길이를 가변시켜야 할 때"
categories: Android Java
---

android:adjustViewBounds="true" 옵션을 추가해주면 됨
```xml
<ImageView 
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:adjustViewBounds="true"
    android:id="@+id/menuView”
    android:scaleType="fitXY"
    android:background="@drawable/ic_menu" />
```