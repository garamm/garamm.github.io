---
layout: post
title: "[Android/Java] xml drawable"
categories: Android Java
---

---
각종 모양 만들기

rectangle.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle">
    <!-- 채우기 -->
    <solid
        android:color="#ccfaf5" />
    <!-- 테두리 두께 및 색 -->
    <stroke
        android:width="5dip"
        android:color="#00aeff" />
    <!-- 모서리 둥근 정도 -->
    <corners
        android:radius="5dip" />
</shape>
```
shadow.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
    <item>
        <shape android:shape="rectangle">
            <solid android:color="#CABBBBBB"/>
            <corners android:radius="2dp" />
        </shape>
    </item>
    <item
        android:left="0dp"
        android:right="0dp"
        android:top="0dp"
        android:bottom="2dp">
        <shape android:shape="rectangle">
            <solid android:color="@android:color/white"/>
            <corners android:radius="2dp" />
        </shape>
    </item>
</layer-list>
```
oval.xml
```xml
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="oval">
    <solid
        android:color="#FFFF0000"/>
    <corners android:radius="4dp" />
    <gradient
        android:startColor="#FFFF0000"
        android:endColor="#80FF00FF"
        android:angle="270"/>
</shape>
```
![img1](/img/2019-08-13-xml-drawable-1.png)

---
가로, 세로 점선 만들기

가로
```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="line">
    <stroke
        android:width="1dp"
        android:color="#FF0000FF"
        android:dashGap="8dp"
        android:dashWidth="4dp" />
</shape>
```
세로
```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle">
    <solid android:color="@android:color/transparent" />
    <stroke
        android:width="1dp"
        android:color="#60000000"
        android:dashGap="2dp"
        android:dashWidth="2dp" />
</shape>
```
![img2](/img/2019-08-13-xml-drawable-2.png)

---
위에만 radius 주기
```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle" >
    <solid android:color="#0177d9" />
    <corners
        android:topLeftRadius="10dp"
        android:topRightRadius="10dp" />
</shape>
```
![img3](/img/2019-08-13-xml-drawable-3.png)


---
우측을 제외한 나머지 테두리 
```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
    <item>
        <shape android:shape="rectangle" >
            <solid android:color="#0177d9" />
            <corners
                android:bottomLeftRadius="3dp"
                android:radius="0.1dp"
                android:topLeftRadius="3dp" />
            </shape>
    <item>
    <item
        android:bottom="3px"
        android:left="3px"
        android:top="3px">
        <shape android:shape="rectangle">
            <solid android:color="#ffffff" />
            <corners android:radius="0.1dp" />
        </shape>
    </item>
</layer-list>
```
![img4](/img/2019-08-13-xml-drawable-4.png)

---
안이 투명한 사각형 만들기
```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android">
    <corners android:radius="1dp" />
    <solid android:color="@android:color/transparent" />
    <stroke android:width="1dp" android:color="#000000" />
</shape>
```
![img5](/img/2019-08-13-xml-drawable-5.png)


---
gradient의 angle 옵션에 따른 그라데이션 각도 변화
![img6](/img/2019-08-13-xml-drawable-6.png)


---
xml로 만든 drawable의 gradient의 color를 java 코드로 변경하는 방법

test_drawable.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle">
    <gradient
        android:angle="270"
        android:endColor="#000"
        android:startColor="#fff"/>
</shape>
```

activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center">

    <ImageView
        android:id="@+id/image"
        android:layout_width="50dp"
        android:background="@drawable/test_drawable"
        android:layout_height="50dp"
        android:layout_marginBottom="10dp" />

    <Button
        android:id="@+id/btn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Change Color"
        android:textAllCaps="false" />

</LinearLayout>
```

MainActivity.java
```java
import android.content.Context;
import android.graphics.Color;
import android.graphics.drawable.GradientDrawable;
import android.net.ConnectivityManager;
import android.net.NetworkInfo;
import android.os.AsyncTask;
import android.os.Handler;
import android.os.Message;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.ImageView;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    private ImageView image;
    private Button btn;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        image = (ImageView) findViewById(R.id.image);
        btn = (Button) findViewById(R.id.btn);

        btn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                GradientDrawable bgShape = (GradientDrawable) image.getBackground();
                int changeColors[] = {0xffffffff, 0xffff0000};
                bgShape.setColors(changeColors);
            }
        });
    }
}
```
![img7](/img/2019-08-13-xml-drawable-7.png)
![img8](/img/2019-08-13-xml-drawable-8.png)