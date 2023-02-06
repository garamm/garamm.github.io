---
layout: post
title: "[Android/Java] 잠자는 화면 깨우기"
category: android
date: "2019-05-08"
---

```java
getWindow().addFlags(
      // 화면이 잠겨있을 때 보여주기
      WindowManager.LayoutParams.FLAG_SHOW_WHEN_LOCKED
      // 키잠금 해제하기
      | WindowManager.LayoutParams.FLAG_DISMISS_KEYGUARD
      // 화면 켜기
      | WindowManager.LayoutParams.FLAG_TURN_SCREEN_ON);
```