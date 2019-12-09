---
layout: posts
title: "[Java] 과세 계산하기"
comments: true
categories: [Java]
---


```java
package test;

public class Example {

	static int sumPrice = 11000;
	static int halin = 1000;
	static int mper = 50;

	public static void main(String[] args) {
		System.out.println("기본금액 : " + String.format("%,d", sumPrice));
		System.out.println("할인금액 : " + String.format("%,d", halin));
		System.out.println("비과세퍼센트 : " + String.format("%,d", mper));
		System.out.println("=====================");

		nonsurtax();
		taxfreeincome();
		surtax();
		nontaxfreeincode();
		taxfree();
	}

	public static void nonsurtax() { // 비부세별도
		int begwase = 0; // 값 초기화

		if (mper != 0) {
			begwase = (int) Math.round((sumPrice - halin) * mper / 100);
		}
		int bugase = (int) Math.round((sumPrice - halin - begwase) * 0.1);
		int gwase = sumPrice - halin - begwase;
		int totalPrice = sumPrice - halin + bugase;
		printResult("비부세별도", begwase, bugase, gwase, totalPrice);
	}

	public static void taxfreeincome() { // 부가세별도
		int begwase = 0;
		int gwase = sumPrice - halin;
		double bugase = (gwase * 1.1) - gwase;
		int totalPrice = (int) (sumPrice - halin + bugase);
		printResult("부가세별도", begwase, (int) bugase, gwase, totalPrice);
	}

	public static void surtax() { // 부가세포함
		int gwase = (int) Math.round((sumPrice - halin) / 1.1);
		int begwase = 0;
		int bugase = sumPrice - halin - gwase;
		int totalPrice = sumPrice - halin;
		printResult("부가세포함", begwase, bugase, gwase, totalPrice);
	}

	public static void nontaxfreeincode() { // 비부세포함
		int begwase = 0; // 값 초기화
		if (mper != 0) {
			begwase = (int) Math.round((sumPrice - halin) * mper / 100);
		}
		int gwase = (int) ((sumPrice - halin - begwase) / 1.1);
		int bugase = (int) sumPrice - halin - begwase - gwase;
		int totalPrice = sumPrice - halin;
		printResult("비부세포함", begwase, bugase, gwase, totalPrice);
	}
	
	public static void taxfree() { // 면세
		int totalPrice = sumPrice - halin;
		printResult("면세", 0, 0, 0, totalPrice);
	}

	public static void printResult(String type, int begwase, int bugase, int gwase, int totalPrice) {
		System.out.println("--- " + type + " ---");
		System.out.println("비과세 : " + String.format("%,d", begwase));
		System.out.println("과세 : " + String.format("%,d", gwase));
		System.out.println("부가세 : " + String.format("%,d", bugase));
		System.out.println("총합 : " + String.format("%,d", totalPrice));
	}

}
```