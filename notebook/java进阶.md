---
title: java进阶
tags: java,
grammar_cjkRuby: true
---
## 内部类
• 内部类的定义
  将类的定义class xxxx{…}置入一个类的内部即可
  编译器生成xxxx$xxxx这样的class文件
  内部类不能够与外部类同名
• 内部类的使用
  在封装它的类的内部使用内部类，与普通类的使用方式相同
  在其他地方使用
• **类名前要冠以外部类的名字**。
• **在用new创建内部类实例时，也要在 new前面冠以对象变量。**
• 外部对象名.new 内部类名(参数)
**内部类中可以直接访问外部类的字段及方法
即使private也可以**
**如果内部类中有与外部类同名的字段或方法，则可以用
外部类名.this.字段及方法**

内部类与类中的字段、方法一样是外部类的成员，它的前面也可以有
访问控制符和其他修饰符。
访问控制符：public,protected,默认及private。
• **注：外部类只能够使用public修饰或者默认**
final,abstract
## 匿名类
• 匿名类( anonymous class)是一种特殊的内部类
它没有类名，在定义类的同时就生成该对象的一个实例
“一次性使用”的类
## lambda表达式
Lambda表达式（λ expression)的基本写法
(参数）->结果
如 (String s) -> s.length()
如 x->x\*x
如 () -> { System.out.println(“aaa”); }
• 大体上相当于其他语言的“匿名函数”或“函数指针”
• 在Java中它实际上是“ 匿名类的一个实例
积分
### 线程实例
```java
class LambdaRunnable  {
    public static void main(String argv[]) {
		Runnable doIt =  new Runnable(){
			public void run(){ 
				System.out.println("aaa");
			}
		};
		new Thread( doIt ).start();

		Runnable doIt2 = ()->System.out.println("bbb");
		new Thread( doIt2 ).start();

		new Thread( ()->System.out.println("ccc") ).start();
		
    }
}

```
### 积分实例（梯形）
```java
@FunctionalInterface
interface Fun { double fun( double x );}

public class LambdaIntegral
{
	public static void main(String[] args)
	{
		double d = Integral( new Fun(){
			public double fun(double x){ 
				return Math.sin(x); 
			}
		}, 0, Math.PI, 1e-5 );

		d = Integral( x->Math.sin(x),
			0, Math.PI, 1e-5 );
		System.out.println( d );

		d = Integral( x->x*x, 0, 1, 1e-5 );
		System.out.println( d );

	}

	static double Integral(Fun f, double a, double b, double eps)// 积分计算
	{
		int n,k;
		double fa,fb,h,t1,p,s,x,t=0;

		fa=f.fun(a); 
		fb=f.fun(b);

		n=1; 
		h=b-a;
		t1=h*(fa+fb)/2.0;
		p=Double.MAX_VALUE;

		while (p>=eps)
		{ 
			s=0.0;
			for (k=0;k<=n-1;k++)
			{ 
				x=a+(k+0.5)*h;
				s=s+f.fun(x);
			}

			t=(t1+h*s)/2.0;
			p=Math.abs(t1-t);
			t1=t; 
			n=n+n; 
			h=h/2.0;
		}
		return t;
	}

}
```
• Lambda大大地简化了书写
• 在线程的例子中
new Thread( ()->{ … } ).start();
• 在积分的例子中
d = Integral( x->Math.sin(x), 0, 1, EPS );
d = Integral( x->x\*x, 0, 1, EPS );
d = Integral( x->1, 0, 1, EPS );
• 在按钮事件处理中
btn.addActionListener( e->{ … } ) );

### 能写成Lambda的接口的条件
由于Lambda只能表示一个函数，所以
• 能写成Lambda的接口要求包含且最多只能有一个**抽象函数**
• 这样的接口可以（但不强求）用注记
@FunctionalInterface 来表示。称为函数式接口
• 如
```
@FunctionalInterface
interface Fun { double fun( double x );}
``` 
• 它将**代码也当成数据**来处理

## 装箱、枚举、注解
• 大部分是编译器自动翻译的，称为Complier sugar

### 基本类型的包装类
它将基本类型（primitive type）包装成object（引用类型）
int->Interger
共8类：
Boolean,Byte,Short,Character,Integer,Long,Float,Double

### 装箱和拆箱
装箱（Boxing） Interger I = 10;//Interger I = Integer.valueOf(10);
拆箱（Unboxing） int i = I;//int i = I.intValue();

### 枚举
enum Light {Red,Yellow,Green};
Light light = Light.Red;
可以在enum定义体中，添加字段、方法、构造方法
```java
• enum Direction
• {
• EAST("东",1), SOUTH("南",2),
• WEST("西",3), NORTH("北",4);
• private Direction(String desc, int num){
• this.desc=desc; this.num=num;
• }
• private String desc;
• private int num;
• public String getDesc(){ return desc; }
• public int getNum(){ return num; }
• } 
```
