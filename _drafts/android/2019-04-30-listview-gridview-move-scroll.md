---
layout: post
title: "[Android/Java] ListView, GridView에서 스크롤 이동"
category: android
date: "2019-04-30"
---

---
원하는 위치로 이동

방법 1
```java
listView.requestFocusFromTouch();
listAdapter.notifyDataSetChanged();
listView.setSelection(position);
```

방법 2
```java
listView.smoothScrollToPosition(position);
```


---
스크롤의 가장 하단으로 이동  

방법 1. 자바 코드로 적용
```java
listView.setTranscriptMode(ListView.TRANSCRIPT_MODE_ALWAYS_SCROLL);
```

방법 2. xml에서 해당 뷰에 속성 추가
```xml
android:transcriptMode="alwaysScroll"
```

방법 3. view의 selection 이용하기
```java
private void scrollMyListViewToBottom() {
    listView.post(new Runnable() {
        @Override
        public void run() {
            listView.setSelection(listAdapter.getCount() - 1);
        }
    });
}
```