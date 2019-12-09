---
layout: posts
title: "[Java] ArrayList + ArrayList"
comments: true
categories: [Java]
---


```java
package test;

import java.util.List;
import java.util.ArrayList;

public class Test {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		List<String> list = new ArrayList<String>();
		list.add("a");
		list.add("b");
		list.add("c");
		list.add("d");

		List<String> list2 = new ArrayList<String>();
		list2.add("1");
		list2.add("2");
		list2.add("3");
		list2.add("4");
		list.addAll(list2);

		List<String> list3 = new ArrayList<String>();
		list3.add("A");
		list3.add("B");
		list.addAll(0, list3);

		System.out.println(list);
	}
}
```