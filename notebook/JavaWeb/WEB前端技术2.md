--- 
title: WEB前端技术2
tags: WEB,java
grammar_cjkRuby: true
---


# jQuery与AJAX技术
jQuery是一个快速、简洁的JavaScript框架
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

![jQuery选择器][1]

```html
<!doctype html>
<html>
<head>
<meta charset="utf-8">
<title>example2_7</title>

<style>
div{	
	padding:5px}
</style>

<script src="jquery-2.1.4.js" type="text/javascript"></script>
<script type="text/javascript">
$(document).ready(function(){
    //使用类选择器，选择class="left"的对象，设置该对象中的字体为12px
    $(".left").css("box-shadow","inset");
	//使用元素选择器，选择<input>对象，设置这些对象的CSS样式的边框
    $("input").css("border","dashed 1px #000000");
	//使用ID选择器，选择id="message"的对象，设置该对象的CSS样式的边框
	$("#message").css("border","dotted 1px #0cff00");
	//选择所有的对象
	$("*").css("font-size","12px");
	//使用$("span,legend")选择<span>和<legend>两个元素。
	$("span,legend").css("color","#F33");
	//属性选择器，选择name属性值为hobby的复选框，并设置为checked状态。
	$("input[name='hobby']").attr("checked", true);
});
</script>

</head>
<body>
  <form action="#" method="post" id="myform" >
	<fieldset>
		<legend>个人基本信息</legend>
		<div>
			<label for="username" class="left">名称：</label>
			<input type="text" name="username" id="username"/>
            <span id="tips_user">*</span>
		</div>
		<div>
			<label for="password" class="left">密码：</label>
			<input type="password" name="password" id="password" />
            <span id="tips_pass">*</span>
		</div>
        <div>
			<label for="requiredpass" class="left">确认密码：</label>
			<input type="password" name="requiredpass" id="requiredpass" />
            <span id="tips_requiredpass">*</span>
		</div>
        <div>
			<label for="usertype" class="left">用户类型：</label>
			<select name="usertype" id="usertype">
            	<option value="1">管理员</option>
                <option value="2">普通用户</option>
            </select>
            
		</div>
        <div>
			<label for="gender" class="left">性别：</label>
			<input type="radio" name="gender" id="gender" value="男"/>男
            <input type="radio" name="gender" id="gender" value="女"/>女
            <span id="tips_gender">*</span>
		</div>
        
         <div>
			<label for="hobby" class="left">爱好：</label>
			<input type="checkbox" name="hobby" id="hobby" value="reading"/>阅读
            <input type="checkbox" name="hobby" id="hobby" value="music"/>音乐
            <input type="checkbox" name="hobby" id="hobby" value="sports"/>运动
            <input type="checkbox" name="hobby" id="hobby" value="travell"/>旅行
            <span id="tips_hobby">*</span>
		</div>
        <div>
			<label for="email" class="left">电子邮件：</label>
			<input type="text" name="email" id="email"/>
            <span id="tips_email">*</span>
		</div>
		<div>
			<label for="message" class="left">自我介绍：</label>
			<textarea name="message" id="message" ></textarea>
		</div>

        <div><button type="submit" id="submit"> 提交</button>
        <button type="reset">重置</button>
        </div>
	</fieldset>
  </form>

</body>
</html>

```
css()设置或设置选择元素的CSS样式
("input[name=='hobby']).attr("checked",true),选择name为Hobby的设置checked属性为true

表单选择器:
```html
<!doctype html>
<html>
<head>
<meta charset="utf-8">

<title>example2_8</title>

<script src="jquery-2.1.4.js" type="text/javascript"></script>
<script type="text/javascript">
	$(document).ready(function() {
     $("#btn").click(function(){
	 //num1为复选框选项的个数
	 var num1 = $("input:checkbox").length;
	 //num2为复选框选中的项目数
	 //var num2 = $("#form1 input:checked").length;
	 var num2 = $("input[name='hobby']:checked").length
	 var val="";
	 //遍历选中复选框的选项，val为选中项的值，中间用逗号隔开
	 $("input[name='hobby']:checked").each(function(index, element) {
     	val = val + $(this).val() + ",";   
     });
	
     if(val.length>1)
	 {
	     //因为字符串val的尾部有一个逗号，去掉尾部的逗号
		 val = val.substring(0,val.length-1);
	 }
	 val = "<hr>复选框的项目总数为：" + num1 + "<br/>选中的个数：" + num2 + "<br/>选中的项目：" + val;
	 //获取下拉列表框的选择项的文本
	 var selected = $("select :selected").text();
	 //获取选择项的值
	 var items = $("select :selected").val();
	 val = val + "<br><hr>下拉列表框选择项的文本：" + selected;
	 val = val + "<br><hr>下拉表表框选择项的值：" + items;
	 //在<span>标签内输出结果
	 $("#data").html(val);
	 });
	 	 
    });
</script>
</head>

<body>
<form id="form1">
爱好：<input type="checkbox" name="hobby" value="足球" />足球
<input type="checkbox" name="hobby" value="篮球" />篮球
<input type="checkbox" name="hobby" value="网球" />网球
<input type="checkbox" name="hobby" value="排球" />排球
<input type="checkbox" name="hobby" value="乒乓球" />乒乓球
<br/>
用户类别：<select id="type" name="type">
<option value="1">本科生</option>
<option value="2">硕士生</option>
<option value="3">博士生</option>
<option value="4">教师</option>
</select>
<br/>
<input type="button" id="btn" value="单击" /> 
<br/>
<span id="data"></span>
</form>
</body>
</html>

```

## 使用jQuery操作HTML
操作方法列表:
http://www.w3school.com.cn/jquery/jquery_ref_manipulation.asp
http://www.w3school.com.cn/jquery/jquery_ref_attributes.asp

其中attr()方法,prop()方法,val()方法在访问文本框的value属性时用一下三种等式时等价的:
```html
<input type = "text" id="txt" name="data" value="jquery"/>
//使用attr
$["#txt"].attr("value");
//使用val()
$["#txt"].val();
//使用prop()
$["#txt"].prop("value")
```
也可以设置文本框内容:
```html
$["#txt"].val("Hello,jQuery");
$["#txt"].attr("value","Hello");
$["#txt"].prop("value","Hello");
```
## jQuery事件
支持键盘鼠标表单文档加载浏览器事件等
### ready()方法
同个这个方法在DOM载入就绪时立刻调用绑定方法.
```htmlbars
$(document).ready(function(){
	页码代码
	};
```
### bind()方法
bind(type,[data],fn)方法为每个匹配元素的特定事件绑定事件处理方法.
type:一个或多个事件"click"或"submit"等,或自定义事件
[data]:evnet.data属性值传递给事件对象的额外数据对象
fn:绑定到匹配的元素的事件上的处理函数

```HTML
<script src="jquery-2.1.4.js" type="text/javascript"></script>
<script>
$(document).ready(function() {
	$("#Text").bind("click mouseover",{first:"1",second:"2"},function(event){
		if(event.data.first=="1"){$(this).val("jQuery事件");}
		if(event.data.second=="1"){$(this).val("");}
	});
});
</script>
```

## AJAX技术
"asynchronous javascript and xml"异步..和..
可以局部刷新页面,减小负载提高效率
技术核心:XMLHttpRequest对象
### XMLHttpRequest对象(XHR)
是JavaScript的一个对象,能够用HTTP连接一个服务器
使用前先实例化
```htmlbars
< script type = "text\javascript">
	var xmlhttp = new XMLHttpRequest();
</script>
```
主要方法:
* open():设置异步请求目标的URL/请求方法以及其他参数信息
* send():向服务器发送信息,若为异步,该方法立刻返回,否则等到接收到响应为止.
* setRequetHeader()方法:为请求的http报头设置值,必须在调用open后调用
* abort()方法,停止当前异步请求
* getAllResponseHeaders():以字符串完整返回HTTP报头信息
常用属性:
* onreadystatechange 状态发生改变触发,用来调用Javascript函数
* readyState 请求的状态
* responseText 服务器响应,字符串
* responseXML 服务器响应,XML,解析为DOM
* status 返回服务器状态码
```
xmlhttp.open("GET","page.htm",true);//true表示异步
xmlhttpl.send():
```
完整实例:
```htmlbars
<!doctype html>
<html>
<head>
<meta charset="utf-8">

<title>example2_10</title>

<script src="jquery-2.1.4.js" type="text/javascript"></script>
<script type="text/javascript">
function loadXMLDoc()
{
var xmlhttp;
//实例化XMLHttpRequest对象
if (window.XMLHttpRequest)
  {// 针对IE7+, Firefox, Chrome, Opera, Safari等浏览器
  xmlhttp=new XMLHttpRequest();
  }
else
  {// 针对IE5,IE6浏览器
  xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
  }
xmlhttp.onreadystatechange=function()
  {
  //readyState状态为4（服务器请求完成）并且HTTP的状态码为200（成功）时
  if (xmlhttp.readyState==4 && xmlhttp.status==200)
    {
    document.getElementById("txtDiv").innerHTML="显示服务器端doc.txt内容："+xmlhttp.responseText;
    }
  }
//以POST方式请求打开与ajax.htm同目录下的doc.txt文档
xmlhttp.open("POST","doc.txt",true);
//发送请求内容
xmlhttp.send();
}
</script>
</head>
<body>
<div id="txtDiv"><h2>显示服务器端doc.txt内容：</h2></div>
<button type="button" onclick="loadXMLDoc()">通过AJAX读取服务器端内容</button>

</body>
</html>
```
### jQuery封装AJAX
jQuery中AJAX相关方法:
$.AJAX(url.[setting]) 通过HTTP请求加载远程数据,底层实现方法
load(url,[data,fn])载入远程HTML文件代码插入至DOM中
$.get(url,[data,fn]) 通过远程HTTP GET方式请求载入信息
$.post(url,[data,fn]) 通过远程http post方式请求载入信息

下面用jQuery实现AJAX前例
```htmlbars
<!doctype html>
<html>
<head>
<meta charset="utf-8">

<title>example2_11</title>

<script src="jquery-2.1.4.js" type="text/javascript"></script>
<script type="text/javascript">
$(document).ready( function() {
    $("button").click(function() {
		$("#txtDiv").load("doc.txt");
	});
});
</script>
</head>
<body>
<div id="txtDiv"><h2>显示服务器端doc.txt内容：</h2></div>
<button type="button">通过AJAX读取服务器端内容</button>

</body>
</html>
```
# JSON
JavaScript Object Notation
功能上类似于XML,更容易解析,纯文本文件.
## JSON数据语法格式
数据书写格式:
"名称":"值"
数据间用逗号分隔, 大括号保存对象, 中括号保存数组
例如:
```JSON
"school":"华科"
```
## JSON对象
一个对象由多组JSON数值
```JSON
{ "school":"华科",
	"address":"……"
}
```
## JSON数组
```JSON
[ { "school":"华科",
	"address":"……"
}
{ "school":"华科",
	"address":"……"
}
]
```
## JSON文本转化为JavaScript对象
name="application/json"
JavaScript创建包含JSON语法的字符串要用单引号' '把JSON对象包括起来.如:
```javascript
var schools = '{"school":"华科","address":"咕嘿嘿"}';
```
## 使用jQuery操作JSON
`$.getJSON(url,[data],[fn]);`
url发送请求地址
data 待发送key/value参数
fn 载入成功的回调方法
```html
<!doctype html>
<html>
<head>
<meta charset="utf-8">

<title>example2_12</title>
<link rel="stylesheet" href="admin.css" />
<script src="jquery-2.1.4.js" type="text/javascript"></script>
<script>
$(document).ready(function() {
    $("#btn").click(function() {
		$.getJSON("test.json",
		function(data){
			    //定义一个字符串变量html，动态生成表格，初始为表格的标题行。
				var html = '<table> <tr><td>学校</td><td>地址</td><td>邮编</td></tr>';
					//使用$.each()方法遍历JSON数组，数组中的每个元素作为表格的一行，回调函数的参数i为迭代变量。
					$.each(data,function(i){
						html+= '<tr><td>' + data[i].school + '</td><td>' + data[i].address + '</td><td>' + data[i].zip + '</td></tr>';
						});
					//为html变量追加字符串，添加表格的闭合标签。
					html+= '</table>';
				//将字符串变量html显示到resData元素中。
				$('#resData').html(html);	
			});
	});
});
</script>
</head>
<body>
<button id="btn">获取数据</button>
<div id="resData"></div>
<div id="status"></div>
</body>
</html>


```


  [1]: ./images/1520077695163.jpg