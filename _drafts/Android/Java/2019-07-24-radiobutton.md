---
layout: post
title: "[Android/Java] RadioButton"
categories: Android Java
---

---
기본 RadioButton 색 바꾸기

```xml
<RadioButton
    android:layout_width="match_parent"
    android:text="first"
    android:layout_marginBottom="10dp"
    android:id="@+id/firstRadio"
    android:layout_height="wrap_content" />
    
<RadioButton
    android:layout_width="match_parent"
    android:text="second"
    android:id="@+id/secondRadio"
    android:buttonTint="#00aaff"
    android:layout_height="wrap_content" />
```
![img1](/img/2019-07-24-radiobutton-1.png)

---
라디오버튼 커스터마이징하기

radio_selector.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">    
    <item android:state_checked="false" android:drawable="@drawable/체크안된이미지" />
    <item android:state_checked="true" android:drawable="@drawable/체크된이미지" />
</selector>
```

activity_main.xml
```xml
<?xml version="1.0"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
 
        <RadioGroup
            android:id="@+id/rg_line1"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:orientation="horizontal">
 
            <RadioButton
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:id="@+id/radio1"
                android:button="@null"
                android:drawableLeft="@drawable/radio_selector"
                android:drawablePadding="8dp"
                android:padding="8dp"
                android:text="option1" />
 
            <RadioButton
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginLeft="20dp"
                android:id="@+id/radio2"
                android:button="@null"
                android:drawableLeft="@drawable/radio_selector"
                android:drawablePadding="8dp"
                android:padding="8dp"
                android:text="option2" />
        </RadioGroup>
        
</LinearLayout>
```