---
title: JSP语法基础
tags: JSP,JAVA
grammar_cjkRuby: true
---
Java Server Pages其根本是一个简化的Servlet设计
jsp运行在服务器端的脚本语言,JSP页面是基于HTML网页的程序.

![enter description here][1]
## JSP页面的基本结构
传统页面中加入JAVA的程序段和JSP标签就构成了一个JSP页面.
### JSP注释
<% -- 此处为注释 --%>

### 脚本元素
<%! 声明语句;%>
元素和方法都行
注意,变量和方法是页面级别的,他们在所在页面有效,变量对所有用户都互相影响.

### 表达式
<%= 表达式 %>
<%= 是一个整体,没有分号
```HTML
<html> 
<head>
<title>A Comment Test</title>
</head> 
<body>
<p>   
  Today's date: <%= (new java.util.Date()).toLocaleString()%>
</p>
</body> 
</html> 
```
### JSP中的JAVA程序段
<% 程序 %>
局部变量,在JSP后继的所有程序中都有效.不同用户不影响

## JSP指令
JSP指令用来设置与整个JSP页面相关的属性。

JSP指令语法格式：
`<%@ directive attribute="value" %>`

![JSP指令][2]

### page指令

```html
<%@ page language="java" 
	 contentType="MIMETpye; charset=characterSet"
    	 pageEncoding="characterSet"
    	 import="package.class"
    	 extends="package.class"
    	 buffer="none|size kb|8kb"
    	 errorPage="URL"
    	 autoFlush="false|true"
    	 session="false|true"
    	 isThreadSafe="false|true"
    	 isErrorPage="true|false"
         isELIgnored="true|false"
    	 %>
```
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8" import="java.util.Date"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>example3_1</title>
</head>
<body>
<%Date date = new Date(); %>
当前的系统日期为：<%=date %>
<br>
<%
for(int i=1;i<6;i++)
	out.print("打印了" + i + "次<br>");
%>
</body>
</html>
```
![page1][3]

![page2][4]
当JSP页面包含中文时,contentType和pageEncoding属性字符集设置为中文字符集编码,UTF-8.
### include

  [1]: ./images/JSP%E5%A4%84%E7%90%86.png "JSP处理"
  [2]: ./images/1520083305109.jpg
  [3]: ./images/1520084622972.jpg
  [4]: ./images/1520084667432.jpg