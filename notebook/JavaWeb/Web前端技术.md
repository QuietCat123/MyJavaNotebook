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
\<br> 	插入换行符。
\<img>	定义图像。
\<hr>	定义水平线。
\<link>	定义资源引用。
\<meta>	定义元信息。
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
### 表单标签\<form>
表单标签是允许用户在表单中(例如文字域,下拉列表.单选框,复选框等)输入信息元素,表单使用标签\<form>定义
```HTML
< form action = "跳转页面" method = "get"|"post" name = "表单名称" target  = "打开方式" enctype = "multipart/form - data">
表单数据项部分
</form>
```
* action属性是提交表单后跳转的目标页面,
* method的取值为get或Post,默认为get.用get方式提交的信息会显示到地址栏,post方式提交则不会.
* target属性指定打开的目标页面方式,其值可以为\_blank/\_parent等
* enctype,用于表单具有上传功能时,且属性值为multipart/form=data,可选属性

### 输入域标签\<input>
在表单\<form>中使用
`< input type = "类型" name = "输入项名称" value="输入项值"/>`
type可以取
* text:表示单行文本框
* radio:单选框,可以用check属性等
* hidden:表示输入界面不可见,表单直接将\<input>中的value属性提交各服务器
* password:密码输入
* file文件上传,则\<form>中还要设置enctype属性为"multipart/form-data"
* checkbox:复选框,支持多选
* email 提交表单时进行email合法性校验
* url 同理,url校验
* number 通过min,max设置接受范围
* range 一定范围内数字值输入域,外观为滑动条,min,max
* data pickers(date,month,week,time,datetime,datetime-locak),其中Imput值可以为括号中任意一个
* search搜索
* submit,提交按钮,单击后服务器可获取提交的数据
* reset,重置按钮,清空表单数据项.

### 下拉列表标签\<select>和选项标签\<option>
表单中使用
```
<select>
<option value = "value1">选项1</option>
<option value = "value2">选项2</option>
</select>
```
### 文本域标签\<textarea>
表单中使用,用于输入多行文本
`<textarea rows="行数"cols="列数" name ="名称">文本内容</textarea>`
### 块标签\<div>
将文档分割为独立的不同的部分.
```
<div position=absolute|relative visibility=visible|hidden|inherit top="像素:right="pixiv"bottom="pixiv"left=""margin="pixiv"height="p"width="p">
文本块
</div>
```
### 超链接标签\<a>
```
<a href="目标页面"target="打开方式">超链接名称或图片</a>
```
target:
* _self默认,当前浏览器窗口
* _blank新浏览器窗口
* _parent _top frmename用于框架,父窗体,整个页面,指定页面framename中打开目标页面

## HTML注释
`<! -- 这是HTML注释 -->`

# CSS样式表
html负责内容的展示,CSS负责内容样式的定义.通过HTML的代码以你用独立的css样式表,可以使内容和样式分离.CSS使一种能够真正做到网页表现与内容分离的样式设计语言.相对于传统HTML的表现而言更绚丽……()
## CSS样式表的定义和引用
### CSS样式表的定义形式
CSS样式表是针对指定对象进行样式修饰的:
```css
div{
	margin:10px;
	background:#FFF;
	text - align:center;
}
```
修饰的是\<div>元素,二margin等表示属性……
对象最为重要,选择器(selector)来指定对象.定义选择器的语法格式如下.
```css
selector{
属性1:属性值1
}
```
### 样式表的引用
4种方法:链接外部,内部,导入内部样式表,内嵌样式
1. 外部样式表

当同一种样式应用于多个页面,最好选择.
可以通过改变一个文件来该表整个站点的外观
在页面中用\<link>标签链接外部样式表:
```html
<head>
<link rel = "stylesheet" type = "text/css"href="style.css"/>
</head>
```
浏览器从style.css中读取样式声明

2. 内部样式表

把样式表放到\<head>区域,只能用到该页面中
用\<style>标签插入:
```html
<style>
	样式声明(如p{margin-left:10px;})
	body{background-image:url("images/bg1.png");}
</style>
```
3. 导入外部样式表

