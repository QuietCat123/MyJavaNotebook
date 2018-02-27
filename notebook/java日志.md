---
title:java日志
tags: java,日志
grammar_cjkRuby: true
---

日志是为了替代System.out.println()，可以定义格式，重定向到文件等
日志可以存档，便于追踪问题
日志记录可以按级别分类，便于打开或关闭某些级别
可以根据配置文件调整日志，无需修改代码

JDK提供了Logging：java.util.logging
（但不常用）
JDK Logging定义了7个日志级别：

SEVERE
WARNING
INFO （默认级别）
CONFIG
FINE
FINER
FINEST
JDK Logging的局限：

JVM启动时读取配置文件并完成初始化
JVM启动后无法修改配置
需要在JVM启动时传递参数 -Djava.util.logging.config.file=config-file-name

``` java
package com.feiyangedu.sample;

import java.util.logging.Level;
import java.util.logging.Logger;

public class Main {

	public static void main(String[] args) {
		Logger logger = Logger.getGlobal();
		//logger.setLevel(Level.WARNING);
		//设置level后就能过滤低级别的消息
		logger.info("Create new person...");
		Person p = new Person("Xiao Ming");
		System.out.println(p.hello());
		try {
			new Person(null);
		} catch (Exception e) {
			logger.log(Level.WARNING, "Create new person failed", e);
		}
		logger.info("Program end.");
	}

}
```
**使用前后对比**
```
2月 27, 2018 2:19:21 下午 com.feiyangedu.sample.Main main
信息: Create new person...
Hello, Xiao Ming!
2月 27, 2018 2:19:21 下午 com.feiyangedu.sample.Main main
警告: Create new person failed
java.lang.IllegalArgumentException: name is null.
	at com.feiyangedu.sample.Person.<init>(Person.java:9)
	at com.feiyangedu.sample.Main.main(Main.java:15)

2月 27, 2018 2:19:21 下午 com.feiyangedu.sample.Main main
信息: Program end.
————————————————————
Hello, Xiao Ming!
2月 27, 2018 2:17:46 下午 com.feiyangedu.sample.Main main
警告: Create new person failed
java.lang.IllegalArgumentException: name is null.
	at com.feiyangedu.sample.Person.<init>(Person.java:9)
	at com.feiyangedu.sample.Main.main(Main.java:15)

```

## 使用Commons Logging

### Commons Logging

Commons Logging是Apache创建的日志系统：

*   Commons Logging是使用最广泛的日志模块
*   Commons Logging的API非常简单
*   Commons Logging可以自动使用其他日志模块

用法
```java
public class Main{
	public static void main(String[] args){
		Log log = LogFactory.getLog(Main.class);
		//
		log.info("start");
		log.warn("end");
	}
}
```
Commons Logging定义了6个日志级别：

*   FATAL
*   ERROR
*   WARNING
*   INFO （默认级别）
*   DEBUG
*   TRACE

``` java
package com.feiyangedu.sample;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;

public class Main {
	static Log log = LogFactory.getLog(Main.class);
	public static void main(String[] args) {
		Person p = new Person("Xiao Ming");
		log.info(p.hello());
		try {
			new Person(null);
		} catch (Exception e) {
			log.error("Exception",e);
		}
	}

}

```
