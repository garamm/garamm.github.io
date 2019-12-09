---
title: "[Android/Java] 전화 상태 확인 Listener"
categories:
  - Android/Java
---

Manifest.xml에 permission 추가
```xml
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
```


MainActivity.java
```java
public class MainActivity extends AppCompatActivity {
 
    TextView stateText;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
 
        stateText = (TextView) findViewById(R.id.stateText);
 
        TelephonyManager manager = (TelephonyManager) getSystemService(Context.TELEPHONY_SERVICE);
        manager.listen(phoneStateListener, PhoneStateListener.LISTEN_CALL_STATE);
 
    }
 
    private PhoneStateListener phoneStateListener = new PhoneStateListener() {
        public void onCallStateChanged(int state, String incomingNumber) {
            if (state == TelephonyManager.CALL_STATE_RINGING) {
                stateText.setText("벨소리 울리는중");
            } else if (state == TelephonyManager.CALL_STATE_OFFHOOK) {
                stateText.setText("통화 시작");
            } else if (state == TelephonyManager.CALL_STATE_IDLE) {
                stateText.setText("대기중");
            }
        }
    };
}
```