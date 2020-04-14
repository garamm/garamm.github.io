---
layout: post
title: "[Android/Java] 네트워크 연결 확인"
categories: Android Java
---

AndroidManifest.xml
```xml
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.CHANGE_NETWORK_STATE" />
```

java
```java
private Boolean getNetworkState(){
    ConnectivityManager manager = (ConnectivityManager) getSystemService (Context.CONNECTIVITY_SERVICE);
    boolean isMobileAvailable = manager.getNetworkInfo(ConnectivityManager.TYPE_MOBILE).isAvailable();
    boolean isMobileConnect = manager.getNetworkInfo(ConnectivityManager.TYPE_MOBILE).isConnectedOrConnecting();
    boolean isWifiAvailable = manager.getNetworkInfo(ConnectivityManager.TYPE_WIFI).isAvailable();
    boolean isWifiConnect = manager.getNetworkInfo(ConnectivityManager.TYPE_WIFI).isConnectedOrConnecting();
 
    if ((isWifiAvailable && isWifiConnect) || (isMobileAvailable && isMobileConnect)){
        return true;
    } else {
        return false;
    }
}
```