---
layout: post
title: "[Java] Timer 타이머"
date: "2019-05-02"
updated: []
categories: [Java]
tags: [timer]
---

```java
import java.util.Timer;
import java.util.TimerTask;

public class Main {
    public static void main(String[] args) {

        TimerTask task;
        Timer timer;

        task = new TimerTask() {
            @Override
            public void run() {
                System.out.println("test!");
            }
        };

        timer = new Timer();

        // timer.schedule(task, 5000); // 5초 뒤에 한번만 실행
        timer.schedule(task, 3000, 5000); // 3초 뒤에 실행하고, 5초마다 반복
        //timer.cancel(); // timer의 작업 취소
    }
}
```