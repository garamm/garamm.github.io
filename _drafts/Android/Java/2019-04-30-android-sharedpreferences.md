---
title: "[Android/Java] SharedPreferences"
categories:
  - Android/Java
---

저장
```java
SharedPreferences pref = getSharedPreferences("pref", MODE_PRIVATE);
SharedPreferences.Editor editor = pref.edit();
editor.putString("hi", "안녕하세요");
editor.commit();
```

조회
```java
SharedPreferences pref = getSharedPreferences("pref", MODE_PRIVATE);
pref.getString("hi", "");
```

삭제
```java
SharedPreferences pref = getSharedPreferences("pref", MODE_PRIVATE);
SharedPreferences.Editor editor = pref.edit();
editor.remove("hi");
editor.commit();
```

전체삭제
```java
SharedPreferences pref = getSharedPreferences("pref", MODE_PRIVATE);
SharedPreferences.Editor editor = pref.edit();
editor.clear();
editor.commit()
```