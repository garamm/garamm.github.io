---
title: "[Android/Java] Manifest 속성"
categories:
  - Android/Java
---

속성이 적용되는 위치를 확인
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.hanul.myapplication">
    <!-- 비고, 여기엔 권한 관련 정보를 입력-->
    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity"
            android:windowSoftInputMode="adjustPan|stateAlwaysHidden"
            android:screenOrientation="landscape">
            
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
 
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>
 
</manifest>
```

Activity 시작 시 EditText로 인해 소프트 키보드가 자동으로 띄워지는 현상 막기
```xml
android:windowSoftInputMode="adjustPan|stateAlwaysHidden"
```

Activity 가로, 세로 고정
```xml
android:screenOrientation="landscape"
```

portrait: 화면을 세로 방향으로 고정
landscape: 화면을 가로 방향으로 고정
sensorPortrait: 화면을 세로 방향으로 고정, 센서에 따라 정/역방향으로 표시
sensorLandscape: 화면을 세로 방향으로 고정, 센서에 따라 정/역방향으로 표시
sensor: 센서에 따라 최대 4가지 방향으로 회전
