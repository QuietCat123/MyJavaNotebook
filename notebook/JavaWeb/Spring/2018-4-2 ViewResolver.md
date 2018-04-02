---
title: 2018-4-2 ViewResolver
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---
在使用JSP进行开发时，您可以声明一==InternalResourceViewResolver==或一个 ==ResourceBundleViewResolverbean==

ResourceBundleViewResolver依赖于一个属性文件来定义映射到一个类和一个URL的视图名称。通过ResourceBundleViewResolver使用一个解析器，您可以混合不同类型的视图。这里是一个例子：
```xml
<!-- the ResourceBundleViewResolver -->
<bean id="viewResolver" class="org.springframework.web.servlet.view.ResourceBundleViewResolver">
    <property name="basename" value="views"/>
</bean>

# And a sample properties file is uses (views.properties in WEB-INF/classes):
welcome.(class)=org.springframework.web.servlet.view.JstlView
welcome.url=/WEB-INF/jsp/welcome.jsp

productList.(class)=org.springframework.web.servlet.view.JstlView
productList.url=/WEB-INF/jsp/productlist.jsp
```

InternalResourceBundleViewResolver也可以用于JSP。作为最佳实践，我们强烈建议将JSP文件放在目录下的'WEB-INF' 目录下，以免客户端直接访问它们。
```xml
<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>
    <property name="prefix" value="/WEB-INF/jsp/"/>
    <property name="suffix" value=".jsp"/>
</bean>
```

