---
layout: post
title: "[Android/Java] 다른 Activity에 있는 함수 호출"
categories: Android Java
---

1. 함수가 있는 Activity에 다음과 같이 설정
```java
public class MainActivity extends AppCompatActivity {
 
    public static Context mContext;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
 
        mContext = this;
        Intent intent = new Intent(MainActivity.this, Main2Activity.class);
        startActivity(intent);
    }
 
    public void sendText() {
        Log.i("resultTag", "active!!");
    }
}
```

2. 다른 Activity의 함수를 호출하는 Activity에서 다음과 같이 호출 
```java
public class Main2Activity extends AppCompatActivity {
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main2);
 
        MainActivity mainActivity = ((MainActivity)MainActivity.mContext);
        mainActivity.sendText();
 
    }
}
```