---
layout: post
title: "[Android/Java] Activity Stack 비우기"
categories: Android Java
---

```java
Intent intent = new Intent(MainActivity.this, SubActivity.class);
intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK|Intent.FLAG_ACTIVITY_CLEAR_TASK);
startActivity(intent);
```