在内部样式表的\<style>标签区域用@import命令导入外部样式表
```html
<style>
	@import "外部样式表.css";
</style>
```
4. 内嵌样式
当样式只在一个元素上应用一次时使用
`<p style="margin-left:20px;color:red">This is a paragragh.</p>`

## CSS常用选择器
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>CSS选择器</title>
<style type="text/css">
/* 元素选择器 */
p{
	color:red;
	font-size:18px;
	text-align:left;
}
/* 类选择器.class */
.p2{
	color:green;
}
/* id选择器 */
#p3{
	color:blue;
}
h2,h5{
	font-wight:bold;
	color:red;
}
</style>
</head>
<body>
<h2>静夜思</h2>
<h5>——李白</h5>
<p>床前明月光</p>
<p class="p2">疑是地上霜</p>
<p id="p3">举头望明月</p>
<p>低头思故乡</p>
</body>
</html>
```
![enter description here][2]

## CSS常用属性
### 文本属性

![文本属性][3]

### 字体属性

![字体属性][4]

### 背景属性

![背景属性][5]

# Java Script概述
基于浏览器的脚本语言,无需编译就可以在浏览器中直接运行.
基于JavaScript的第三方函数库:jQuery,Ext JS等.
可以直接嵌入网页,也可以保存为.js文件
## 语法基础
### 数据类型
对数据类型无严格要求……
### 变量常量
拥有动态类型
```javascript
var x; //声明x
x = 100;
x = "JavaScript"
//数组声明
//静态
var array = ["a","b","c"]
var array1 = new Array();
var array2 = new Array(4);
var array3 = new Array("Java",5,true,null);
```
### 运算符,控制语句基本与JAVA一致(

## JavaScript事件
事件是指文档或浏览器窗口与用户交互发生了特定的动作.
如:

![JavaScript部分事件][6]

```html
<标签 事件处理程序 = "函数名称(参数)">
```
## JavaScript函数
声明函数语法:
```JavaScript
function 函数名称(参数列表)
{
	函数体
}
```
声明函数的方法:
* 内嵌式
* 外部连接式

### 内嵌式
```html
<head>
	<script type="text/javascripts">
	function funcname(参数列表)
	{
		函数体
	}
	</script>
</head>
```

### 外部链接式
```HTML
<script type="text/javascript" src="xxx.js">
```

## DOM对象……

# jQuery与AJAX技术
## jQuery
```html
<!DOCTYPE html>
<html>
<head>
<meta content="text/html" charset="UTF-8" http-equiv="Content-Type">
<script src="jquery/jquery-3.3.1.js" type="text/javascript"></script>
<!--  以下是jquery程序段  -->
<script>
$(document).ready(function(){
	$("p").click(function(){
		$(this).hide();
	})
});
</script>
<title>第一个jquery程序</title>
</head>
<body>
<p>如果你单击我我就会消失</p>
</body>
</html>
```
$(document)表示html文档对象,$(document).ready()方法只是$(document)的ready事件处理函数.在文档对象就绪时被触发
$()时jQuery()方法的缩写,可以在DOM中搜索指定的选择器匹配的元素.,并引用该元素的jquery对对象.例如
$("p")表示文档中\<p>元素,$("p").click()方法指\<P>的click事件处理函数
$(this),这里指当前引用的HTML对象,hide()是隐藏当前对象.

## jQuery选择器
和CSS一样
$("#id") 返回单个元素
$("element") 返回集合元素
$(".class") ……
$("[property]") 选择所有属性名为property的元素
全局选择器 "*","多个"











  [1]: ./images/Web%E5%89%8D%E7%AB%AF%E6%8A%80%E6%9C%AF.jpg "Web前端技术"
  [2]: ./images/CSS%E9%80%89%E6%8B%A9%E5%99%A8%E5%AE%9E%E4%BE%8B.png "CSS选择器实例"
  [3]: ./images/1519911594187.jpg
  [4]: ./images/1519911786304.jpg
  [5]: ./images/1519911874170.jpg
  [6]: ./images/scanner_20180301_215615.jpg