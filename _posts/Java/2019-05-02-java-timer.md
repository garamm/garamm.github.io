---
layout: post
title: "[Java] Timer"
categories: Java
---


```java
package test;

import java.util.Timer;
import java.util.TimerTask;

public class Example {

	public static void main(String[] args) {
		TimerTask mTask;
		Timer mTimer;
	
		mTask = new TimerTask() {
			@Override
			public void run() {
				System.out.println("test!");
			}
		};

		mTimer = new Timer();

		// mTimer.schedule(mTask, 5000); // 5초 뒤에 한번만 실행
		mTimer.schedule(mTask, 3000, 5000); // 3초 뒤에 실행하고, 5초마다 반복
		
		//mTimer.cancel(); // timer의 작업 취소
	}
}
```