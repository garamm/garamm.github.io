---
layout: post
title: "[Android/Java] status bar 상태바 색 변경하기"
categories: Android Java
---

```java
View view = getWindow().getDecorView();
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
    if (view != null) {
        // 23 버전 이상일 때
        view.setSystemUiVisibility(View.SYSTEM_UI_FLAG_LIGHT_STATUS_BAR);
        getWindow().setStatusBarColor(Color.parseColor("#99D258"));
    }
} else if (Build.VERSION.SDK_INT >= 21) {
    // 21 버전 이상일 때
    getWindow().setStatusBarColor(Color.parseColor("#99D258"));
}
```

![img1](/img/2019-04-30-change-statusbar-color-1.png)