---
title: java线程
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---
# 线程的概念
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

# 线程同步
多个线程同时运行，其调度由操作系统决定，程序本身无法决定
若多个线程同时读写共享变量：
* 对共享变量进行写入时，必须是原子操作
* 原子操作是指不能被中断的一个或一系列操作
![enter description here][4]
因此，要保证thread1执行ILoad时，其他线程不执行。
就要在ILOAD之前加锁，ISTORE之后解锁
Java使用synchronized对一个对象加锁：
```java
synchronized(lock){
	n = n+1;
}
```
* 找出修改共享变量的线程代码块
* 选择一个实例作为锁
* 使用synchronized(lockObject)
```java
package com.feiyangedu.sample;

class AddThread extends Thread {
	public void run() {
		for (int i = 0; i < Main.LOOP; i++) {
			synchronized(Main.LOCK) {
				Main.count += 1;
			}
		}
	}
}

class DecThread extends Thread {
	public void run() {
		for (int i = 0; i < Main.LOOP; i++) {
			synchronized(Main.LOCK) {
				Main.count -= 1;
			}
		}
	}
}

public class Main {

	final static int LOOP = 10000;

	public static int count = 0;
	public static final Object LOCK = new Object();//it's LOCK!!!!
	public static void main(String[] args) throws Exception {
		Thread t1 = new AddThread();
		Thread t2 = new DecThread();
		t1.start();
		t2.start();
		t1.join();
		t2.join();
		System.out.println(count);
	}
}

```
对同一个变量修改的锁应该是一样的。
在使用synchronized时不应担心异常

基本类型（除Long，double）赋值是原子操作；
引用类型赋值是原子操作；
原子操作不需要同步

局部变量不需要同步

## synchronized方法
```
public synchronized void add(int n){
	count += n;
	total += n;
}
等价于
public void add(int n){
	synchronized(this){
		count += n;
		total += n;
	}
}
```
### Thread-safe
一个类在设计时运行多线程正确访问，就是线程安全的。
java.lang.StringBuffer
线程安全的类：
* 不变类：String Interge LocalDate
* 没有成员变量的类： Math
* 正确使用synchronized的类：StringBuffer

### 死锁
* 死锁产生的条件：
多线程各自持有不同的锁，
并互相试图获取对方已持有的锁，
双方无限等待下去：导致死锁
![enter description here][5]
* 如何避免死锁：
多线程获取锁的顺序要一致

Java的线程锁是可重入的锁
```java
package com.feiyangedu.sample;

class SharedObject {

	final Object lockA = new Object();
	final Object lockB = new Object();

	int accountA = 1000;
	int accountB = 2000;

	public void a2b(int balance) {
		synchronized (lockA) {
			accountA -= balance;
			synchronized (lockB) {
				accountB += balance;
			}
		}
	}

	public void b2a(int balance) {
		synchronized (lockB) {
			accountB -= balance;
			synchronized (lockA) {
				accountA += balance;
			}
		}
	}
}

class AThread extends Thread {
	public void run() {
		for (int i = 0; i < Main.LOOP; i++) {
			Main.shared.a2b(1);
			if (i % 100 == 0) {
				System.out.println(".");
			}
		}
	}
}

class BThread extends Thread {
	public void run() {
		for (int i = 0; i < Main.LOOP; i++) {
			Main.shared.b2a(1);
			if (i % 100 == 0) {
				System.out.println(".");
			}
		}
	}
}

public class Main {

	final static int LOOP = 1000;

	public static SharedObject shared = new SharedObject();

	public static void main(String[] args) throws Exception {
		Thread t1 = new AThread();
		Thread t2 = new BThread();
		t1.start();
		t2.start();
		t1.join();
		t2.join();
		System.out.println("END");
	}
}

```
### wait和notify
多线程协调运行：当条件不满足时，线程进入等待状态；
wait()
只能在synchronized中调用，wait()只能作用于锁对象
wait()在执行时释放所得的锁，恢复重新获得锁
然后其他线程才能获得锁，从而执行
notify()可以唤醒线程，但不释放锁
notifyAll()都可以唤醒
![enter description here][6]
```java
package com.feiyangedu.sample;

import java.util.LinkedList;
import java.util.Queue;

class TaskQueue {

	final Queue<String> queue = new LinkedList<>();

	public synchronized String getTask() throws InterruptedException {
		while (this.queue.isEmpty()) {
			this.wait();
		}
		return queue.remove();
	}

	public synchronized void addTask(String name) {
		this.queue.add(name);
		this.notifyAll();
	}
}

class WorkerThread extends Thread {
	TaskQueue taskQueue;

	public WorkerThread(TaskQueue taskQueue) {
		this.taskQueue = taskQueue;
	}

	public void run() {
		while (!isInterrupted()) {
			String name;
			try {
				name = taskQueue.getTask();
			} catch (InterruptedException e) {
				break;
			}
			String result = "Hello, " + name + "!";
			System.out.println(result);
		}
	}
}

public class Main {

	public static void main(String[] args) throws Exception {
		TaskQueue taskQueue = new TaskQueue();
		WorkerThread worker = new WorkerThread(taskQueue);
		worker.start();
		// add task:
		taskQueue.addTask("Bob");
		Thread.sleep(1000);
		taskQueue.addTask("Alice");
		Thread.sleep(1000);
		taskQueue.addTask("Tim");
		Thread.sleep(1000);
		worker.interrupt();//中断
		worker.join();
		System.out.println("END");
	}
}

```
# 高级concurrent包
线程同步：
多线程读写竞争资源，Java提供了synchronized/wait/hotify
编写多线程同步很困难。
//concurrent 同时，current 当前，现在

