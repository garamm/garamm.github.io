---
layout: post
title: "[Android/Java] 객체 배열 Intent"
date: "2019-04-30 00:00"
updated: []
categories: [Android]
tags: [intent, array, object]
---

**[1] 데이터 클래스 직렬화**<br>
```java
// Data.java
public class Data implements Serializable {
    ...
}
```
<br>
**[2] 인텐트에 데이터 담기**<br>
```java
// MainActivity.java
Intent intent = new Intent(MainActivity.this, SubActivity.class);
intent.putExtra("datas", datas);
startActivity(intent);
```
<br>
**[3] 데이터 받아서 사용하기**<br>
```java
// SubActivity.java
ArrayList<Data> datas = (ArrayList<Data>)getIntent().getSerializableExtra("datas");
```