---
layout: post
title: "[Android/Java] 디바이스의 해상도 확인(폴더 구분)"
category: android
date: "2019-04-30"
---

```java
if ((getResources().getConfiguration().screenLayout & Configuration.SCREENLAYOUT_SIZE_MASK) == Configuration.SCREENLAYOUT_SIZE_LARGE) {
    textView.setText("LARGE");
} else if ((getResources().getConfiguration().screenLayout & Configuration.SCREENLAYOUT_SIZE_MASK) == Configuration.SCREENLAYOUT_SIZE_XLARGE) {
    textView.setText("XLARGE");
} else if ((getResources().getConfiguration().screenLayout & Configuration.SCREENLAYOUT_SIZE_MASK) == Configuration.SCREENLAYOUT_SIZE_NORMAL) {
    textView.setText("NORMAL")
```


```java
int density= getResources().getDisplayMetrics().densityDpi;
switch(density) {
    case DisplayMetrics.DENSITY_LOW:
        Toast.makeText(context, "LDPI", Toast.LENGTH_SHORT).show();
        break;
    case DisplayMetrics.DENSITY_MEDIUM:
        Toast.makeText(context, "MDPI", Toast.LENGTH_SHORT).show();
        break;
    case DisplayMetrics.DENSITY_HIGH:
        Toast.makeText(context, "HDPI", Toast.LENGTH_SHORT).show();
        break;
    case DisplayMetrics.DENSITY_XHIGH:
        Toast.makeText(context, "XHDPI", Toast.LENGTH_SHORT).show();
        break;
}
```