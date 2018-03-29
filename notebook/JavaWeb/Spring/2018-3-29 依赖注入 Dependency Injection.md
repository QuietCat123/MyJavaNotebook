---
title: 2018-3-29 依赖注入 Dependency Injection
tags: 依赖注入
grammar_cjkRuby: true
---
## 依赖注入
为什么要有==依赖注入==（一种设计代码模式），因为我们要==控制反转==（设计代码的思路）。
为什么==控制反转==。因为我们软件设计需要符合软件设计原则 ==依赖倒置==（设计代码原则），单一职责原则。

说通俗点，咱们要解耦啊。


简单来说，a依赖b，但a不控制b的创建和销毁，仅使用b，那么b的控制权交给a之外处理，这叫控制反转（IOC），而a要依赖b，必然要使用b的instance，那么
(1) 通过a的接口，把b传入；(接口传递)
(2) 通过a的构造，把b传入；(构造方法传递)
(3)通过设置a的属性，把b传入；(setter方法)
这个过程叫依赖注入（DI）。

那么什么是IOC Container？

随着DI的频繁使用，要实现IOC，会有很多重复代码，甚至随着技术的发展，有更多新的实现方法和方案，那么有人就把这些实现IOC的代码打包成组件或框架，来避免人们重复造轮子。所以实现IOC的组件或者框架，我们可以叫它IOC Container。

> http://insights.thoughtworkers.org/injection/
> 
> 控制反转
> 几位轻量级容器的作者曾骄傲地对我说：这些容器非常有用，因为它们实现了控制反转。这样的说辞让我深感迷惑：控制反转是框架所共有的特征，如果仅仅因为使用了控制反转就认为这些轻量级容器与众不同，就好象在说我的轿车是与众不同的，因为它有四个轮子。
> ////////////////////////////////////////////////////////
> 问题的关键在于：它们反转了哪方面的控制？我第一次接触到的控制反转针对的是用户界面的主控权。早期的用户界面是完全由应用程序来控制的，你预先设计一系列命令，例如输入姓名、输入地址等，应用程序逐条输出提示信息，并取回用户的响应。而在图形用户界面环境下，UI框架将负责执行一个主循环，你的应用程序只需为屏幕的各个区域提供事件处理函数即可。在这里，程序的主控权发生了反转：从应用程序移到了框架。对于这些新生的容器，它们反转的是如何定位插件的具体实现。在前面那个简单的例子中，MovieLister类负责定位MovieFinder的具体实现——它直接实例化后者的一个子类。这样一来，MovieFinder也就不成其为一个插件了，因为它并不是在运行期插入应用程序中的。而这些轻量级容器则使用了更为灵活的办法，只要插件遵循一定的规则，一个独立的组装模块就能够将插件的具体实现注射到应用程序中。因此，我想我们需要给这个模式起一个更能说明其特点的名字——”控制反转”这个名字太泛了，常常让人有些迷惑。与多位IoC
> 爱好者讨论之后，我们决定将这个模式叫做”依赖注入”（Dependency Injection）

## Spring控制反转实现
通过提供一个控制反转容器(IOC Container),Spring为我们提供了一种可以聪明管理JAVA对象依赖关系的方法.

Spring将使程序把几乎所有重要对象的创建工作交给Spring.并配置如何注入依赖.
Spring支持XML和注解两种配置方式.

**此外**,还需要创建一个==ApplicationContext==对象,代表一个Spring控制反转控制容器.
接口==org.springframework.context.ApplicationContext==有多个实现,
包括,==ClassPathXmlApplicationContext==和==FileSystemPathXmlApplicationContext==.
这两个实现这都需要至少一个包含beans信息的XML文件.

例如:从类路径中加载config1.xml和config2.xml
```
ApplicationContext context = new ClassPathXmlApplicationContext(
	new String[] {"config1.xml","config2.xml"});
```
可以通过调用ApplicationContext的getBean获得对象
```
Product product = context.getBean("product",Product.class);
//getBean 查询id为product且类型为Product的bean对象
```

Spring MVC应用,可以通过Spring Servlet来处理ApplicationContext

### XML配置文件
导入其他配置文件的示例:
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop-3.0.xsd">
		
		<import resource = "config.xml"/>
		<import resource = "/resources/config3.xml"/>
</beans>

## Spring如何管理Bean和依赖关系
### 通过构造器创建一个bean实例
```
<beans>...
	<bean name="product" calss="app15a.bean.Product"/>
<beans>

//in java:
Application context = new ClassPathXmlApplicationContext(
		new String[]{"spring-config.xml"})
product product1 = context.getBean("product",Product.class);
product1.setName("miao");
....
```
### 通过工厂方法
```xml
通过工厂方法实例化java.util.Calendar:
<!-- 
    factory-bean：指向实例工厂方法的bean
    factory-method：指向实例工厂方法的名字
    constructor-arg：如果实例工厂方法需要传入参数，则使用constructor-arg来配置参数
-->
<bean id="calendar" class="java.util.Calendar"
	factory-method="getInstance"/>
	
	
//in java:
Application context = new ClassPathXmlApplicationContext(
		new String[]{"spring-config.xml"})
product product1 = context.getBean("calendar",Calendar.class);

```