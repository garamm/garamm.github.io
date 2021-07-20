---
layout: post
title: "[Java] Object ArrayList 정렬하기"
category: java
date: "2019-05-02"
---


```java
// Data.java
public class Data {
	
	String str;
	int num;
	
	public Data(String str, int num) {
		this.str = str;
		this.num = num;
	}
	
	public String getStr() {
		return str;
	}
	public void setStr(String str) {
		this.str = str;
	}
	public int getNum() {
		return num;
	}
	public void setNum(int num) {
		this.num = num;
	}
}
```

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;

public class Example {

	static ArrayList<Data> data;

	public static void main(String[] args) {
		data = new ArrayList<Data>();
		data.add(new Data("str1", 1));
		data.add(new Data("str3", 3));
		data.add(new Data("str2", 2));
		data.add(new Data("str4", 4));

		data = sortIntDesc(data);
		for (int i = 0; i < data.size(); i++) {
			System.out.println(data.get(i).getStr() + ", " + data.get(i).getNum());
		}
	}

	// String 가나다라 순 정렬
	public static ArrayList<Data> sortStr(ArrayList<Data> getData) {
		Collections.sort(getData, new Comparator<Data>() {
			@Override
			public int compare(Data o1, Data o2) {
				String first = o1.getStr();
				String second = o2.getStr();
				return first.compareTo(second);
			}
		});
		return getData;
	}

	// int 오름차순 정렬
	public static ArrayList<Data> sortIntAsc(ArrayList<Data> getData) {
		Collections.sort(getData, new Comparator<Data>() {
			@Override
			public int compare(Data o1, Data o2) {
				int first = o1.getNum();
				int second = o2.getNum();
				if (first > second) {
					return 1;
				} else if (first < second) {
					return -1;
				} else {
					return 0;
				}
			}
		});
		return getData;
	}

	// int 내림차순 정렬
	public static ArrayList<Data> sortIntDesc(ArrayList<Data> getData) {
		Collections.sort(getData, new Comparator<Data>() {
			@Override
			public int compare(Data o1, Data o2) {
				int first = o1.getNum();
				int second = o2.getNum();
				if (first < second) {
					return 1;
				} else if (first > second) {
					return -1;
				} else {
					return 0;
				}
			}
		});
		return getData;
	}
}
```
