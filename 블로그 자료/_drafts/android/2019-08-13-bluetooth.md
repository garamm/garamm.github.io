---
layout: post
title: "[Android/Java] Bluetooth 블루투스 on/off 확인"
category: android
date: "2019-08-13"
---

```java
BluetoothAdapter bluetoothAdapter = BluetoothAdapter.getDefaultAdapter();

private boolean checkBluetooth() {
    if (bluetoothAdapter == null) {
        Toast.makeText(getApplicationContext(), "블루투스를 지원하지 않는 기기입니다.", Toast.LENGTH_SHORT).show();
        return false;
    } else {
        if (bluetoothAdapter.isEnabled()) {
            Toast.makeText(getApplicationContext(), "블루투스를 켜주세요", Toast.LENGTH_SHORT).show();
            return false;
        } else {
            return true;
        }
    }
}
```