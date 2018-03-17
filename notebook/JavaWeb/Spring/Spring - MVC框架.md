---
title: Spring - MVC框架
tags: Spring,MVC
grammar_cjkRuby: true
---

Spring Web MVC框架提供了模型 - 视图 - 控制器（MVC）体系结构和可用于开发灵活和松散耦合的Web应用程序的组件。MVC模式导致分离应用程序的不同方面（输入逻辑，业务逻辑和UI逻辑），同时提供这些元素之间的松散耦合。

*   该**模型**封装了应用程序数据，通常它们将由POJO组成。

*   该**视图**负责呈现模型数据，并在总体上产生HTML输出，客户端的浏览器可以解释。

*   该**控制器**负责处理用户请求，并且建立一个合适的模型，并将其传递到用于呈现该视图。

## DispatcherServlet

Spring Web模型 - 视图 - 控制器（MVC）框架是围绕 _DispatcherServlet_ 设计的，它处理所有HTTP请求和响应。Spring Web MVC  _DispatcherServlet_ 的请求处理工作流程如下图所示 -

![Spring DispatcherServlet](https://www.tutorialspoint.com/spring/images/spring_dispatcherservlet.png)

以下是与传入的 _DispatcherServlet_ HTTP请求相对应的事件序列-

*   在接收到HTTP请求后，_DispatcherServlet_ 会查询 _HandlerMapping_ 以调用相应的 _Controller_。

*   该 _Controller_ 接受请求，并调用基于所使用GET或POST方法相应的服务的方法。服务方法将根据定义的业务逻辑设置模型数据，并将视图名称返回给 _DispatcherServlet_。

*   所述的 _DispatcherServlet_ 将帮助从的  _ViewResolver_ 到拾取该请求的已定义视图。

*   View完成后， _DispatcherServlet_ 将模型数据传递给最终在浏览器中呈现的视图。

上述所有组件，即HandlerMapping，Controller和ViewResolver都是_WebApplicationContext的_一部分，它是普通_ApplicationContext_的扩展，并带有Web应用程序所需的一些额外功能。

## 所需的配置

您需要映射您希望 _DispatcherServlet_ 处理的请求，方法是使用 **web.xml** 文件中的URL映射。以下是显示 **HelloWeb**  _DispatcherServlet_ 示例的声明和映射的示例 -
```xml
<web-app id = "WebApp_ID" version = "2.4"
   xmlns = "http://java.sun.com/xml/ns/j2ee" 
   xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation = "http://java.sun.com/xml/ns/j2ee 
   http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">

   <display-name>Spring MVC Application</display-name>

   <servlet>
      <servlet-name>HelloWeb</servlet-name>
      <servlet-class>
         org.springframework.web.servlet.DispatcherServlet
      </servlet-class>
      <load-on-startup>1</load-on-startup>
   </servlet>

   <servlet-mapping>
      <servlet-name>HelloWeb</servlet-name>
      <url-pattern>*.jsp</url-pattern>
   </servlet-mapping>

</web-app>
```
在**web.xml中**的文件将被保存在你的Web应用程序的WebContent / WEB-INF目录下。在初始化**HelloWeb** DispatcherServlet时，框架将尝试从位于应用程序的WebContent / WEB-INF 目录中的名为**[servlet-name] -servlet.xml**的文件加载应用程序上下文。在这种情况下，我们的文件将是**HelloWebservlet.xml**。

接下来，<servlet-mapping>标签指示哪些URL将由哪个DispatcherServlet处理。这里所有以 **.jsp** 结尾的HTTP请求都将由 **HelloWeb** DispatcherServlet 处理。

如果您不想使用缺省文件名作为 _[servlet-name] -servlet.xml_ 和缺省位置为 _WebContent / WEB-INF_ ，则可以通过在web.xml文件中添加servlet侦听器 _ContextLoaderListener_ 来自定义此文件名和位置如下 -

```xml
<web-app...>

   <!-------- _DispatcherServlet_ definition goes here----->
   ....
   <context-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>/WEB-INF/HelloWeb-servlet.xml</param-value>
   </context-param>

   <listener>
      <listener-class>
         org.springframework.web.context.ContextLoaderListener
      </listener-class>
   </listener>

</web-app>
```

现在，让我们检查放置在Web应用程序的_WebContent / WEB-INF_目录中的**HelloWeb-servlet.xml**文件的必需配置-
```xml
<beans xmlns = "http://www.springframework.org/schema/beans"
   xmlns:context = "http://www.springframework.org/schema/context"
   xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation = "http://www.springframework.org/schema/beans     
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
   http://www.springframework.org/schema/context 
   http://www.springframework.org/schema/context/spring-context-3.0.xsd">

   <context:component-scan base-package = "com.tutorialspoint" />

   <bean class = "org.springframework.web.servlet.view.InternalResourceViewResolver">
      <property name = "prefix" value = "/WEB-INF/jsp/" />
      <property name = "suffix" value = ".jsp" />
   </bean>

</beans>
```
以下是关于**HelloWeb-servlet.xml**文件的要点-

*    _[servlet name] -servlet.xml_ 的文件将被用来创建定义的豆类，覆盖在全域范围内的名字相同的Bean的定义。

*   所述 _\<context:component-scan\>_ 标签将使用以激活弹簧MVC注释扫描功能，其允许使用像@Controller和@RequestMapping等注解

*   该 _InternalResourceViewResolver这个_将定义解析视图名称规则。按照上面定义的规则，名为**hello**的逻辑视图被委托给位于_/WEB-INF/jsp/hello.jsp_的视图实现。

以下部分将向您展示如何创建您的实际组件，即Controller，Model和View。

## 定义一个控制器

DispatcherServlet将请求委托给控制器以执行特定于其的功能。
该 **\@Controller** 注解表明特定类供应控制器的作用。的**\@RequestMapping** 注解用于将URL映射到要么整个类或特定处理程序方法。

<pre class="prettyprint notranslate prettyprinted">@Controller
@RequestMapping("/hello")
public class HelloController { 
   @RequestMapping(method = RequestMethod.GET)
   public String printHello(ModelMap model) {
      model.addAttribute("message", "Hello Spring MVC Framework!");
      return "hello";
   }
}</pre>

该**@Controller**注解类定义为Spring MVC的控制器。在这里，@ **RequestMapping**的第一次使用表明这个控制器上的所有处理方法都是相对于**/ hello**路径的。下一个注释**@RequestMapping（method = RequestMethod.GET）**用于声明printHello（）方法作为处理HTTP GET请求的控制器的默认服务方法。您可以定义另一种方法来处理同一个URL上的任何POST请求。

您可以使用另一种形式编写上述控制器，您可以在_@RequestMapping中_添加其他属性，如下所示 -

<pre class="prettyprint notranslate prettyprinted">@Controller
public class HelloController {
   @RequestMapping(value = "/hello", method = RequestMethod.GET)
   public String printHello(ModelMap model) {
      model.addAttribute("message", "Hello Spring MVC Framework!");
      return "hello";
   }
}</pre>

所述**值**属性表示该处理程序方法被映射的URL和**方法**属性定义了服务的方法来处理HTTP GET请求。关于以上定义的控制器，需要注意以下几点：

*   您将在服务方法内定义所需​​的业务逻辑。您可以根据需要调用此方法中的另一个方法。

*   根据定义的业务逻辑，您将在此方法内创建一个模型。您可以使用setter不同的模型属性，这些属性将被视图访问以呈现最终结果。本示例创建一个模型，其属性为“消息”。

*   定义的服务方法可以返回一个字符串，其中包含要用于呈现模型的**视图**的名称。本例返回“hello”作为逻辑视图名称。

## 创建JSP视图

Spring MVC为不同的表示技术支持多种类型的视图。这些包括--JSP，HTML，PDF，Excel工作表，XML，Velocity模板，XSLT，JSON，Atom和RSS提要，JasperReports等。但最常用的是我们使用JSTL编写的JSP模板。

让我们在/WEB-INF/hello/hello.jsp中编写一个简单的**hello**视图 -

<pre class="prettyprint notranslate prettyprinted"><html>
   <head>
      <title>Hello Spring MVC</title>
   </head>

   <body>
      <h2>${message}</h2>
   </body>
</html></pre>

这里**$ {message}**是我们在Controller中设置的属性。您可以在视图中显示多个属性。

## Spring Web MVC框架示例

基于上述概念，让我们检查几个重要的例子，它们将帮助您构建Spring Web应用程序 -

| Sr.No. | 示例和说明 |
| 1 | [Spring MVC Hello World示例](https://www.tutorialspoint.com/spring/spring_mvc_hello_world_example.htm)这个例子将解释如何编写一个简单的Spring Web Hello World应用程序。 |
| 2 | [Spring MVC表单处理示例](https://www.tutorialspoint.com/spring/spring_mvc_form_handling_example.htm)本例将说明如何使用HTML表单编写Spring Web应用程序将数据提交给控制器并显示处理后的结果。 |
| 3 | [Spring页面重定向示例](https://www.tutorialspoint.com/spring/spring_page_redirection_example.htm)学习如何在Spring MVC框架中使用页面重定向功能。 |
| 4 | [Spring静态页面示例](https://www.tutorialspoint.com/spring/spring_static_pages_example.htm)学习如何在Spring MVC框架中访问静态页面以及动态页面。 |
| 五 | [Spring异常处理示例](https://www.tutorialspoint.com/spring/spring_exception_handling_example.htm)学习如何处理Spring MVC框架中的异常。 |