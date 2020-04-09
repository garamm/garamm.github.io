---
layout: post
title: "[Android/Java] View의 절대좌표 구하기"
categories: Android Java
---

```java
Rect r = new Rect();
btn.getGlobalVisibleRect(r);
String s = "btn의 절대좌표 : r.left=" + r.left + ", r.top=" + r.top + ", r.right=" + r.right + ", r.bottom=" + r.bottom;
```


* 중요
View가 생성된 다음에 해당 View의 좌표를 구해야 하므로 
값이 0,0,0,0으로 나오는 경우 click event 안에 넣거나
Activity의 경우 onWindowFocusChanged를 override 하여 그 안에서 좌표 구하는 함수를 호출
Fragment의 경우 ViewTreeObserver를 override 하여 그 안에서 좌표 구하는 함수를 호출해야 한다.  
