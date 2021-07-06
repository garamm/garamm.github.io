---
layout: post
title: "[Android/Java] Activity를 Dialog 형태로 띄우기"
category: android
date: "2019-08-12"
---

style.xml
```xml
<style
    name="NoBackgroundDialog"
    parent="android:Theme.DeviceDefault.Light.Dialog.NoActionBar">
    <item name="android:windowBackground">@android:color/transparent</item>
</style>
```

AndroidManifest.xml
```xml
<activity
    android:name=".SubnActivity"
    android:theme="@style/NoBackgroundDialog" />
```

SubActivity.java
```java
@Overrideprotected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    requestWindowFeature(Window.FEATURE_NO_TITLE);
    setFinishOnTouchOutside(false);
    setContentView(R.layout.activity_customer_pay_list);
    Display display = ((WindowManager) getSystemService(Context.WINDOW_SERVICE)).getDefaultDisplay();
    int width = (int) (display.getWidth() * 0.7);
    int height = (int) (display.getHeight() * 0.7);
    getWindow().getAttributes().width = width;
    getWindow().getAttributes().height = height;
}
```

이때 Activity는 extends AppCompatActivity가 아닌
extends Activity로 적용해야 함