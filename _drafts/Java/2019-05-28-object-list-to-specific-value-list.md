---
layout: posts
title: "[Java] Object List에서 특정 value만 뽑아서 List 만들기"
comments: true
categories: [Java]
---


```java
// Obj.java
public class Obj {
	
	public int num;
	public String name;
	
	public Obj(int num, String name) {
		this.num = num;
		this.name = name;
	}

	public int getNum() {
		return num;
	}

	public void setNum(int num) {
		this.num = num;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}
}
```
```java
// Example.java
import java.util.ArrayList;
import java.util.List;
import java.util.stream.Collectors;

public class Example {
	public static void main(String[] args) {

		List<Obj> objs = new ArrayList<>();
		
		objs.add(new Obj(1, "name1"));
		objs.add(new Obj(2, "name2"));
		objs.add(new Obj(3, "name3"));
		objs.add(new Obj(4, "name4"));
		objs.add(new Obj(5, "name5"));
		
		
		// 방법 1
		List<String> ids = objs.stream().map(e -> e.name)
				.collect(Collectors.toCollection(ArrayList::new));
		System.out.println(ids);

		// 방법 2
		List<String> ids2 = objs.stream().map(Obj::getName)
				.collect(Collectors.toCollection(ArrayList::new));
		System.out.println(ids2);
	}
}
```