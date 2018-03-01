---
title:  Web前端技术
tags: web前端,
grammar_cjkRuby: true
---
![enter description here][1]

# HTTP协议
## http协议介绍
特点：
* 支持客户/服务器模式
* 简单快速，GET/PUT/HEAD/POST等
* 灵活，允许传输任意类型的数据对象，由Content-Type标记类型
* 面向无连接：每次连接只处理一次请求。服务器处理完客户端请求并接受到客户应答后，便断开连接
* 无状态：指协议对于处理事物没有记忆能力。意味着如果后续处理需要前面的信息，只能重传。

## http请求响应机制
请求（Request）、响应(Response)机制。
1. 客户端请求服务。包括请求行，请求头标，空行，请求数据
2. 服务器接收请求并响应。状态行，响应头标，空行，相应数据
3. 服务器关闭本次连接，客户端解析响应。

# html基础
## 页面结构
```html
<!DOCTYPE html>
<html>
	<head>
		头部信息
	</head>
	<body>
		文档主体，正文部分
	</body>
</html>	
```
body一般不能忽略，head可以忽略，
<!DOCTYPE>声明不是HTML标签，它只是浏览器针对该页面用哪个HTML版本进行编码。

## HTML标签
### 单标签
\<br>
\<img>
\<hr>
\<link>
\<mera>
等
### 双标签
分为开始和结束
除了单标签，其他都是双

### 标签属性
<标签名字 属性1 = "值" 属性2 属性3>
例如
`<img src = "logo.jpg" width="100" height="100">`
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>网页标题</title>
</head>
<body>
<img src = "logo.jpg" width="100" height = "100">
<br>
<p align = "center">
这是我第一个HTML页面
</p>
</body>
</html>
```
* \<html>：表明此文档为html文档，在文档开始和结束
* \<head>：此部分内容为HTML头部
* \<meta>:在文档中模拟HTTP协议响应头报文。
* 用在head和\head中，用处很多，属性有content,name,http-equiv等。其中name主要用于描述网页，对应content（网页内容）方便搜索引擎查找分类。最重要的是description（描述）keywords。所以该给每个HTML文档加一个meta值
* \<title>:是指该HTML文档的标题/title结束
* \<body>：主体部分，可嵌套其他标签
* \<img>:包含src,width,length等属性，引用页面外部的图片资源等
* \<br>标签实现换行功能单标签
* \<p>段落标志，有一些属性，如align等

## html常用标签
### 表格标签\<table>
表格有如干行\<tr>标签定义。\<td>把每行分为若干单元格
第一行可以用\<th>标为标题行
td指 Table Data
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>网页标题</title>
</head>
<body>
<img src = "logo.jpg" width="100" height = "100">
<br>
<p align = "center">
这是我第一个HTML页面
</p>
<table border = "1">
<tr>
<th>标题1</th>
<th>标题2</th>
<th>标题m</th>
</tr>
<tr>
<td>第一行,第一列</td>
<td>第一行,第二列</td>
<td>第一行,第三列</td>
</tr>
<tr>
<td>第二行,第一列</td>
<td>第二行,第二列</td>
<td>第二行,第三列</td>
</tr>
</table>
</body>
</html>
```
### 表单标签




















  [1]: ./images/Web%E5%89%8D%E7%AB%AF%E6%8A%80%E6%9C%AF.jpg "Web前端技术"