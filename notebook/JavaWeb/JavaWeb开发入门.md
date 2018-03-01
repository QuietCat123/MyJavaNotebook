---
title: JavaWeb开发入门
tags: java,web
grammar_cjkRuby: true
---
# 介绍
![enter description here][1]

![enter description here][2]

## 安装配置Tomcat
conf/server.xml 下有服务器配置

## Eclipse Java Web配置
纯净版需要安装插件
http://blog.csdn.net/icing9520/article/details/17162423

安装插件注意work with要选择符合版本的（

创建New dynamic Web Project
...

![enter description here][3]

在WebContent中创建jsp页面
```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>example1_2.jsp</title>
</head>
<body>
<%
int i = 100, j = 200;
%>
100 + 200 的和为：<%= i+j%>
</body>
</html>
```

## jsp运行机制

![JSP页面运行机制][4]

.java保存在Tomcat安装目录的work文件夹内。




  [1]: ./images/JAVA%20WEB%E5%BC%80%E5%8F%91%E6%8A%80%E6%9C%AF.jpg "JAVA WEB开发技术"
  [2]: ./images/JAVA%20Web%E5%BC%80%E5%8F%91%E5%B7%A5%E5%85%B7.jpg "JAVA Web开发工具"
  [3]: ./images/JAVA%20WEB%E9%A1%B9%E7%9B%AE%E7%BB%93%E6%9E%84.png "JAVA WEB项目结构"
  [4]: ./images/IMG_20180301_142922~01.jpg "IMG_20180301_142922~01"