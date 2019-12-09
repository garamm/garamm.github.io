---
title: "[Android/Java] WebView"
categories:
  - Android/Java
---

AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.ram.myapplication">
 
    <!-- Permission 추가 -->
    <uses-permission android:name="android.permission.INTERNET" />
    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
 
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>
 
</manifest>
```

styles.xml
```xml
<resources>
    <!-- Base application theme. -->
    <style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
        <!-- Customize your theme here. -->
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
        <!-- 다음 두 줄 추가 -->
        <item name="windowNoTitle">true</item>
        <item name="windowActionBar">false</item>
    </style>
</resources>
```

activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">
 
    <!-- WebView 추가-->
    <WebView
        android:id="@+id/webView"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
 
</LinearLayout>
```

MainActivity.java
```java
package com.ram.myapplication;
 
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
 
import android.content.Context;
import android.content.pm.ApplicationInfo;
import android.content.pm.PackageInfo;
import android.content.pm.PackageManager;
import android.net.http.SslError;
import android.os.Build;
import android.webkit.CookieManager;
import android.webkit.CookieSyncManager;
import android.webkit.SslErrorHandler;
import android.webkit.WebSettings;
import android.webkit.WebView;
import android.webkit.WebViewClient;
 
public class MainActivity extends AppCompatActivity {
 
    WebView webView;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
 
        // 쿠키
        if (Build.VERSION.SDK_INT < Build.VERSION_CODES.LOLLIPOP) {
            CookieSyncManager.createInstance(this);
        }
 
        webView = findViewById(R.id.webView);
 
        WebSettings settings = webView.getSettings();
        settings.setJavaScriptEnabled(true);  // JavaScript 활성화
        settings.setSupportZoom(false);
 
        // HTML5 LocalStorage 활성화
        // SDK 2.1 부터 HTML5 LocalStorage를 사용할 수 있다.
        if (Build.VERSION.SDK_INT >= 7) {
            settings.setDomStorageEnabled(true);
        }
 
        // https도 접근 가능하도록..
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP) {
            webView.getSettings().setMixedContentMode(WebSettings.MIXED_CONTENT_ALWAYS_ALLOW);
            CookieManager cookieManager = CookieManager.getInstance();
            cookieManager.setAcceptCookie(true);
            cookieManager.setAcceptThirdPartyCookies(webView, true);
        }
 
        // User-Agent
        Context context = webView.getContext();
        PackageManager packageManager = context.getPackageManager();
        String appName = "";
        String appVersion = "";
        String userAgent = settings.getUserAgentString();
 
        try {
            // App 이름 추출
            ApplicationInfo applicationInfo = packageManager.getApplicationInfo(context.getPackageName(), PackageManager.GET_META_DATA);
            appName = packageManager.getApplicationLabel(applicationInfo).toString();
            // App 버전 추출
            PackageInfo packageInfo = packageManager.getPackageInfo(context.getPackageName(), 0);
            appVersion = String.format("%s", "" + packageInfo.versionName);
 
            // User-Agent에 App 이름과 버전을 붙여서 보내줌
            userAgent = String.format("%s %s %s", userAgent, appName, appVersion);
 
            // 변경된 User-Agent 반영
            settings.setUserAgentString(userAgent);
 
        } catch (PackageManager.NameNotFoundException e) {
            // e.printStackTrace();
        }
 
 
        webView.setWebViewClient(new SslWebViewConnect());
        webView.loadUrl("https://garamm.github.io");
    }
 
    @Override
    protected void onResume() {
        super.onResume();
        if (Build.VERSION.SDK_INT < Build.VERSION_CODES.LOLLIPOP) {
            CookieSyncManager.getInstance().startSync();
        }
    }
 
    @Override
    protected void onPause() {
        super.onPause();
        if (Build.VERSION.SDK_INT < Build.VERSION_CODES.LOLLIPOP) {
            CookieSyncManager.getInstance().stopSync();
        }
    }
 
    // https도 접근 가능하도록..
    //웹뷰 로드시 SSL 인증서 에러 방지
    public class SslWebViewConnect extends WebViewClient {
 
        @Override
        public void onReceivedSslError(WebView view, SslErrorHandler handler, SslError error) {
            handler.proceed(); // SSL 에러가 발생해도 계속 진행!
        }
 
        public boolean shouldOverrideUrlLoading(WebView view, String url) {
            view.loadUrl(url);
            return true;//응용프로그램이 직접 url를 처리함
        }
 
 
        @Override
        public void onPageFinished(WebView view, String url) {
            // 쿠키
            if (Build.VERSION.SDK_INT < Build.VERSION_CODES.LOLLIPOP) {
                CookieSyncManager.getInstance().sync();
            } else {
                CookieManager.getInstance().flush();
            }
        }
    }
 
    @Override
    public void onBackPressed() {
        if (webView.canGoBack()) {
            webView.goBack();
        } else {
            super.onBackPressed();
        }
    }
}
```

// 참고
1) 화면 전환시 WebView reload 현상을 없애고 싶을 때
MainActivity.java
```java
@Override
public void onConfigurationChanged(Configuration newConfig){
    super.onConfigurationChanged(newConfig);
}
```
AndroidMainfest.xml
```xml
android:configChanges="keyboard|keyboardHidden|orientation|orientation|screenSize"
```