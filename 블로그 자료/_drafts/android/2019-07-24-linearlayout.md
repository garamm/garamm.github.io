---
layout: post
title: "[Android/Java] LinearLayout"
category: android
date: "2019-07-24"
---

---
비율에 맞게 수평 배치하기

부모뷰의 orientation을 horizontal로 지정해주고 자식뷰의 width를 0dp로, layout_weight에 비율을 설정하여 사용
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal">
 
    <TextView
        android:layout_width="0dp"
        android:layout_height="match_parent"
        android:layout_weight="1"
        android:background="#ffd900"/>
 
    <TextView
        android:layout_width="0dp"
        android:layout_height="match_parent"
        android:layout_weight="2"
        android:background="#0062ff"/>
 
    <TextView
        android:layout_width="0dp"
        android:layout_height="match_parent"
        android:layout_weight="3"
        android:background="#ff0051"/>
</LinearLayout>
```

![img2](/img/2019-07-24-linearlayout-1.png)

---
비율에 맞게 수직 배치하기

부모뷰의 orientation을 vertical로 지정해주고 자식뷰의 height를 0dp로, layout_weight에 비율을 설정하여 사용
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">
 
    <TextView
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"
        android:background="#ffd900"/>
 
    <TextView
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="2"
        android:background="#0062ff"/>
 
    <TextView
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="3"
        android:background="#ff0051"/>
</LinearLayout>

```
![img2](/img/2019-07-24-linearlayout-2.png)
