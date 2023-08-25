---
layout: post
title: "[Java] 객체 배열에서 특정 값만 뽑아서 리스트 만들기"
date: "2019-05-28"
updated: []
categories: [Java]
tags: [array, object]
---

Data.java
```java
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
}
```
<br>
Main.java
```java
import java.util.ArrayList;
import java.util.List;
import java.util.stream.Collectors;

public class Main {

    static ArrayList<Data> data;

    public static void main(String[] args) {
        data = new ArrayList<Data>();
        data.add(new Data("str1", 1));
        data.add(new Data("str3", 3));
        data.add(new Data("str2", 2));
        data.add(new Data("str4", 4));

        // 방법 1
        List<String> ids = data.stream().map(e -> e.str)
                .collect(Collectors.toCollection(ArrayList::new));
        System.out.println(ids);

        // 방법 2
        List<String> ids2 = data.stream().map(Data::getStr)
                .collect(Collectors.toCollection(ArrayList::new));
        System.out.println(ids2);
    }

}
```