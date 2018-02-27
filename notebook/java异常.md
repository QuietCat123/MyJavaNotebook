---
title: java异常
tags: java,异常
grammar_cjkRuby: true
---

> 原文来自：http://www.cnblogs.com/lulipro/p/7504267.html

总体上我们根据Javac对异常的处理要求，将异常类分为2类。
![enter description here][1]
非检查异常（unckecked exception）：Error 和 RuntimeException 以及他们的子类。javac在编译时，不会提示和发现这样的异常，不要求在程序处理这些异常。所以如果愿意，我们可以编写代码处理（使用try…catch…finally）这样的异常，也可以不处理。对于这些异常，我们应该修正代码，而不是去通过异常处理器处理 。这样的异常发生的原因多半是代码写的有问题。如除0错误ArithmeticException，错误的强制类型转换错误ClassCastException，数组索引越界ArrayIndexOutOfBoundsException，使用了空对象NullPointerException等等。

检查异常（checked exception）：除了Error 和 RuntimeException的其它异常。javac强制要求程序员为这样的异常做预备处理工作（使用try…catch…finally或者throws）。在方法中要么用try-catch语句捕获它并处理，要么用throws子句声明抛出它，否则编译不会通过。这样的异常一般是由程序的运行环境导致的。因为程序可能被运行在各种未知的环境下，而程序员无法干预用户如何使用他编写的程序，于是程序员就应该为这样的异常时刻准备着。如SQLException , IOException,ClassNotFoundException 等。
```java
package com.example;
import java. util .Scanner ;
public class AllDemo
{
      public static void main (String [] args )
      {
            System . out. println( "----欢迎使用命令行除法计算器----" ) ;
            CMDCalculate ();
      }
      public static void CMDCalculate ()
      {
            Scanner scan = new Scanner ( System. in );
            int num1 = scan .nextInt () ;
            int num2 = scan .nextInt () ;
            int result = devide (num1 , num2 ) ;
            System . out. println( "result:" + result) ;
            scan .close () ;
      }
      public static int devide (int num1, int num2 ){
            return num1 / num2 ;
      }
}
/*****************************************
 
----欢迎使用命令行除法计算器----
0
Exception in thread "main" java.lang.ArithmeticException : / by zero
     at com.example.AllDemo.devide( AllDemo.java:30 )
     at com.example.AllDemo.CMDCalculate( AllDemo.java:22 )
     at com.example.AllDemo.main( AllDemo.java:12 )
 
----欢迎使用命令行除法计算器----
r
Exception in thread "main" java.util.InputMismatchException
     at java.util.Scanner.throwFor( Scanner.java:864 )
     at java.util.Scanner.next( Scanner.java:1485 )
     at java.util.Scanner.nextInt( Scanner.java:2117 )
     at java.util.Scanner.nextInt( Scanner.java:2076 )
     at com.example.AllDemo.CMDCalculate( AllDemo.java:20 )
     at com.example.AllDemo.main( AllDemo.java:12 )
*****************************************/
```

上面的代码不使用异常处理机制，也可以顺利编译，因为2个异常都是非检查异常。但是下面的例子就必须使用异常处理机制，因为异常是检查异常。

----------

代码中我选择使用throws声明异常，让函数的调用者去处理可能发生的异常。但是为什么只throws了IOException呢？因为FileNotFoundException是IOException的子类，在处理范围内。
```java
@Test
public void testException() throws IOException
{
    //FileInputStream的构造函数会抛出FileNotFoundException
    FileInputStream fileIn = new FileInputStream("E:\\a.txt");
     
    int word;
    //read方法会抛出IOException
    while((word =  fileIn.read())!=-1) 
    {
        System.out.print((char)word);
    }
    //close方法会抛出IOException
    fileIn.close;
}
```
## 异常处理的基本语法
在编写代码处理异常有两种方法
1. 使用try…catch…finally语句块处理它。或者，
2. 在函数签名中使用throws 声明交给函数调用者caller去解决。

