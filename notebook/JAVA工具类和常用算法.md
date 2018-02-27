---
title: JAVA工具类和常用算法
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---
## JAVA语言基础类
java.lang
java.util 实用工具
java.io
java.awt javax.swing GUI类库
java.net 网络功能
java.sql 数据库

• JDK的源代码
安装JDK后即有 src.zip
例如：在 C:\Program Files\Java\jdk\下

### Object类
(1)equals()
==是判断引用是否相等，equals是内容相等
如果要覆盖equals()方法，一般也要覆盖hashCode()方法。

(2)getClass()是final方法，不能被重载
它返回一个对象在运行时所赌赢的类的表示

(3)toString()方法用来返回对象的字符串表示
通过重载toString方法，方便

(5)notify()、notifyAll()、wait()
与线程相关

### 包装类
包装类的特点
• （1）这些类都提供了一些常数
 如Integer.MAX_VALUE（整数最大值）， Double.NaN(非数字)，Double. POSITIVE_INFINITY（正无穷）
等。
• （2）提供了valueOf(String)，toString()
 用于从字符串转换及或转换成字符串。
• （3）通过xxxxValue()方法可以得到所包装的值
 Integer对象的intValue()方法。
• （4）对象中所包装的值是**不可改变**的（immutable）。
 要改变对象中的值只有重新生成新的对象。
• （5）toString(), equals()等方法进行了覆盖。
• 除了以上特点外，有的类还提供了一些实用的方法以方便操作。
 例如，Double类就提供了parseDouble(), max, min方法等

### Math类
……

### System类
`System.getProperty(String name)`方法获得特定的系统属性值
`System.getProperties()`方法获得一个 Properties类的对象，其中包含了所有可用的系统属性信息
在命令行运行Java程序时可使用-D选项添加新的系统属性
如 java –Dvar=value MyProg

``` java
import java.util.*;
class SystemProperties
{
	public static void main(String[] args) 
	{
		Properties props = System.getProperties();
		Enumeration keys = props.propertyNames();
		while(keys.hasMoreElements() ){
			String key = (String) keys.nextElement();
			System.out.println( key + " = " + props.getProperty(key) );
		}
	}
}

```
## 字符串和日期
### String StringBuffer类
字符串可以分为两大类
String类
• 创建之后不会再做修改和变动，即 immutable
StringBuffer、StringBuilder类
• 创建之后允许再做更改和变化
• 其中 StringBuilder是JDK1.5增加的，它是非线程安全的

在循环中使用String的+=可能会带来效率问题

String类的下述方法能创建并返回一个新的String对象实例: 
连接concat, 替换replace,替换全部replaceAll,子串substring, 小写toLowerCase, 大写toUpperCase,去除前后空格trim,返回toString.
查找: endsWith, startsWith, 查找一个字符串出现位置indexOf,，lastIndexOf.
比较: equals, equalsIgnoreCase,
字符及长度: charAt，length.
```java
class TestStringMethod
{
	public static void main(String[] args) 
	{
		String s = new String( "Hello World" );

		System.out.println( s.length() );
		System.out.println( s.indexOf('o') );
		System.out.println( s.indexOf("He") );
		System.out.println( s.startsWith("He") );
		System.out.println( s.equals("Hello world") );
		System.out.println( s.equalsIgnoreCase("Hello world") );
		System.out.println( s.compareTo("Hello Java") );
		System.out.println( s.charAt(1) );
		System.out.println( s.substring(0,2) );
		System.out.println( s.substring(2) );
		System.out.println( s.concat("!!!") );
		System.out.println( s.trim() );
		System.out.println( s.toUpperCase() );
		System.out.println( s.toLowerCase() );
		System.out.println( s.replace('o', 'x' ) );
		
		System.out.println( s );  //注意，s本身没有改变
	}
}
```
```
11
4
0
true
false
true
13
e
He
llo World
Hello World!!!
Hello World
HELLO WORLD
hello world
Hellx Wxrld
Hello World

```

StringBuffer类对象保存可修改的Unicode字符序列:
构造方法
StringBuffer()
StringBuffer(int capacity)
StringBuffer(String initialString)
• 实现修改操作的方法:
append, insert, reverse, setCharAt, setLength.
### 字符串的分割
构造
StringTokenizer(String str, String delim);
delim(分隔符);
• 该类的重要方法有：
public int countTokens()；// 分割串的个数
public boolean hasMoreTokens()；// 是否还有分割串
public String nextToken()；// 得到下一分割串

### 日期类
Calendar
 得到一个实例 Calendar.getInstance() //Locale.ZH
 .get(DAY_OF_MONTH) .getDisplayName(DAY_OF_WEEK)
 .set .add(HOUR,1) .roll(MONTH, 5),
 .setTime(date), .getTime()
• Date
 new Date(), new Date(System.currentTimeMillis())
 .setTime(long), .getTime()
• SimpleDateFormat(“yyyy-MM-dd HH:mm:ss”)
 .format, .parse

