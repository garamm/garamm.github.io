---
layout: post
title: "[Android/Java] View의 width, height 구하기"
category: android
date: "2019-05-08"
---

1. Activity에서 View의 width, height 구하기
화면의 포커스가 변경될 때 onCreate - onWindowFocusChanged 이 순서로 호출된다.
onCreate에서 화면의 width, height를 구하면 view가 읽혀지기도 전에 width, height를 구해달라고 요청하는 것이기 때문에 출력해보면 결과값이 모두 0으로 나온다. 
따라서 화면의 포커스가 바뀐 후 width, height를 구해야 하기 때문에.  
onCreate가 아닌 onWindowFocusChanged 여기서 view의 width, height를 구해야 한다.
```java
@Override
public void onWindowFocusChanged(boolean hasFocus) {
    super.onWindowFocusChanged(hasFocus);
    width = view.getWidth();
    height = view.getHeight();
}
```


2. Fragment에서 View의 width, height 구하기
```java
ViewTreeObserver viewTreeObserver = myView.getViewTreeObserver();
viewTreeObserver.addOnGlobalLayoutListener(new ViewTreeObserver.OnGlobalLayoutListener() {
    @Override
    public void onGlobalLayout() {
        height = myView.getHeight();
    }
});
```
참고) width, height 값이 계속 0인 경우 timertask, timer로 약간의 delay를 주면 값을 얻을 수 있다.