---
title: "[Android/Java] 점선 그리기"
categories:
  - Android/Java
---

line_horizontal_dash.xml
```xml
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:left="-2dp"
        android:right="-2dp"
        android:top="-2dp">
        <shape>
            <solid android:color="@android:color/transparent" />
            <stroke
                android:width="1px"
                android:color="#ababb2"
                android:dashGap="10px"
                android:dashWidth="10px" />
        </shape>
    </item>
</layer-list>
```

activity_main.xml
```xml
<ImageView
    android:layout_width="match_parent"
    android:layout_height="2px"
    android:src="@drawable/line_horizontal_dash" >
</ImageView>
```