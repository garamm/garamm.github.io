---
layout: post
title: "[Android/Java] 동적으로 View의 크기 바꾸기"
category: android
date: "2019-04-30"
---

```java
ViewGroup.LayoutParams params = view.getLayoutParams();
params.width = newWidthInPx;
params.height = newHeightInPx;
view.setLayoutParams(params);
```