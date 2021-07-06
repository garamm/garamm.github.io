---
layout: post
title: "[Android/Java] Activity Stack 비우기"
category: android
date: "2019-04-30"
---

```java
Intent intent = new Intent(MainActivity.this, SubActivity.class);
intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK|Intent.FLAG_ACTIVITY_CLEAR_TASK);
startActivity(intent);
```