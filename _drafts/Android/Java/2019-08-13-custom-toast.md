---
layout: post
title: "[Android/Java] Custom Toast Message"
categories: Android Java
---

toast_layout.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#005eab"
    android:orientation="vertical">

    <TextView
        android:id="@+id/text"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="5dp"
        android:layout_marginLeft="15dp"
        android:layout_marginRight="15dp"
        android:layout_marginTop="5dp"
        android:textColor="#fff" />

</LinearLayout>
```

CustomToast.java
```java
import android.app.Activity;
import android.content.Context;
import android.view.Gravity;
import android.view.LayoutInflater;
import android.view.View;
import android.widget.TextView;
import android.widget.Toast;

public class CustomToast extends Toast {
    Context mContext;

    public CustomToast(Context context) {
        super(context);
        mContext = context;
    }


    public void showToast(String body, int duration) {
        LayoutInflater inflater;
        View v;
        if (false) {
            Activity act = (Activity) mContext;
            inflater = act.getLayoutInflater();
            v = inflater.inflate(R.layout.toast_layout, null);
        } else {
            inflater = (LayoutInflater) mContext.getSystemService(Context.LAYOUT_INFLATER_SERVICE);
            v = inflater.inflate(R.layout.toast_layout, null);
        }
        TextView text = (TextView) v.findViewById(R.id.text);
        text.setText(body);
        show(this, v, duration);
    }

    private void show(Toast toast, View v, int duration) {
        toast.setGravity(Gravity.TOP, 0, 350);
        toast.setDuration(duration);
        toast.setView(v);
        toast.show();
    }
}
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
    android:gravity="center"
    android:padding="16dp">

    <Button
        android:layout_width="wrap_content"
        android:id="@+id/toastBtn"
        android:text="TOAST"
        android:layout_height="wrap_content" />

</LinearLayout>
```

MainActivity.java
```java
import android.content.ComponentName;
import android.content.Context;
import android.content.Intent;
import android.content.ServiceConnection;
import android.os.Handler;
import android.os.IBinder;
import android.os.Message;
import android.os.Messenger;
import android.os.RemoteException;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.util.Log;
import android.view.KeyEvent;
import android.view.View;
import android.view.inputmethod.EditorInfo;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;

import java.util.ArrayList;


public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Button toastBtn = (Button) findViewById(R.id.toastBtn);

        toastBtn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                CustomToast customToast = new CustomToast(getApplicationContext());
                customToast.showToast("커스텀 토스트", Toast.LENGTH_SHORT);
            }
        });
    }
}
```
![img1](/img/2019-08-13-custom-toast-1.png)

[참고](http://developer.android.com/guide/topics/ui/notifiers/toasts.html)
