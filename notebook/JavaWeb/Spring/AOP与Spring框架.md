---
title: AOP与Spring框架
tags: xml,aop,spring
grammar_cjkRuby: true
---

![enter description here][1]

## 基于xml的实现
首先需要按照描述导入springAOP模式import xmlns:aop 和 xsi:schemaLocation
```xml
<?xml version = "1.0" encoding = "UTF-8"?>
<beans xmlns = "http://www.springframework.org/schema/beans"
   xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance" 
   xmlns:aop = "http://www.springframework.org/schema/aop"
   xsi:schemaLocation = "http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd 
   http://www.springframework.org/schema/aop 
   http://www.springframework.org/schema/aop/spring-aop-3.0.xsd ">

   <!-- bean definition & AOP specific configuration -->

</beans>

        <!-- aspectj -->
        <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjrt -->
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjrt</artifactId>
            <version>1.8.13</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.8.13</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjtools -->
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjtools</artifactId>
            <version>1.8.13</version>
        </dependency>
```


## 声明一个方面
ref属性引用辅助bean
```xml
<aop:config>
   <aop:aspect id = "myAspect" ref = "aBean">
      ...
   </aop:aspect>
</aop:config>

<bean id = "aBean" class = "...">
   ...
</bean>
```


## 声明一个切入点

一个**切入点**有助于确定不同的意见执行感兴趣的连接点（即方法）。在使用基于XML模式的配置时，切入点将被定义如下 -

```xml
<pre class="prettyprint notranslate prettyprinted"><aop:config>
   <aop:aspect id = "myAspect" ref = "aBean">
      <aop:pointcut id = "businessService" 
         expression = "execution(*com.xyz.myapp.service.*.*(..))"/>
         ...
   </aop:aspect>
</aop:config>

<bean id = "aBean" class = "...">
   ...
</bean></pre>
```
以下示例定义了一个名为'businessService'的切入点，该切入点将与com.tutorialspoint下的Student类中可用的getName（）方法的执行相匹配 -
```xml
<pre class="prettyprint notranslate prettyprinted"><aop:config>
   <aop:aspect id = "myAspect" ref = "aBean">
      <aop:pointcut id = "businessService" 
         expression = "execution(*com.tutorialspoint.Student.getName(..))"/>
         ...
   </aop:aspect>
</aop:config>

<bean id = "aBean" class = "...">
   ...
</bean></pre>
```
## 声明建议

您可以使用<aop：{ADVICE NAME}>元素在<aop：aspect>中声明五个建议中的任何一个，如下所示 -
```xml
<pre class="prettyprint notranslate prettyprinted"><aop:config>
   <aop:aspect id = "myAspect" ref = "aBean">
      <aop:pointcut id = "businessService"
         expression = "execution(* com.xyz.myapp.service.*.*(..))"/>

      <!-- a before advice definition -->
      <aop:before pointcut-ref = "businessService" method = "doRequiredTask"/>

      <!-- an after advice definition -->
      <aop:after pointcut-ref = "businessService" method = "doRequiredTask"/>

      <!-- an after-returning advice definition -->
      <!--The doRequiredTask method must have parameter named retVal -->
      <aop:after-returning pointcut-ref = "businessService"
         returning = "retVal" method = "doRequiredTask"/>

      <!-- an after-throwing advice definition -->
      <!--The doRequiredTask method must have parameter named ex -->
      <aop:after-throwing pointcut-ref = "businessService"
         throwing = "ex" method = "doRequiredTask"/>

      <!-- an around advice definition -->
      <aop:around pointcut-ref = "businessService" method = "doRequiredTask"/>
      ...
   </aop:aspect>
</aop:config>

<bean id = "aBean" class = "...">
  ...
</bean></pre>
```
您可以针对不同的建议使用相同的**doRequiredTask**或不同的方法。这些方法将被定义为方面模块的一部分。


## 基于XML架构的AOP示例

为了理解上面提到的与基于XML Schema的AOP相关的概念，让我们编写一个实现几个建议的例子。为了编写我们的例子，我们提供了很少的建议，让我们有一个可用的Eclipse IDE，并采取以下步骤来创建一个Spring应用程序 -

| 步 | 描述 |
| 1 | 创建一个名称为_SpringExample_的项目，并在创建的项目的**src**文件夹下创建一个_com.tutorialspoint_包。 |
| 2 | 按照_Spring Hello World Example_章节的介绍，使用_Add External JARs_选项添加所需的Spring库。 |
| 3 | 在项目中添加Spring AOP特定的库**aspectjrt.jar，aspectjweaver.jar**和**aspectj.jar**。 |
| 4 | 在_com.tutorialspoint_包下创建Java类**Logging**，_Student_和_MainApp_。 |
| 五 | 在**src**文件夹下创建Beans配置文件_Beans.xml_。 |
| 6 | 最后一步是创建所有Java文件和Bean配置文件的内容并按照下面的说明运行应用程序。 |

