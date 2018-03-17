---
title:  Intellij idea Language level和Java Compiler版本自动变化问题
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

## 原因分析

经排查，原来是这个问题的根源在于maven的pom.xml文件中未配置jdk版本导致。当未配置jdk版本时，一旦pom文件发生变化，Java Compiler和Language level会自动变回到原来的默认1.5版本。

## 解决方案

在pom文件中添加maven-compiler-plugin插件，并指定jdk使用的jdk版本即可解决上面问题。maven-compiler-plugin的配置同时对Java compiler和Language level同时生效。 
配置内容如下：
```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.5.1</version>
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
            </configuration>
        </plugin>
    </plugins>
</build>
```