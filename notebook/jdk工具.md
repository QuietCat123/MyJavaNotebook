---
title: jdk工具
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---
1. 编译 javac A.java
2. 打包 jar cvfm A.jar A.man A.class
c为创建，v显示详情verbose，f文件名，m表示清单文件
3. 运行 java -jar A.jar

其中A.man是清单文件（manifest)内容如下：
```
Manifest-Version:1.0
Class-Path:.
Main-Class:A
```
清单文件可以任意命名，常见为MANIFEST.MF

javadoc -d 目录名 xxx.java

/** */其中用的标记：
@author 类说明，开发作者
@version 类说明，模块版本
@see 类、属性、方法说明，相关主题
@param 方法说明，对方法参数说明
@return 方法说明，返回值
@exception 方法说明，对可能抛出的异常

javap 类名 查看类的信息

javap -c 类名 ， 反汇编（？
