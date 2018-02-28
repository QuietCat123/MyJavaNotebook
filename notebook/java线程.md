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

### Condition对象
Condition
Condition.await/signal/signalAll原理和synchronized + wait / notify一致

Condition对象必须从ReentrantLock对象获取
```
package com.feiyangedu.sample;

import java.util.LinkedList;
import java.util.Queue;
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

class TaskQueue {

	final Queue<String> queue = new LinkedList<>();

	final Lock lock = new ReentrantLock();
	final Condition notEmpty = lock.newCondition(); //创建Condition对象lock.newCondition()

	public String getTask() throws InterruptedException {
		lock.lock();
		try {
			while (this.queue.isEmpty()) {
				notEmpty.await(); //使Condition对象await实现释放锁，进入等待状态。
			}
			return queue.remove();
		} finally {
			lock.unlock();
		}
	}

	public void addTask(String name) {
		lock.lock();
		try {
			this.queue.add(name);
			notEmpty.signalAll(); //而sganalAll则相当于this.notifyAll()
		} finally {
			lock.unlock();
		}
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
		worker.interrupt();
		worker.join();
		System.out.println("END");
	}
}

```
ReentrantLock＋Condition可以替代synchronized + wait / notify
## Concurrent集合
ReentrantLock+Condition实现Blocking Queue
java.util.concurrent提供了线程安全的集合……
```java
package com.feiyangedu.sample;

import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.BlockingQueue;

class WorkerThread extends Thread {
	BlockingQueue<String> taskQueue;

	public WorkerThread(BlockingQueue<String> taskQueue) {
		this.taskQueue = taskQueue;
	}

	public void run() {
		while (!isInterrupted()) {
			String name;
			try {
				name = taskQueue.take();
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
		BlockingQueue<String> taskQueue = new ArrayBlockingQueue<>(100);
		WorkerThread worker = new WorkerThread(taskQueue);
		worker.start();
		// add task:
		taskQueue.put("Bob");
		Thread.sleep(1000);
		taskQueue.put("Alice");
		Thread.sleep(1000);
		taskQueue.put("Tim");
		Thread.sleep(1000);
		worker.interrupt();
		worker.join();
		System.out.println("END");
	}
}
```
CopyOnWriteArrayList
ConcurrentHashMap
CopyOnWriteArraySet
ArrayBlockingQueue
LinkedBlockingQueue
LinkedBlockingDeque

## Atomic
无锁实现有锁
使用java.util.atomic提供的原子操作可以简化多线程编程：

AtomicInteger／AtomicLong／AtomicIntegerArray等

原子操作实现了无锁的线程安全

适用于计数器，累加器等

## ExecutorService
复用线程
* 线程池维护若干个线程，处于等待状态
* 如果有新任务就分配一个空闲线程
* 所有线程都忙碌就将先任务放入队列等待

JDK提供了ExecutorService实现了线程池功能
//submit,提交
```
ExecutorService executor = ExecutoeServicenewFixedThreadPool(4);//固定大小的线程池
executor.submit(task1);//提交一个任务到线程池
executor.submit(task2);
...
```
常用ExecutorService：
* FixedThreadPool：线程数固定
* CachedThreadPool：线程数根据任务动态调整
* SingleThreadExecutor：仅单线程执行

必须调用shutdown()关闭ExecutorService
ScheduledThreadPool可以定期调度多个任务（可取代Timer）

## Future
Callable<T>接口
Future<V>接口
get()
get(long timeout,TimeUnit unit)
cancel(boolean mayInterruptIfRunning)
isDone()

