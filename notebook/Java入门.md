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
欢迎使用 **{小书匠}(xiaoshujiang)编辑器**，您可以通过==设置==里的修改模板来改变新建文章的内容。