---
layout: post
title: "[Android/Java] 이어폰 연결 여부 확인"
categories: Android Java
---

```java
public class MainActivity extends AppCompatActivity {
 
    TextView text;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
 
        text = (TextView) findViewById(R.id.text);
 
        // 이어폰 broadcastreceiver
        IntentFilter mIntentFilter = new IntentFilter(Intent.ACTION_HEADSET_PLUG);
        BroadcastReceiver mBroadcastReceiver = new BroadcastReceiver() {
            @Override
            public void onReceive(Context context, Intent intent) {
                boolean isEarphoneOn = (intent.getIntExtra("state", 0) > 0) ? true : false;
                if (isEarphoneOn) {
                    text.setText("Earphone is plugged");
                } else {
                    text.setText("Earphone is unPlugged");
                }
            }
        };
        registerReceiver(mBroadcastReceiver, mIntentFilter);
    }
}
```