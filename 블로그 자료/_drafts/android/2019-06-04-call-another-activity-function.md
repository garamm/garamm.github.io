---
layout: post
title: "[Android/Java] 다른 Activity에 있는 함수 호출"
category: android
date: "2019-06-04"
---

함수가 있는 액티비티에 설정
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
    public String sendText() {
        return "testString";
    }
}
```


함수를 가져와서 쓸 액티비티에 호출
```java
MainActivity mainActivity = ((MainActivity)MainActivity.mContext);
mainActivity.sendText();
```