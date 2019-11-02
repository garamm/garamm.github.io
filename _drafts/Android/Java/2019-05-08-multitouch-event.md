---
title: "[Android/Java] 멀티터치 이벤트"
categories:
  - Android/Java
---

```java
public class MainActivity extends AppCompatActivity {
 
    TextView tv;
    // 3개까지의 멀티터치를 다루기 위한 배열
    int id[] = new int[3];
    int x[] = new int[3];
    int y[] = new int[3];
    String result;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        tv = (TextView) findViewById(R.id.tv);
    }
 
    @Override
    public boolean onTouchEvent(MotionEvent event) {
        int pointer_count = event.getPointerCount(); //현재 터치 발생한 포인트 수를 얻는다.
        if (pointer_count > 3) pointer_count = 3; //4개 이상의 포인트를 터치했더라도 3개까지만 처리를 한다.
 
        switch (event.getAction() & MotionEvent.ACTION_MASK) {
            case MotionEvent.ACTION_DOWN: //한 개 포인트에 대한 DOWN을 얻을 때.
                result = "싱글터치 : \n";
                id[0] = event.getPointerId(0); //터치한 순간부터 부여되는 포인트 고유번호.
                x[0] = (int) (event.getX());
                y[0] = (int) (event.getY());
                result = "싱글터치 : \n";
                result += "(" + x[0] + "," + y[0] + ")";
                break;
 
            case MotionEvent.ACTION_POINTER_DOWN: //두 개 이상의 포인트에 대한 DOWN을 얻을 때.
                result = "멀티터치 :\n";
                for (int i = 0; i < pointer_count; i++) {
                    id[i] = event.getPointerId(i); //터치한 순간부터 부여되는 포인트 고유번호.
                    x[i] = (int) (event.getX(i));
                    y[i] = (int) (event.getY(i));
                    result += "id[" + id[i] + "] (" + x[i] + "," + y[i] + ")\n";
                }
                break;
 
            case MotionEvent.ACTION_MOVE:
                result = "멀티터치 MOVE:\n";
                for (int i = 0; i < pointer_count; i++) {
                    id[i] = event.getPointerId(i);
                    x[i] = (int) (event.getX(i));
                    y[i] = (int) (event.getY(i));
                    result += "id[" + id[i] + "] (" + x[i] + "," + y[i] + ")\n";
                }
                break;
            case MotionEvent.ACTION_UP:
                result = "";
                break;
        }
 
        tv.setText(result);
 
        return super.onTouchEvent(event);
    }
}
```