``` java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.URL;
import java.net.URLConnection;
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

class DownloadTask implements Callable<String> {
	String url;

	public DownloadTask(String url) {
		this.url = url;
	}

	public String call() throws Exception {
		System.out.println("Start download " + url + "...");
		URLConnection conn = new URL(this.url).openConnection();
		conn.connect();
		try (BufferedReader reader = new BufferedReader(new InputStreamReader(conn.getInputStream(), "UTF-8"))) {
			String s = null;
			StringBuilder sb = new StringBuilder();
			while ((s = reader.readLine()) != null) {
				sb.append(s).append("\n");
			}
			return sb.toString();
		}
	}
}

public class Main {

	public static void main(String[] args) throws Exception {
		ExecutorService executor = Executors.newFixedThreadPool(3);
		DownloadTask task = new DownloadTask("http://www.sina.com.cn/");
		Future<String> future = executor.submit(task);
		String html = future.get();
		System.out.println(html);
		executor.shutdown();
	}
}

```
![enter description here][7]
提交Callable任务，可以获得一个Future对象
可以用Future在将来某个时刻获取结果

## CompletableFuture
CompletableFuture的优点：

异步任务结束时，会自动回调某个对象的方法
异步任务出错时，会自动回调某个对象的方法
主线程设置好回调后，不再关心异步任务的执行
![enter description here][8]

``` java
import java.util.concurrent.CompletableFuture;
import java.util.function.Consumer;
import java.util.function.Function;
import java.util.function.Supplier;

class StockSupplier implements Supplier<Float> {

	@Override
	public Float get() {
		String url = "http://hq.sinajs.cn/list=sh000001";
		System.out.println("GET: " + url);
		try {
			String result = DownloadUtil.download(url);
			String[] ss = result.split(",");
			return Float.parseFloat(ss[3]);
		} catch (Exception e) {
			throw new RuntimeException(e);
		}
	}
}

public class CompletableFutureSample {

	public static void main(String[] args) throws Exception {
		CompletableFuture<Float> getStockFuture = CompletableFuture.supplyAsync(new StockSupplier());
		getStockFuture.thenAccept(new Consumer<Float>() {
			@Override
			public void accept(Float price) {
				System.out.println("Current price: " + price);
			}
		});
		getStockFuture.exceptionally(new Function<Throwable, Float>() {
			@Override
			public Float apply(Throwable t) {
				System.out.println("Error: " + t.getMessage());
				return Float.NaN;
			}
		});
		getStockFuture.join();
	}

}
```
![enter description here][9]

```java
package com.feiyangedu.sample;

import java.net.URLEncoder;
import java.util.concurrent.CompletableFuture;
import java.util.function.Consumer;
import java.util.function.Function;
import java.util.function.Supplier;

class Price {
	final String code;
	final float price;

	Price(String code, float price) {
		this.code = code;
		this.price = price;
	}
}

class StockLookupSupplier implements Supplier<String> {
	String name;

	public StockLookupSupplier(String name) {
		this.name = name;
	}

	public String get() {
		System.out.println("lookup: " + name);
		try {
			String url = "http://suggest3.sinajs.cn/suggest/type=11,12&key=" + URLEncoder.encode(name, "UTF-8");
			String result = DownloadUtil.download(url);
			String[] ss = result.split(",");
			return ss[3];
		} catch (Exception e) {
			throw new RuntimeException(e);
		}
	}
}

public class CompletableFutureSequenceSample {

	public static void main(String[] args) throws Exception {
		String name = "上证指数";
		CompletableFuture<String> getStockCodeFuture = CompletableFuture.supplyAsync(new StockLookupSupplier(name));
		CompletableFuture<Price> getStockPriceFuture = getStockCodeFuture.thenApplyAsync(new Function<String, Price>() {
			public Price apply(String code) {
				System.out.println("got code: " + code);
				try {
					String url = "http://hq.sinajs.cn/list=" + code;
					String result = DownloadUtil.download(url);
					String[] ss = result.split(",");
					return new Price(code, Float.parseFloat(ss[3]));
				} catch (Exception e) {
					throw new RuntimeException(e);
				}
			}
		});
		getStockPriceFuture.thenAccept(new Consumer<Price>() {
			public void accept(Price p) {
				System.out.println(p.code + ": " + p.price);
			}
		});
		getStockPriceFuture.join();
	}
}

```
![enter description here][10]