### try…catch…finally语句块
```java
try{
     //try块中放可能发生异常的代码。
     //如果执行完try且不发生异常，则接着去执行finally块和finally后面的代码（如果有的话）。
     //如果发生异常，则尝试去匹配catch块。
 
}catch(SQLException SQLexception){
    //每一个catch块用于捕获并处理一个特定的异常，或者这异常类型的子类。Java7中可以将多个异常声明在一个catch中。
    //catch后面的括号定义了异常类型和异常参数。如果异常与之匹配且是最先匹配到的，则虚拟机将使用这个catch块来处理异常。
    //在catch块中可以使用这个块的异常参数来获取异常的相关信息。异常参数是这个catch块中的局部变量，其它块不能访问。
    //如果当前try块中发生的异常在后续的所有catch中都没捕获到，则先去执行finally，然后到这个函数的外部caller中去匹配异常处理器。
    //如果try中没有发生异常，则所有的catch块将被忽略。
 
}catch(Exception exception){
    //...
}finally{
    
    //finally块通常是可选的。
   //无论异常是否发生，异常是否匹配被处理，finally都会执行。
   //一个try至少要有一个catch块，否则， 至少要有1个finally块。但是finally不是用来处理异常的，finally不会捕获异常。
  //finally主要做一些清理工作，如流的关闭，数据库连接的关闭等。 
}
```
**需要注意的地方**

1、try块中的局部变量和catch块中的局部变量（包括异常变量），以及finally中的局部变量，他们之间不可共享使用。

2、每一个catch块用于处理一个异常。异常匹配是按照catch块的顺序从上往下寻找的，只有第一个匹配的catch会得到执行。匹配时，不仅运行精确匹配，也支持父类匹配，因此，如果同一个try块下的多个catch异常类型有父子关系，应该将子类异常放在前面，父类异常放在后面，这样保证每个catch块都有存在的意义。

3、java中，异常处理的任务就是将执行控制流从异常发生的地方转移到能够处理这种异常的地方去。也就是说：当一个函数的某条语句发生异常时，这条语句的后面的语句不会再执行，它失去了焦点。执行流跳转到最近的匹配的异常处理catch代码块去执行，异常被处理完后，执行流会接着在“处理了这个异常的catch代码块”后面接着执行。
有的编程语言当异常被处理后，控制流会恢复到异常抛出点接着执行，这种策略叫做：resumption model of exception handling（恢复式异常处理模式 ）
而Java则是让执行流恢复到处理了异常的catch块后接着执行，这种策略叫做：termination model of exception handling（终结式异常处理模式）

### throws 函数声明

throws声明：如果一个方法内部的代码会抛出检查异常（checked exception），而方法自己又没有完全处理掉，==则javac你应保证必须在方法的签名上使用throws关键字声明这些可能抛出的异常==，否则编译不通过。

throws是另一种处理异常的方式，它不同于try…catch…finally，throws**仅仅是将函数中可能出现的异常向调用者声明，而自己则不具体处理**。

采取这种异常处理的原因可能是：方法本身不知道如何处理这样的异常，或者说让调用者处理更好，调用者需要为可能发生的异常负责

```
public void foo() throws ExceptionType1 , ExceptionType2 ,ExceptionTypeN
{ 
     //foo内部可以抛出 ExceptionType1 , ExceptionType2 ,ExceptionTypeN 类的异常，或者他们的子类的异常对象。
}
```
### finally块
finally块不管异常是否发生，只要对应的try执行了，则它一定也执行。只有一种方法让finally块不执行：System.exit()。因此finally块通常用来做资源释放操作：关闭文件，关闭数据库连接等等。

良好的编程习惯是：在try块中打开资源，在finally块中清理释放这些资源。

**需要注意的地方:**

1、finally块没有处理异常的能力。处理异常的只能是catch块。

2、在同一try…catch…finally块中 ，如果try中抛出异常，且有匹配的catch块，则先执行catch块，再执行finally块。如果没有catch块匹配，则先执行finally，然后去外面的调用者中寻找合适的catch块。

