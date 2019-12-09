---
layout: posts
title: "[Java] 문자열 비교하기, compareTo()"
comments: true
categories: [Java]
---

a.compareTo(b) 라고 할 때 결과 값이  
  
0 일 경우→ a = b  
0 보다 작을 경우 → a < b  
0 보다 클 경우 → a > b  

```java
String a = "abc";
String b = "abb";
 
if( a.compareTo(b)==0 || a.compareTo(b)<0 ) {
           Log.i("result tag", "a<=b");
} else if ( a.compareTo(b)>0 ) {
           Log.i("result tag", "a>b");
}
```