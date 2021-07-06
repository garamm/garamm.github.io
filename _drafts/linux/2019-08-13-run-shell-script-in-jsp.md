---
layout: post
title: "[Linux] jsp로 쉘 스크립트 실행해서 실행 결과 화면에 출력하기"
category: linux
date: "2019-08-13"
---

```java
<%@ page contentType="text/html;charset=EUC-KR" %>
<%@ page import="java.io.BufferedReader" %>
<%@ page import="java.io.InputStreamReader" %>
<%@ page import="java.util.regex.Matcher" %>
<%@ page import="java.util.regex.Pattern" %>
<%
 String cmd = "sh /home/hello.sh";
 String str = null; 
 String result = "";    
 StringBuffer sb = null;

 if (cmd == null || "".equals(cmd)) {
    result = "invalid cmd";
 } else {
    sb = new StringBuffer(); 
    Process proc = Runtime.getRuntime().exec(cmd); 
    BufferedReader br = new BufferedReader(new InputStreamReader(proc.getInputStream())); 
    while ((str = br.readLine()) != null){
        sb.append(str).append("</br>"); 
    } 
    try { 
        proc.waitFor(); 
        int rtn = proc.exitValue();

        if (rtn != 0) {
            result = "exit value was non-zero " + rtn;
        }
    } catch(InterruptedException e) {
        result = "process was interrupted";
    } finally {
        br.close(); 
   }
}
%>
<%
   if(result.equals("")) {
%>
<%=sb.toString()%>
<%
   } else {
%>
       Error : <%=result%>
<%
   }
%>
```

[출처](http://blog.naver.com/PostView.nhn?blogId=hyoun1202&logNo=220156221481&categoryNo=7&parentCategoryNo=0&viewDate=&currentPage=2&postListTopCurrentPage=)