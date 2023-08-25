---
layout: post
title: "[Android/Java] SharedPreferences 데이터 저장"
date: "2019-04-30 00:00"
updated: []
categories: [Android]
tags: [SharedPreferences]
---

저장
```java
SharedPreferences pref = getSharedPreferences("pref", MODE_PRIVATE);
SharedPreferences.Editor editor = pref.edit();
editor.putString("hi", "안녕하세요");
editor.commit();
```
<br>
조회
```java
SharedPreferences pref = getSharedPreferences("pref", MODE_PRIVATE);
pref.getString("hi", "");
```
<br>
삭제
```java
SharedPreferences pref = getSharedPreferences("pref", MODE_PRIVATE);
SharedPreferences.Editor editor = pref.edit();
editor.remove("hi");
editor.commit();
```
<br>
전체삭제
```java
SharedPreferences pref = getSharedPreferences("pref", MODE_PRIVATE);
SharedPreferences.Editor editor = pref.edit();
editor.clear();
editor.commit()
```