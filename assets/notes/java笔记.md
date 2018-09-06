# java笔记

* Java直接调用Thread类中的run()方法
  * 调用start()会开启一个新线程，并调用run()方法
  * 直接调用run()方法，就当成一个普通成员对象的方法来执行，不会新开一个线程

* java并发
  * volatile和synchronized
    * volatile是轻量级锁，sychronized是重量级锁（sychronize会引起线程的上下文切换）
    * volatile 内存可见性：当一个共享变量被volatile修饰时，它会保证修改的值立即被更新到主存，通俗来说就是，线程A对一个volatile变量的修改，对于其它线程来说是可见的
    * 用volatile必须满足两个条件：
      1. 对变量的写操作不依赖当前值，如多线程下执行a++，是无法通过volatile保证结果准确性的
      2. 该变量没有包含在具有其它变量的不变式中
    * volatile 可以禁止 JVM 的指令重排
    * Volatile和Synchronized四个不同点：
      1. 粒度不同，前者针对变量 ，后者锁对象和类
      2. syn阻塞，volatile线程不阻塞
      3. syn保证三大特性，volatile不保证原子性
      4. syn编译器优化，volatile不优化 volatile具备两种特性：
  * java并发包原子性操作

* 面向对象的编程语言有封装、继承、抽象、多态等4个主要的特征

* 线程sleep和wait区别
  * sleep是Thread类的方法,wait是Object类中定义的方法
  * Thread.sleep不会导致锁行为的改变
  * wait会释放对象锁，必须在synchronized block中执行否则会在programme runtime时扔出"java.lang.IllegalMonitorStateException"异常
  * Thread.sleep和Object.wait都会暂停当前的线程，对于CPU资源来说，不管是哪种方式暂停的线程，都表示它暂时不再需要CPU的执行时间。OS会将执行时间分配给其它线程。区别是，调用wait后，需要别的线程执行notify/notifyAll才能够重新获得CPU执行时间
  * sleep 让线程从 [running] -> [阻塞态] 时间结束/interrupt -> [runnable]
  * wait 让线程从 [running] -> [等待队列]notify  -> [锁池] -> [runnable]
  * sleep是一个线程的运行状态控制,wait一个是线程之间的通讯的问题
  * yield让当前运行进程回到可运行状态，也不会释放对象锁

* java的Thread类，start()和run()方法有什么区别
  * start启用线程
  * run调用类方法

* 线程sleep和wait区别
  * sleep是Thread类的方法,wait是Object类中定义的方法
  * Thread.sleep不会导致锁行为的改变
  * wait会释放对象锁，必须在synchronized block中执行否则会在programme runtime时扔出"java.lang.IllegalMonitorStateException"异常
  * Thread.sleep和Object.wait都会暂停当前的线程，对于CPU资源来说，不管是哪种方式暂停的线程，都表示它暂时不再需要CPU的执行时间。OS会将执行时间分配给其它线程。区别是，调用wait后，需要别的线程执行notify/notifyAll才能够重新获得CPU执行时间
  * sleep 让线程从 [running] -> [阻塞态] 时间结束/interrupt -> [runnable]
  * wait 让线程从 [running] -> [等待队列]notify  -> [锁池] -> [runnable]
  * sleep是一个线程的运行状态控制,wait一个是线程之间的通讯的问题
  * yield让当前运行进程回到可运行状态，也不会释放对象锁

## 容器

* hashmap和treemap的数据结构以及时间复杂度
  * hashmap 采用 entry 数组，每个数组元素是一个桶，每个桶存放一个链表，链表长度超过8就转红黑树
  * treemap 采用红黑树

* hashmap如何解决哈希冲突
  * 拉链法