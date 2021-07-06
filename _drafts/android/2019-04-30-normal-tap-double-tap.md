---
layout: post
title: "[Android/Java] 버튼 일반클릭, 더블클릭 구분하기"
category: android
date: "2019-04-30"
---

```java
public class MainActivity extends AppCompatActivity {
 
    private Button btn;
    private long btnPressTime = 0;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
 
        btn = (Button) findViewById(R.id.button);
 
        btn.setOnClickListener(new View.OnClickListener() {
 
            @Override
            public void onClick(View v) {
                if (System.currentTimeMillis() > btnPressTime + 1000) {
                    btnPressTime = System.currentTimeMillis();
                    Toast.makeText(getApplicationContext(), "일반클릭~", Toast.LENGTH_SHORT).show();
                    return;
                }
                if (System.currentTimeMillis() <= btnPressTime + 1000) {
                    Toast.makeText(getApplicationContext(), "더블클릭!", Toast.LENGTH_SHORT).show();
                }
            }
        });
    }
}
```