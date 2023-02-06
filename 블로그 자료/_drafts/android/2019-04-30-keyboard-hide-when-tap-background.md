---
layout: post
title: "[Android/Java] 배경 터치 했을 때 소프트 키보드 내리기"
category: android
date: "2019-04-30"
---

activity_main.xml
```xml
<!-- 최상위 뷰 터치시 키보드를 내릴 수 있도록 이벤트  -->
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:orientation="vertical"
    android:gravity="center"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:id="@+id/background">
 
    <EditText
        android:layout_width="match_parent"
        android:inputType="text"
        android:maxLines="1"
        android:layout_height="wrap_content" />
</LinearLayout>
```


MainActivity.java
```java
public class MainActivity extends AppCompatActivity {
 
    LinearLayout mainBackground;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
 
        mainBackground = (LinearLayout) findViewById(R.id.background);
        mainBackground.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                InputMethodManager imm = (InputMethodManager) getSystemService(Context.INPUT_METHOD_SERVICE);
                imm.hideSoftInputFromWindow(mainBackground.getWindowToken(), 0);
            }
        });
    }
}
```