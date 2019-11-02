---
title: "[Android/Java] 뒤로가기 두 번 눌러 종료하기"
categories:
  - Android/Java
---

BackPressCloseHandler.java
```java
public class BackPressCloseHandler {
 
    private long backKeyPressedTime = 0;
    private Toast toast;
 
    private Activity activity;
 
    public BackPressCloseHandler(Activity context) {
        this.activity = context;
    }
 
    public void onBackPressed() {
        if (System.currentTimeMillis() > backKeyPressedTime + 2000) {
            backKeyPressedTime = System.currentTimeMillis();
            showGuide();
            return;
        }
        if (System.currentTimeMillis() <= backKeyPressedTime + 2000) {
            activity.finish();
            toast.cancel();
        }
    }
 
    public void showGuide() {
        toast = Toast.makeText(activity,
                "\'뒤로\'버튼을 한번 더 누르시면 종료됩니다.", Toast.LENGTH_SHORT);
        toast.show();
    }
}
```


MainActivity.java
```java
public class MainActivity extends Activity {
 
    private BackPressCloseHandler backPressCloseHandler;
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);
        
        backPressCloseHandler = new BackPressCloseHandler(this);
    }
 
    @Override
    public void onBackPressed() {
        //super.onBackPressed();
        backPressCloseHandler.onBackPressed();
    }
}
```