```java
package com.feiyangedu.sample;

import java.util.concurrent.CompletableFuture;
import java.util.function.Consumer;
import java.util.function.Supplier;

class StockPrice {
	final float price;
	final String from;

	StockPrice(float price, String from) {
		this.price = price;
		this.from = from;
	}

	public String toString() {
		return "Price: " + price + " from " + from;
	}
}

class StockFromSina implements Supplier<StockPrice> {

	@Override
	public StockPrice get() {
		String url = "http://hq.sinajs.cn/list=sh000001";
		System.out.println("GET: " + url);
		try {
			String result = DownloadUtil.download(url);
			String[] ss = result.split(",");
			return new StockPrice(Float.parseFloat(ss[3]), "sina");
		} catch (Exception e) {
			throw new RuntimeException(e);
		}
	}
}

class StockFromNetease implements Supplier<StockPrice> {

	@Override
	public StockPrice get() {
		String url = "http://api.money.126.net/data/feed/0000001,money.api";
		System.out.println("GET: " + url);
		try {
			String result = DownloadUtil.download(url);
			int priceIndex = result.indexOf("\"price\"");
			int start = result.indexOf(":", priceIndex);
			int end = result.indexOf(",", priceIndex);
			return new StockPrice(Float.parseFloat(result.substring(start + 1, end)), "netease");
		} catch (Exception e) {
			throw new RuntimeException(e);
		}
	}
}

public class CompletableFutureAnyOfSample {

	public static void main(String[] args) throws Exception {
		CompletableFuture<StockPrice> getStockFromSina = CompletableFuture.supplyAsync(new StockFromSina());
		CompletableFuture<StockPrice> getStockFromNetease = CompletableFuture.supplyAsync(new StockFromNetease());
		CompletableFuture<Object> getStock = CompletableFuture.anyOf(getStockFromSina, getStockFromNetease);
		\\CompletableFuture<Void> getStock = CompletableFuture.allOf(getStockFromSina, getStockFromNetease);

		getStock.thenAccept(new Consumer<Object>() {
			public void accept(Object result) {
				System.out.println("Reuslt: " + result);
			}
		});
		getStock.join();//在前两个对象任意一个完成时就调用geStock
	}

}

```
CompletableFuture对象可以指定异步处理流程：

* thenAccept()处理正常结果
* exceptional()处理异常结果
* thenApplyAsync() 用于串行化另一个CompletableFuture
* anyOf / allOf 用于并行化两个CompletableFuture

## Fork/Join
Fork/Join是一种基于“分治”的算法：分解任务＋合并结果

ForkJoinPool线程池可以把一个大任务分拆成小任务并行执行
比如将一个大数组拆成几个小数组……

任务类必须继承自RecursiveTask／RecursiveAction
![enter description here][11]
![enter description here][12]
使用Fork/Join模式可以进行并行计算提高效率




















  [1]: ./images/JAVA%E5%A4%9A%E7%BA%BF%E7%A8%8B1.png "JAVA多线程1"
  [2]: ./images/JAVA%E5%A4%9A%E7%BA%BF%E7%A8%8B24.png "JAVA多线程24"
  [3]: ./images/%E7%BA%BF%E7%A8%8B%E7%9A%84%E7%8A%B6%E6%80%81.png "线程的状态"
  [4]: ./images/%E7%BA%BF%E7%A8%8B%E5%90%8C%E6%AD%A51.png "线程同步1"
  [5]: ./images/JAVA%E6%AD%BB%E9%94%81.png "JAVA死锁"
  [6]: ./images/JAVAwait%E5%92%8CNotify.png "JAVAwait和Notify"
  [7]: ./images/JAVA%20FUTURE.png "JAVA FUTURE"
  [8]: ./images/JAVA%20CompletableFuture.png "JAVA CompletableFuture"
  [9]: ./images/JAVACompletableFuture.png "JAVACompletableFuture"
  [10]: ./images/JAVACompletableFuture2.png "JAVACompletableFuture2"
  [11]: ./images/JAVAForkJoin.png "JAVAForkJoin"
  [12]: ./images/JAVAForkJoin2.png "JAVAForkJoin2"