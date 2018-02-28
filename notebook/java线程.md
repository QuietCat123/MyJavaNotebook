---
title: java线程
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---
## 多线程简介
实现多任务的方法：
* 多进程模式（每个进程只有一个线程）
* 多线程模式（一个进程多个线程）
* 多进程+多线程模式（复杂度最高）
创建进程比线程开销大，
进程间通信比线程间通信慢。

Java从语言级别支持多线程
一个JAVA程序就是一个JVM进程
JVM用一个主线程来执行main()方法
在main()方法中又可以启动多个线程

特点：
网络、数据库、WEB都依赖。

如：Object中wait(), notify()
• java.lang中的类 Thread

### 创建新线程
线程体 ----run()方法来实现的
线程启动后，系统自动调用run()方法
通常run()方法执行一个时间较长的操作
1.通过MyThread类继承Thread类创建线程
从Thread继承、复写run()方法、创建MyThread实例、调用start()启动线程
```java
public class TestThread1 {	
	public static void main(String args[]){
		Thread t = new MyThread(100);
		t.start();
	}
}

class MyThread extends Thread {
	private int n;;
	public MyThread( int n ){
		super();
		this.n=n;
	}
	public void run() {
		for(int i=0;i<n;i++) {
			System.out.print (" " + i);
		}
	}
}
```
```java
package com.feiyangedu.sample;

class HelloThread extends Thread {

	String name;

	public HelloThread(String name) {
		this.name = name;
	}

	@Override
	public void run() {
		for (int i = 0; i < 3; i++) {
			System.out.println("Hello, " + name + "!");
		}
	}
}

public class Main {

	public static void main(String[] args) throws Exception {
		Thread t1 = new HelloThread("Bob");
		Thread t2 = new HelloThread("Alice");
		t1.start();
		t2.start();
		for (int i = 0; i < 3; i++) {
			System.out.println("Main!");
			Thread.sleep(50);
		}
	}
}

```
2.通过Thread()构造方法传递Runnable对象来创建线程
（一个类已经有了继承，就可以用第二个方法）
可以实现Ruannable接口、
复写run()方法、
在main()方法中创建Runnable实例，
创建Thread实例传入Runnable、
调用start()启动线程。
```java
public class MyThread implements Runnable{
	public void run {
		System.out.println();
	}
}
public class Main{
	public static void main(String[] args){
		Runnable r = new MyThread();
		Thread t = new Thread(r);
		t.start();
	}
}
```
![enter description here][1]
![enter description here][2]
直接调用run()方法时无效的。必须调用start()方法才能启动新线程

用Thread.sleep(100) 当前线程暂停100ms.

### 线程的优先级
Thread.setPriority(int n)//1~10，默认为5
不能通过设置优先级确保功能执行顺序

## 线程的状态
![enter description here][3]
### 线程对象Thread状态包括
New / Runnable / Blocked / Waiting / Timed Waiting / Terminated
**终止原因**
- run()执行到return
- 未捕获的异常
- 对线程调用stop()
### join()
通过对另一个线程对象调用join()方法可以等待其执行结束，再继续往下运行

## 中断线程
中断线程就是其他线程发出信号，使之中断run()
需要检测isInterruptede()标志
其他线程通过调用interrupt方法中断该线程
若处于等待状态，捕获InterruptedException
或，设置一个标志位
public **volatile** boolean running = ture;
线程间共享变量需要用到vloatile关键字标记

volatile是为了告诉虚拟机，
每次访问变量，总是获取主内存最新值
每次修改变量后，立刻回写到主内存

``` java
package com.feiyangedu.sample;

class HelloThread extends Thread {
	public volatile boolean running = true;
	@Override
	public void run() {
		while (!isInterrupted()) {//while(runing)
			System.out.println("Hello!");
			try {
				Thread.sleep(100);
			} catch (InterruptedException e) {
				System.out.println("Interrupted!");
				break;
			}
		}
		System.out.println("Thread end");
	}
}

public class Main {

	public static void main(String[] args) throws Exception {
		HelloThread t = new HelloThread();
		t.start();
		Thread.sleep(1000);
		t.interrupt();//t.running = false;
		System.out.println("Main end");
	}
}
```

## 守护线程
如果某个线程不结束，JVM进程就无法结束
那么谁来结束该线程？

守护线程（Daemon):
* 是为其他线程服务的线程
* 所有 非守护线程 都执行完毕后，虚拟机退出
特点：
不能持有任何资源（打开文件）等。

setDaemon(true);
就可以将进程设置为守护线程










  [1]: ./images/JAVA%E5%A4%9A%E7%BA%BF%E7%A8%8B1.png "JAVA多线程1"
  [2]: ./images/JAVA%E5%A4%9A%E7%BA%BF%E7%A8%8B24.png "JAVA多线程24"
  [3]: ./images/%E7%BA%BF%E7%A8%8B%E7%9A%84%E7%8A%B6%E6%80%81.png "线程的状态"