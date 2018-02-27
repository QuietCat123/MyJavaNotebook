---
title: Java入门
tags: java
grammar_cjkRuby: true
---
## JAVA输入和输出
### 输入
```java
import java.util.Scanner;

public class Input {

	public static void main(String[] args) {
		Scanner scanner = new Scanner(System.in);
		System.out.print("Input your name: ");
		String name = scanner.nextLine();
		System.out.print("Input your age: ");
		int age = scanner.nextInt();
		System.out.println("Hi, " + name + ", you are " + age);
	}

}
```
### 输出
```java
public class Output {

	public static void main(String[] args) {
		double d = 3.1415926;
		System.out.println(d);
		System.out.printf("PI = %.2f\n", d);
		System.out.printf("PI = %7.2f\n", d);
		// 格式化小数:
		double f = 0.123456;
		System.out.printf("%f\n", f); // 0.123456
		System.out.printf("%e\n", f); // 1.234560e-01
		System.out.printf("%.2f\n", f); // 0.12
		System.out.printf("%6.2f\n", f); // <space><space>0.12
		System.out.printf("%+.2f\n", f); // +0.12
		// 调整参数顺序:
		System.out.printf("%s %s %s\n", "A", "B", "C");
		System.out.printf("%2$s %1$s %1$s %3$s\n", "A", "B", "C");
		// 参数不够，报错:
		System.out.printf("%s %s %s\n", "A", "B");
	}

}

```

Java中可以采用三种注释方式：
• // 用于单行注释。注释从//开始，终止于行尾；
• /\* … \*/ 用于多行注释。注释从/\*开始，到\*/结束，且这种注释不能
互相嵌套；
• /\*\* … \*/ 是Java所特有的doc注释。它以/\*\*开始，到\*/结束。

## 数组
Java语言中声明数组时不能指定其长度(数组中元素的个数)，例如：
`• int a[5]; //非法`
• 数组是引用类型
 `int [ ] a = new int[5];`
 这里 a 只是一个引用
数组是引用类型，
• 数组一经分配空间，其中的每个元素也被按照成员变量同样的方式被
隐式初始化。例如：
( 数值类型是0， 引用类型是null )
• int []a= new int[5];
• //a[3]则是0

Enhanced for语句可以方便地处理数组、集合中各元素
• 如：
```
int[] ages = new int[10];
for ( int age ： ages )
{
	System.out.println( age );
}
```
这种语句是**只读式**的遍历

Array.Copy方法提供了数组元素复制功能：
Array.Copy( source, 0, dest, 0, source.Length );