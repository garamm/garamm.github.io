---
layout: post
title: "[Java] 전화번호 포멧 만들기"
category: java
date: "2019-05-02"
---


```java
public class PhoneNumFormatter {
  public static void main(String[] args) {
    System.out.println(phoneFormatter("0314745371", false));
  }

  public static String phoneFormatter(String num, boolean type) {
    String formatNum = "";
    String temp = "";
    
    if (num.length() == 11) {
      if (type) {
        temp = "(\\d{3})(\\d{4})(\\d{4})";
        formatNum = num.replaceAll(temp, "$1-****-$3");
      } else {
        temp = "(\\d{3})(\\d{4})(\\d{4})";
        formatNum = num.replaceAll(temp, "$1-$2-$3");
      }
    } else if (num.length() == 8) {
      temp = "(\\d{4})(\\d{4})";
      formatNum = num.replaceAll(temp, "$1-$2");
    } else {
      if (num.substring(0, 2).equals("02")) {
        if (type) {
            temp = "(\\d{2})(\\d{4})(\\d{4})";
            formatNum = num.replaceAll(temp, "$1-****-$3");
        } else {
          temp = "(\\d{2})(\\d{4})(\\d{4})";
          formatNum = num.replaceAll(temp, "$1-$2-$3");
        }
      } else {
        System.out.println("else");
        if (type) {
          temp = "(\\d{3})(\\d{3})(\\d{4})";
          formatNum = num.replaceAll(temp, "$1-****-$3");
        } else {
          temp = "(\\d{3})(\\d{3})(\\d{4})";
          formatNum = num.replaceAll(temp, "$1-$2-$3");
        }
      }
    }
    return formatNum;
  }
}
```