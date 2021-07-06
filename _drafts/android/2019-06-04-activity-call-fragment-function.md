---
layout: post
title: "[Android/Java] Activity에서 Fragment, Fragment에서 Activity 메소드 호출"
category: android
date: "2019-06-04"
---

Activity에서 Fragment의 메소드 호출
```java
FragmentManager fm = getSupportFragmentManager();
MainFragment fragment = (MainFragment)fm.findFragmentById(R.id.main_fragment);
fragment.method();
```

Fragment에서 Activity의 메소드 호출
```java
((MainActivity)getActivity()).method();
```