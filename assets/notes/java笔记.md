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