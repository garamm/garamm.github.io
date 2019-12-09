---
title: "[Android/Java] 객체 배열 Intent"
categories:
  - Android/Java
---

1. 데이터 클래스 직렬화
```java
// Data.java
public class Data implements Serializable {
    ...
}
```

2. 인텐트에 데이터 담기
```java
// MainActivity.java
Intent intent = new Intent(MainActivity.this, SubActivity.class);
intent.putExtra("datas", datas);
startActivity(intent);
```

3. 데이터 받아서 사용하기
```java
// SubActivity.java
ArrayList<Data> datas = (ArrayList<Data>)getIntent().getSerializableExtra("datas");
```