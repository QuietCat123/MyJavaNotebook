---
title: java面向对象
tags: 面向对象
grammar_cjkRuby: true
---
本章介绍Java中面向对象的程序设计的基本方法，包括类的定义、类的继承、
包、访问控制、修饰符、接口等方面的内容。
• 4.1 类、字段、方法
• 4.2 类的继承
• 4.3 包
• 4.4 访问控制符
• 4.5 非访问控制符
• 4.6 接口
• 4.7 枚举

## 类、字段、方法
字段（field）是类的属性，是用变量来表示的。
字段又称为域、域变量、属性、成员变量等
方法（method）是类的功能和操作, 是用函数来表示的

### 构造方法
构造方法（constructor )是一种特殊的方法
• 用来初始化（new)该类的一个新的对象
• 构造方法和类名同名，而且不写返回数据类型。
``` 
Person( String n, int a ){
	name = n;
	age = a;
}
```
### 方法重载……
### 继承（inheritance)
单继承，使用**extends**关键字实现
```
class Student extends Person{
...
}
```
**super的使用**
使用super访问父类域和方法，如Student有一个域age,可以用super.age来访问。而调用和父类同名的方法时也可以使用。

构造方法是不能继承的，
但在子类中可以用super调用父类构造方法。
```java
Student(String name, int age, String school ){
	super( name, age );
	this.school = school;
}
```

子类对象可以被视为其父类的一个对象
父类对象不能被当做其某一个子类的对象。

## 包
### package语句
package pkg1[.pkg2[.pkg3…]];
• 包及子包的定义，实际上是为了解决名字空间、名字冲突
它与类的继承没有关系。事实上，一个子类与其父类可以位于不同的包中。
• 包有两方面的含义
一是名字空间、存储路径（文件夹）、
一是可访问性（同一包中的各个类，**默认情况下可互相访问**）
• 包层次的根目录是由环境变量CLASSPATH来确定的。
• 在简单情况下，没有package语句，这时称为无名包（unnamed
package）

使用javac可以将.class文件放入到相应的目录，只需要使用一个命令
选项-d来指明包的根目录即可。
` javac -d d:\tang\ch04 d:\tang\ch04\pk\TestPkg.java`
` javac -d . pk\*.java`
• 其中，“.”表示当前目录
• 运行该程序，需要指明含有main的类名：
• java pk.TestPkg
### CLASSPATH
在编译和运行程序中，经常要用到多个包，怎样指明这些包的根目录呢？
简单地说，包层次的根目录是由环境变量CLASSPATH来确定的。具体操作
有两种方法。
• 一是在java及javac命令行中，用-classpath（或-cp)选项来指明，如：
• java –classpath d:\tang\ch04;c:\java\classes;.
pk.TestPkg
• 二是设定classpath环境变量，用命令行设定环境变量，如：
• set classpath= d:\tang\ch04;c:\java\classes;.

## 访问控制符
### 修饰符
修饰符分为两类：
* 访问修饰符（access modifiers)：public/private
* 其他修饰符 abstact

|           | 同一个类中 | 同一个包中 | 不同包中的*子类* | 不同包中的非子类 |
| --------- | ---------- | ---------- | ------------------ | ---------------- |
| private   | Y          |            |                    |                  |
| 默认      | Y          | Y          |                    |                  |
| *protected* | Y          | Y          | *Y*                  |                  |
| public    | Y          | Y          | Y                  | Y                |
类：public或者默认
如果类用public修饰，则该类可以被其他类所访问；
若类默认访问控制符，则该类只能被同包中的类访问。
### setter与getter
将字段用private修饰，从而更好地将信息进行封装和隐藏。
• 用setXXXX和getXXXX方法对类的属性进行存取，分别称为setter与getter。
• 这种方法有以下优点
（1）属性用private更好地封装和隐藏，外部类不能随意存取和修改。
（2）提供方法来存取对象的属性，在方法中可以对给定的参数的合法性进行检验。
（3）方法可以用来给出计算后的值。
（4）方法可以完成其他必要的工作（如清理资源、设定状态，等等）。
（5）只提供getXXXX方法，而不提供setXXXX方法，可以保证属性是只读的。

``` stylus
class Person2
{
	private int age;
	public void setAge( int age ){
		if (age>0 && age<200) this.age = age;
	}
	public int getAge(){
		return age;
	}
}
```
## 其他修饰符

|          | 基本含义   | 修饰类         | 修饰成员 | 修饰局部变量 |
| -------- | ---------- | -------------- | -------- | ------------ |
| static   | 静态的     | 可以修饰内部类 | Y        |              |
| final    | 不可改变的 | Y              | Y        | Y            |
| abstract | 抽象的     | Y              | Y        |              |
### static 字段
静态字段最本质的特点是：
它们是类的字段，不属于任何一个对象实例。
• 它不保存在某个对象实例的内存区间中，而是保存在类的内存区域的
公共存储单元。
• 类变量可以通过类名直接访问，也可以通过实例对象来访问，两种方法的结果是相同的
如System类的in和out对象，就是属于类的域，直接用类名来访问，
即System.in和System.out。
```
class Person{
	static long totalNum;
	int age;
	String Name;
}
```
totalNum代表人类的总人数，它与具体对象实例无关。可以有两种方法来
访问：Person.totalNum和p.totalNum (假定p是Person对象)。
• 在一定意义上，可以用来表示全局变量
### static 方法
用static修饰符修饰的方法仅属于类的静态方法，又称为类方法
而不用的是实例方法
类方法属于整个类。
1. 非static 方法属于某个对象。static属于整个类
2. static属于整个类，因此不能操纵处理属于某个对象的成员变量，只能处理整个类的成员变量，即static方法只能调用static方法
3. static方法中，不能使用this或super
4. 调用方法可以用类名也可以用对象
### final
final说明该类不能被继承
final方法说明不能被子类覆盖
final字段只能被赋值一次（常量

### abstract
凡是用abstract修饰符修饰的类被称为抽象类。
抽象类不能被实例化
抽象类中可以包含抽象方法，也可以不包含abstract方法。但是，一旦某个类中包含
了abstract方法，则这个类必须声明为abstract类。
对抽象方法只需声明，而不需实现，即用分号（；）而不是用{}

## 接口
接口，某种特征的约定
* 定义接口 interface
• 所有方法都自动是public abstract的
* 实现接口 implements
• 可以实现多继承
• 与类的继承关系无关
```
• interface Collection {
	void add (Object obj);
	void delete (Object obj);
	Object find (Object obj);
	int size ( );
• }
```
完整的接口声明如下：
```
[public] interface interfaceName [extends listOfSuperInterface]{
……
}
```
extends 子句与类声明中的extends子句基本相同，不同的是一个接口可以有多个父接口，
用逗号隔开，而一个类只能有一个父类。子接口继承父接口中所有的常量和方法。
接口体中可以包含常量定义
• 常量定义的格式为：type NAME = value;
• 其中type可以是任意类型，NAME是常量名，通常用大写，value是常量值。
• 在接口中定义的常量可以被实现该接口的多个类共享，它与 C中用#define以及C++中用const定义的常量是相同的。
• 在接口中定义的常量具有public, static, final的属性。
## 总结
```java
[public] [abstact|final] class className [extends superclassName][implements InterfaceNameList]
{ //类声明
	[public | protected | private] [static] [final] [transient] [volatile] type variableName;
	//成员变量声明，可为多个
	[public | protected | private] [static] [final | abstract] [native][synchronized]
	returnType methodName ( [paramList] ) //方法定义及实现，可为多个
	[throws exceptionList]{
		statements
	}
}
```