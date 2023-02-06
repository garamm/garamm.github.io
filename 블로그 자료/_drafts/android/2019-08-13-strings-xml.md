---
layout: post
title: "[Android/Java] strings.xml"
category: android
date: "2019-08-13"
---

---
xml에 &를 입력하고 싶을 때
```
& 대신에 &amp; 로 입력해서 사용하기
```

---
Activity에서 strings.xml에 선언된 값 가져오기
```java
getString(R.string.string_name);
```
Activity가 아닌 곳에서 strings.xml에 선언된 값 가져오기
```java
Context.getString(R.string.string_name);
```

---
string.xml에 특수문자(%,&,@,# 등) 넣고싶을 때
<![CDATA[특수문자]> 형태로 입력하면 됨
```xml
<string name="question_btn_homepage_eng">Q<![CDATA[&]]>A for Homepage</string>
<string name="question_facebook_eng">- Q<![CDATA[&]]>A for Facebook</string>
<string name="question_tweeter_eng">- Q<![CDATA[&]]>A for Twitter</string>
```

---
strings.xml에 띄어쓰기 넣기
```
&#160;
```