java.util.concurrent包
* 更高级的同步功能
* 简化多线程程序的编写

java.util.locks.ReentrantLock用于替代synchronized加锁
```java
synchronized(lockObj){
	n = n+1;
}

lock.lock();
try{
	n = n+ 1;
} finally {
	lock.unlock();
}
```
```java
class Counter {
	final Lock lock = new ReentrantLock();
	public void inc(){
		lock.lock();
		try{
			n = n+1;
		} finally {
			lock.unlock();
		}
	}	
}		
```
ReentratLock:
* 一个线程可多次获取同一个锁
* lock()方法可获得锁
* tryLock()方法可尝试回去锁并可指定超时

``` java
package com.feiyangedu.sample;

import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

class Counter {

	private Lock lock = new ReentrantLock(); //创建一个ReentrantLock对象并向上转型为Lock对象

	private int value = 0;

	public void add(int m) {
		lock.lock(); //在try外获取锁
		try {
			this.value += m; //try中实现业务逻辑
		} finally {
			lock.unlock(); //finally中unlock释放锁
		}
	}

	public void dec(int m) {
		lock.lock();
		try {
			this.value -= m;
		} finally {
			lock.unlock();
		}
	}

	public int get() {
		lock.lock();
		try {
			return this.value;
		} finally {
			lock.unlock();
		}
	}
}

public class Main {

	final static int LOOP = 100;

	public static void main(String[] args) throws Exception {
		Counter counter = new Counter();
		Thread t1 = new Thread() {
			public void run() {
				for (int i = 0; i < LOOP; i++) {
					counter.add(1);
				}
			}
		};
		Thread t2 = new Thread() {
			public void run() {
				for (int i = 0; i < LOOP; i++) {
					counter.dec(1);
				}
			}
		};
		t1.start();
		t2.start();
		t1.join();
		t2.join();
		System.out.println(counter.get());
	}
}
```
if(lock.tryLock(1,TImeUnit.SECONDS))
可以指定超时

## ReadWriteLock
允许多个线程同时读，只要一个在写，其他就暂停
使用ReadWriteLock可以提高读取效率：

* ReadWriteLock只允许一个线程写入
* ReadWriteLock允许多个线程同时读取
* ReadWriteLock适合读多写少的场景
```java
package com.feiyangedu.sample;

import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReadWriteLock;
import java.util.concurrent.locks.ReentrantReadWriteLock;

class Counter {

	private ReadWriteLock lock = new ReentrantReadWriteLock();
	//获取一个ReetreantReadWriteLock对象
	private Lock rlock = lock.readLock();
	//创建一个读锁
	private Lock wlock = lock.writeLock();
	//创建一个写锁
	private int value = 0;

	public void add(int m) {
		wlock.lock();
		try {
			this.value += m;
		} finally {
			wlock.unlock();
		}
	}

	public void dec(int m) {
		wlock.lock();
		try {
			this.value -= m;
		} finally {
			wlock.unlock();
		}
	}

	public int get() {
		rlock.lock();
		try {
			return this.value;
		} finally {
			rlock.unlock();
		}
	}
}

public class Main {

	final static int LOOP = 100;

	public static void main(String[] args) throws Exception {
		Counter counter = new Counter();
		Thread t1 = new Thread() {
			public void run() {
				for (int i = 0; i < LOOP; i++) {
					counter.add(1);
				}
			}
		};
		Thread t2 = new Thread() {
			public void run() {
				for (int i = 0; i < LOOP; i++) {
					counter.dec(1);
				}
			}
		};
		t1.start();
		t2.start();
		t1.join();
		t2.join();
		System.out.println(counter.get());
	}
}
```
适合一个实例，有大量线程读取，仅有少量修改







  [1]: ./images/JAVA%E5%A4%9A%E7%BA%BF%E7%A8%8B1.png "JAVA多线程1"
  [2]: ./images/JAVA%E5%A4%9A%E7%BA%BF%E7%A8%8B24.png "JAVA多线程24"
  [3]: ./images/%E7%BA%BF%E7%A8%8B%E7%9A%84%E7%8A%B6%E6%80%81.png "线程的状态"
  [4]: ./images/%E7%BA%BF%E7%A8%8B%E5%90%8C%E6%AD%A51.png "线程同步1"
  [5]: ./images/JAVA%E6%AD%BB%E9%94%81.png "JAVA死锁"
  [6]: ./images/JAVAwait%E5%92%8CNotify.png "JAVAwait和Notify"