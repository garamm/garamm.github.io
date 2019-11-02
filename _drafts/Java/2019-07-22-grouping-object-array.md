---
layout: posts
title: "[Java] 객체 배열에서 같은 날짜끼리 그룹지어 새로운 배열에 저장하기"
comments: true
categories: [Java]
---

```
0번째 데이터 : 2019-01-01, txt1
1번째 데이터 : 2019-01-01, txt2
2번째 데이터 : 2019-01-01, txt3
3번째 데이터 : 2019-01-01, txt4
4번째 데이터 : 2019-01-02, txt1
5번째 데이터 : 2019-01-02, txt2
```
이런 구조로 되어있는 객체 베열을

```
0번째 데이터 : 2019-01-01, [{2019-01-01, txt1}, {2019-01-01, txt2}...]
1번째 데이터 : 2019-01-02, [{2019-01-02, txt1}, {2019-01-02, txt2}]
```
다음과 같이 변환하는 방법!

Obj.java
```java
public class Obj {
    String date;
    String txt;

    public Obj(String date, String txt) {
        this.date = date;
        this.txt = txt;
    }

    public String getDate() {
        return date;
    }

    public void setDate(String date) {
        this.date = date;
    }

    public String getTxt() {
        return txt;
    }

    public void setTxt(String txt) {
        this.txt = txt;
    }
}
```

Obj2.java
```java
import java.util.ArrayList;

public class Obj2 {

    String date;
    ArrayList<Obj> objs;

    public Obj2(String date, ArrayList<Obj> objs) {
        this.date = date;
        this.objs = objs;
    }

    public String getDate() {
        return date;
    }

    public void setDate(String date) {
        this.date = date;
    }

    public ArrayList<Obj> getObjs() {
        return objs;
    }

    public void setObjs(ArrayList<Obj> objs) {
        this.objs = objs;
    }
}
```

Main.java
```java
import java.awt.desktop.AppReopenedListener;
import java.util.ArrayList;
import java.util.List;

public class Test {

    public static ArrayList<Obj> objs;

    public static void main(String[] args) {

        objs = new ArrayList<>();
        objs.add(new Obj("2019-01-01", "txt1"));
        objs.add(new Obj("2019-01-01", "txt2"));
        objs.add(new Obj("2019-01-01", "txt3"));
        objs.add(new Obj("2019-01-01", "txt4"));
        objs.add(new Obj("2019-01-02", "txt1"));
        objs.add(new Obj("2019-01-02", "txt2"));

        ArrayList<Obj2> obj2s = new ArrayList<>();

        ArrayList<Obj> tt = new ArrayList<>();
        tt.add(objs.get(0));
        obj2s.add(new Obj2(objs.get(0).getDate(), tt));

        for(int i=1; i<objs.size(); i++) {
            if(objs.get(i-1).getDate().equals(objs.get(i).getDate())) {
                obj2s.get(obj2s.size()-1).getObjs().add(objs.get(i));
            } else {
                ArrayList<Obj> ooo = new ArrayList<>();
                ooo.add(objs.get(i));
                obj2s.add(new Obj2(objs.get(i).getDate(), ooo));
            }
        }

        for(int i=0; i<obj2s.size(); i++) {
            System.out.println(obj2s.get(i).getDate());
            for(int j=0; j<obj2s.get(i).getObjs().size(); j++) {
                System.out.println(obj2s.get(i).getObjs().get(j).txt);
            }
        }
    }
}
```