这里是**Logging.java**文件的内容。这实际上是一个方面模块的样本，它定义了在各个点上调用的方法。
```java
<pre class="prettyprint notranslate prettyprinted">package com.tutorialspoint;

public class Logging {
   /** 
      * This is the method which I would like to execute
      * before a selected method execution.
   */
   public void beforeAdvice(){
      System.out.println("Going to setup student profile.");
   }

   /** 
      * This is the method which I would like to execute
      * after a selected method execution.
   */
   public void afterAdvice(){
      System.out.println("Student profile has been setup.");
   }

   /** 
      * This is the method which I would like to execute
      * when any method returns.
   */
   public void afterReturningAdvice(Object retVal) {
      System.out.println("Returning:" + retVal.toString() );
   }

   /**
      * This is the method which I would like to execute
      * if there is an exception raised.
   */
   public void AfterThrowingAdvice(IllegalArgumentException ex){
      System.out.println("There has been an exception: " + ex.toString());   
   }
}</pre>
```
以下是**Student.java**文件的内容
```java
<pre class="prettyprint notranslate prettyprinted">package com.tutorialspoint;

public class Student {
   private Integer age;
   private String name;

   public void setAge(Integer age) {
      this.age = age;
   }
   public Integer getAge() {
	  System.out.println("Age : " + age );
      return age;
   }
   public void setName(String name) {
      this.name = name;
   }
   public String getName() {
      System.out.println("Name : " + name );
      return name;
   }
   public void printThrowException(){
	   System.out.println("Exception raised");
      throw new IllegalArgumentException();
   }
}</pre>
```
以下是**MainApp.java**文件的内容
```java
<pre class="prettyprint notranslate prettyprinted">package com.tutorialspoint;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MainApp {
   public static void main(String[] args) {
      ApplicationContext context = new ClassPathXmlApplicationContext("Beans.xml");

      Student student = (Student) context.getBean("student");
      student.getName();
      student.getAge();
      student.printThrowException();
   }
}</pre>
```
以下是配置文件**Beans.xml**
```xml
<pre class="prettyprint notranslate prettyprinted"><?xml version = "1.0" encoding = "UTF-8"?>
<beans xmlns = "http://www.springframework.org/schema/beans"
   xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance" 
   xmlns:aop = "http://www.springframework.org/schema/aop"
   xsi:schemaLocation = "http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd 
   http://www.springframework.org/schema/aop 
   http://www.springframework.org/schema/aop/spring-aop-3.0.xsd ">

   <aop:config>
      <aop:aspect id = "log" ref = "logging">
         <aop:pointcut id = "selectAll" 
            expression = "execution(* com.tutorialspoint.*.*(..))"/>

         <aop:before pointcut-ref = "selectAll" method = "beforeAdvice"/>
         <aop:after pointcut-ref = "selectAll" method = "afterAdvice"/>
         <aop:after-returning pointcut-ref = "selectAll" 
            returning = "retVal" method = "afterReturningAdvice"/>

         <aop:after-throwing pointcut-ref = "selectAll" 
            throwing = "ex" method = "AfterThrowingAdvice"/>
      </aop:aspect>
   </aop:config>

   <!-- Definition for student bean -->
   <bean id = "student" class = "com.tutorialspoint.Student">
      <property name = "name" value = "Zara" />
      <property name = "age" value = "11"/>      
   </bean>

   <!-- Definition for logging aspect -->
   <bean id = "logging" class = "com.tutorialspoint.Logging"/> 

</beans></pre>
```
一旦完成创建源和bean配置文件，让我们运行该应用程序。如果你的应用程序一切正常，它会打印下面的消息 -

<pre class="result notranslate">Going to setup student profile.
Name : Zara
Student profile has been setup.
Returning:Zara
Going to setup student profile.
Age : 11
Student profile has been setup.
Returning:11
Going to setup student profile.
Exception raised
Student profile has been setup.
There has been an exception: java.lang.IllegalArgumentException
.....
other exception content
</pre>

上面定义的<aop：pointcut>选择在com.tutorialspoint包下定义的所有方法。让我们假设，您希望在特定方法之前或之后执行您的建议，您可以定义切入点以通过用实际的类和方法名称替换切入点定义中的星号（*）来缩小执行范围。以下是一个修改的XML配置文件来展示这个概念 -
```
<pre class="prettyprint notranslate prettyprinted"><?xml version = "1.0" encoding = "UTF-8"?>
<beans xmlns = "http://www.springframework.org/schema/beans"
   xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance" 
   xmlns:aop = "http://www.springframework.org/schema/aop"
   xsi:schemaLocation = "http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd 
   http://www.springframework.org/schema/aop 
   http://www.springframework.org/schema/aop/spring-aop-3.0.xsd ">

   <aop:config>
      <aop:aspect id = "log" ref = "logging">
         <aop:pointcut id = "selectAll" 
            expression = "execution(* com.tutorialspoint.Student.getName(..))"/>
         <aop:before pointcut-ref = "selectAll" method = "beforeAdvice"/>
         <aop:after pointcut-ref = "selectAll" method = "afterAdvice"/>
      </aop:aspect>
   </aop:config>

   <!-- Definition for student bean -->
   <bean id = "student" class = "com.tutorialspoint.Student">
      <property name = "name" value = "Zara" />
      <property name = "age" value = "11"/>      
   </bean>

   <!-- Definition for logging aspect -->
   <bean id = "logging" class = "com.tutorialspoint.Logging"/> 

</beans></pre>
```
如果您使用这些配置更改执行示例应用程序，它将打印以下消息 -

<pre class="result notranslate">Going to setup student profile.
Name : Zara
Student profile has been setup.
Age : 11
Exception raised
.....
other exception content</pre>




















  [1]: ./images/AOP.jpg "AOP"