3、在同一try…catch…finally块中 ，try发生异常，且匹配的catch块中处理异常时也抛出异常，那么后面的finally也会执行：首先执行finally块，然后去外围调用者中寻找合适的catch块。

这是正常的情况，但是也有特例。关于finally有很多恶心，偏、怪、难的问题，我在本文最后统一介绍了，电梯速达->：finally块和return

## throw 异常抛出语句
程序员也可以通过throw语句手动显式的抛出一个异常。throw语句的后面必须是一个异常对象。

throw 语句必须写在函数中，执行throw 语句的地方就是一个异常抛出点，它和由JRE自动形成的异常抛出点没有任何差别。
**异常最先发生的地方，叫做异常抛出点**
![enter description here][2]
``` java
public void save(User user)
{
      if(user  == null) 
          throw new IllegalArgumentException("User对象为空");
      //......
```
## 异常的链化
在一些模块化的软件开发中，一旦一个地方发生异常，将导致一连串的异常。假设B模块完成自己的逻辑需要调用A模块的方法，如果A模块发生异常，则B也将不能完成而发生异常，但是B在抛出异常时，会将A的异常信息掩盖掉，这将使得异常的根源信息丢失。
异常的链化可以将多个模块的异常串联起来，使得异常信息不会丢失。

**异常链化:以一个异常对象为参数构造新的异常对象**。新的异常对象将包含先前异常的信息。这项技术主要是异常类的一个带Throwable参数的函数来实现的。这个当做参数的异常，我们叫他根源异常（cause）。

查看Throwable类源码，可以发现里面有一个Throwable字段cause，就是它保存了构造时传递的根源异常参数。**这种设计和链表的结点类设计如出一辙**，因此形成链也是自然的了。

``` java
public class Throwable implements Serializable {
    private Throwable cause = this;
   
    public Throwable(String message, Throwable cause) {
        fillInStackTrace();
        detailMessage = message;
        this.cause = cause;
    }
     public Throwable(Throwable cause) {
        fillInStackTrace();
        detailMessage = (cause==null ? null : cause.toString());
        this.cause = cause;
    }
    
    //........
} 
```
## 自定义异常
如果要自定义异常类，则扩展Exception类即可，因此这样的自定义异常都属于检查异常（checked exception）。如果要自定义非检查异常，则扩展自RuntimeException。

按照国际惯例，自定义的异常应该总是包含如下的构造函数：

 - 一个无参构造函数
 - 一个带有String参数的构造函数，并传递给父类的构造函数。
 - 一个带有String参数和Throwable参数，并都传递给父类构造函数 一个带有Throwable
 - 参数的构造函数，并传递给父类的构造函数。
例如IOException的源代码作为参考
``` java
public class IOException extends Exception
{
    static final long serialVersionUID = 7818375828146090155L;

    public IOException()
    {
        super();
    }

    public IOException(String message)
    {
        super(message);
    }

    public IOException(String message, Throwable cause)
    {
        super(message, cause);
    }

    
    public IOException(Throwable cause)
    {
        super(cause);
    }
}
```
## finally块和return
首先一个不容易理解的事实：在 try块中即便有return，break，continue等改变执行流的语句，finally也会执行。

省略（）

上面的3个例子都异于常人的编码思维，因此我建议：

*   不要在fianlly中使用return。
*   不要在finally中抛出异常。
*   减轻finally的任务，不要在finally中做一些其它的事情，finally块仅仅用来释放资源是最合适的。
*   将尽量将所有的return写在函数的最后面，而不是try ... catch ... finally中。

打印方法调用栈：printStackTrace()

用throw语句抛出异常

转换异常时注意保留原始异常信息

抛出异常前会保证执行finally

finally如果抛出异常会导致suppressed exception

获取所有异常信息：getSuppressed()


  [1]: ./images/java%E5%BC%82%E5%B8%B8%E5%88%86%E7%B1%BB.png "java异常分类"
  [2]: ./images/%E5%BC%82%E5%B8%B8%E6%8A%9B%E5%87%BA%E7%82%B9.png "异常抛出点"