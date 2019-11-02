---
title: "[Android/Java] Custom Checkbox"
categories:
  - Android/Java
---

---
체크박스의 아이콘 변경하기

check_off.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:width="15dp" android:height="15dp">
        <shape android:shape="rectangle">
            <solid android:color="#000" />
        </shape>
    </item>
 
    <item android:left="1dp" android:right="1dp" android:top="1dp" android:bottom="1dp">
        <shape android:shape="rectangle">
            <solid android:color="#fff" />
        </shape>
    </item>
</layer-list>
```

check_on.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:width="15dp"
        android:height="15dp">
        <shape android:shape="rectangle">
            <solid android:color="#000" />
        </shape>
    </item>
</layer-list>
```

check_selector.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <!-- xml 말고 이미지로 대체 가능-->
    <item android:drawable="@drawable/check_on" android:state_checked="true" />
    <item android:drawable="@drawable/check_off" />
</selector>
```

activity_main.xml
```xml
<?xml version="1.0"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">
 
    <CheckBox
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:button="@drawable/check_selector"
        android:paddingLeft="10dp"
        android:text="check" />
 
</LinearLayout>
```