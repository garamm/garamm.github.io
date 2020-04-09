---
layout: post
title: "[Java] ArrayList 데이터의 중복 제거(String, 객체)"
categories: Java
---

String일 때 ArrayList 데이터의 중복 제거
```java
public ArrayList<String> containsList(ArrayList<String> inputList) {
        ArrayList<String> resultList = new ArrayList<String>();
        for (int i = 0; i < inputList.size(); i++) {
            if (!resultList.contains(inputList.get(i))) {
                resultList.add(inputList.get(i));
            }
        }
        return resultList;
    }
```   
   
Object일 때 ArrayList 데이터의 중복 제거
```java
// Data.java
package test;

public class Data {
	String codeNum;
	String id;
	String name;

	public Data(String codeNum, String id, String name) {
		this.codeNum = codeNum;
		this.id = id;
		this.name = name;
	}

	public String getCodeNum() {
		return codeNum;
	}

	public void setCodeNum(String codeNum) {
		this.codeNum = codeNum;
	}

	public String getId() {
		return id;
	}

	public void setId(String id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	@Override
	public boolean equals(Object obj) {
		if (obj instanceof Data) {
			Data temp = (Data) obj;
			if (this.codeNum.equals(temp.getCodeNum()) && this.id.equals(temp.getId())) {
				return true;
			}
		}
		return super.equals(false);
	}

	@Override
	public int hashCode() {
		return (this.codeNum.hashCode() + this.id.hashCode());
	}
}
```
```java
package test;

import java.util.ArrayList;
import java.util.HashSet;

public class Example {
	public static ArrayList<Data> datas;
	
	public static void main(String[] args) {	
		datas = new ArrayList<Data>();
		
		addDatas();
		
		HashSet<Data> listSet = new HashSet<Data>(datas);
		ArrayList<Data> processedList = new ArrayList<Data>(listSet);
		
		printListInfo(processedList);
	}
	
	private static void addDatas() {
		datas.add(new Data("1", "a", "임"));
		datas.add(new Data("1", "b", "임"));
		datas.add(new Data("2", "c", "김"));
		datas.add(new Data("2", "c", "김"));
		datas.add(new Data("2", "c", "김"));
		datas.add(new Data("2", "c", "김"));
	}
	
	private static void printListInfo(ArrayList<Data> resultDatas) {
		
		for(int i=0; i<resultDatas.size(); i++) {
			System.out.println(resultDatas.get(i).getCodeNum()+", "+resultDatas.get(i).getId()
					+", "+resultDatas.get(i).getName());
		}
	}
}
```