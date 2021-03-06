# c/cpp笔记

* cpp 三个特征
  * 多态、封装、继承

* cpp多态如何实现(虚函数的具体实现原理)
  * 概念：在面向对象过程中，多态是向不同对象发送相同消息做出不同行为（方法）。在 cpp 中就是相同的函数签名可以有不同的实现
  * 多态分为静态多态和动态多态，静态多态包括函数重载、泛型编程，动态多态包括虚函数
  * 静态多态：编译器在编译期间完成，编译器根据实参的类型（可能会进行隐式类型转换），推断出要调用哪个函数，如果有对应的函数就调用该函数，否则编译错误
  * 动态多态：虚函数的实现
    * 根据虚函数机制在运行时根据调用对象来判断具体调用哪一个函数，是一种动态绑定的机制，因此增加时间开销；因为需要虚表和虚指针，增加内存开销
    * 条件：
      1. 必须是虚函数（一定在派生类中对基类虚函数进行重写）
      2. 通过基类类型引用或指针指向子类对象（通过基类类型指针或引用调用虚函数时，要根据运行时指针实际指向的类型确认基类还是派生类的虚函数；调用非虚函数时，都调用基类的函数）
    * 编译器为每个包含虚函数的类生成一张虚函数表，即存放每个虚函数地址的函数指针数组，简称虚表(vtbl)，每个虚函数对应虚函数表中的一个索引号
    * 编译器还会为该类增加一个隐式的成员变量，通常在该类实例化对象的起始位置，用于存放虚函数表的首地址，该变量被称为虚函数表指针，简称虚指针(vptr)
    * 虚函数表是一个类一张，所有对象通过虚指针共享同一张虚表
    * 虚函数覆盖本质是用子类的虚函数地址覆盖拷贝的基类表中对应的基类虚函数地址
    * 多重继承时派生类会有多个虚函数表和多个虚指针
    * [参考链接](https://blog.csdn.net/u010126059/article/details/50768646)
  * [参考链接](https://blog.csdn.net/apt1203jn/article/details/78506855)

* 重载、重写（覆盖）、重定义的区别
  * 重载：在同一个作用域上，相同函数名不同参数列表（无关返回值，参数列表不同指参数的个数、类型或者顺序）
  * 重写：不在同一个作用域上（子类重写父类函数），函数签名相同，返回值也相同（协变例外），基类函数必须有 virture 修饰
  * 重定义：不在同一作用域上函数名相同不构成重写
  * 协变：基类虚函数返回基类指针或引用，派生类虚函数返回派生类指针或引用，依旧构成重写

* 什么是封装，面向对象和面向过程有什么区别
  * 封装是把数据和操作数据的方法绑定起来，对数据的访问只能通过已定义的接口。封装就是隐藏一切可隐藏的东西，只向外界提供最简单的编程接口
  * 面向过程就是分析出解决问题所需要的步骤，然后用函数把这些步骤一步一步实现，使用的时候一个一个依次调用就可以了。面向对象是把构成问题事务分解成各个对象，建立对象的目的不是为了完成一个步骤，而是为了描叙某个事物在整个解决问题的步骤中的行为
  * 面向过程是编年体；面向对象是纪传体

* cpp文件是怎么运行起来的
  * cpp语言要想运行起来，就要通过翻译环境把 .cpp 文件翻译成机器可以识别的机器码
  * cpp 翻译过程可以分为编译和链接两个阶段
  * 编译分为预编译、编译、汇编
    * 预编译是文件操作，将头文件包含，删除注释，宏定义替换，.c 文件经过预编译成为 .i 文件
    * 编译是将源代码翻译成汇编代码，包括词法分析、语法分析、语义分析、源代码优化、代码生成和目标代码优化。.i 编译成 .s 文件
    * 汇编是将汇编代码翻译成机器代码，.s 汇编成 .o 文件
  * 链接是符号表的合并和重定义
    * 生成 .exe 文件

* 动态链接和静态链接的区别
  * 动态链接解决静态链接的两个问题：
    * 多个副本问题：在多进程情况下，静态链接每个进程都要保存一个静态链接副本
    * 发布一致性和更新问题：对静态链接库进行了更新，要重新编译代码，动态链接只需要将就目标文件替换掉程序下次运行时新版本目标文件会被自动装载到内存并链接起来
  * 链接工作：
    * 合并所有".obj"文件，合并符号表，给符号分配地址
    * 符号的重定位：将符号分配的地址写回原来未正确分配地址的地方
  * [动态链接和静态链接的区别](https://www.cnblogs.com/improvement/p/4719325.html)
  * [编译链接全过程](https://blog.csdn.net/weixin_41966991/article/details/81152490)

* cpp文件运行时的堆和栈
  * 栈区是分配局部变量的空间，栈的空间是自动分配自动回收的，生存周期只在函数的运行过程；由系统分配，速度较快；栈地址向小地址增长
  * 堆由 new 分配，需要手动回收空间，否则造成内存泄漏，分配速度较慢

* 设计一个类，只能生成该类的一个实例（单例模式）
  * 饿汉模式：直接在静态区初始化 instance，然后通过 getInstance() 返回
    * 称为懒汉是因为，不论这个单例会不会被使用都会被初始化，空间换时间，在之后的使用过程中不用再生成单例
  * 懒汉模式：在 getInstance() 里构造单例并且返回
    * 称为懒汉是因为只有在需要用到单例的时候才返回单例，时间换空间，每次取单例的时候都需要判断生成返回
    * 构造函数和析构函数都是私有成员，不会被外界调用，类有一个指向自己的静态指针
    * 通过静态函数 getInstance() new 出一个对象赋值给静态指针，返回
    * getInstance() 方法需要处理多线程环境，需要对生成对象代码加锁
    ```c
    Singleton* getInstance()
    {
        if (instance == NULL)
        {
          lock();
          if (instance == NULL)
          {
              instance = new Singleton();
          }
          unlock();
        }
        return instance;
    }
    ```

* auto 和 static
  * auto 是由程序自动控制变量的生存周期，变量在进入作用域的时候被分配，离开作用域的时候被释放； static 在程序初始化时被分配，直到程序退出前才被释放
  * static 声明变量或者函数，实现不需要 static 修饰

* static 局部变量
  * 静态局部变量存储在静态存储区内
  * 静态局部变量生存周期为整个源程序周期，不随函数结束而结束
  * 静态局部变量不为其他函数可见
  * 静态局部变量只初始化一次，下次进入函数使用上一次的值
  * [static详解](https://blog.csdn.net/zkangaroo/article/details/61202533)

* cpp static 和 java static 的异同
  * 同：都可以用来修饰方法、变量
  * 异：java 中有静态代码块，且只在类载入时调用一次； java 中可以用 static 修饰内部类，表示内部类不能访问外部类

* 智能指针
  * 智能指针出现的原因：函数结束时指针指向内存没有释放导致内存泄露（可能是函数异常结束没有执行 delete， 也可能是程序员没有写 delete），为了让指针像普通变量在退出作用域时自动释放内存，将指针封装成类，在类的析构函数执行 delete 释放内存操作
  * 智能指针类型 auto_ptr, unique_ptr, shared_ptr, weak_ptr
  * 摒弃 auto_ptr 使用 unique_ptr 原因：多个 auto_ptr 可以指向同一块内存（编译不会报错），会出现重复释放同一块内存的错误导致运行时内存崩溃；而 unique_ptr 则在编译阶段检查这种错误
  * 防止多个指针指向同一块内存出现多次释放内存的问题所采用的方法：
    * 定义陚值运算符，使之执行深复制。这样两个指针将指向不同的对象，其中的一个对象是另一个对象的副本，缺点是浪费空间，所以智能指针都未采用此方案。
    * 建立所有权（ownership）概念。对于特定的对象，只能有一个智能指针可拥有，这样只有拥有对象的智能指针的构造函数会删除该对象。然后让赋值操作转让所有权。这就是用于auto_ptr和uniqiie_ptr 的策略，但unique_ptr的策略更严格。
    * 创建智能更高的指针，跟踪引用特定对象的智能指针数。这称为引用计数。例如，赋值时，计数将加1，而指针过期时，计数将减1,。当减为0时才调用delete。这是shared_ptr采用的策略。
  * [参考链接](http://www.cnblogs.com/lanxuezaipiao/p/4132096.html)

* struct 和 class 的区别
  * struct 默认访问类型是 public， class 是 private
  * 默认继承保护级别不同， struct 默认是 public 继承， class 默认是 private 继承
  * class 还可用于定义模板参数，像 typename，但是关键字 struct 不能同于定义模板参数
  * struct 更多是被用作数据结构， class 被用作对象
  * 保留 struct 的原因：保证兼容 c

* struct 在 c 和 cpp 中的异同
  * 异：cpp 将 struct 当作类来处理，因此定义结构体变量时直接 A a, 而 c 则要加上关键字 struct A a；

* 结构体 struct 和联合体 union 的区别
  * union 内所有成员共用一个内存首地址，内存按照 union 内占最大内存的成员内存来分配
  * 区别：变
    1. 内存分配， struct 对所有成员分配内存， union 只分配一块最大内存，所有成员共用这块内存
    2. 变量间关系， struct 成员之间不会互相影响， union 成员会对其他成员重写

* explicit 关键字作用
  * explicit 主要是用来修饰类的构造函数，被修饰的构造函数的类，不能发生相应的隐式类型转换，只能以显示的方式进行类型转换
  * explicit 修饰了构造函数， 生成类对象时只能 A a(12)，不能 A a = 12

* const 关键字作用
  * 声明常量对象，常量指针，常量函数
  * const 修饰函数参数表示参数只读不可改
  * const 不能和 static 一起修饰同一个函数，因为 static 表示静态，而 const 修饰函数会在函数中添加一个隐式的参数 const this*，但 static 修饰函数是没有 this 指针的，互相矛盾

* extern 关键字作用
  * extern "C" 表示用 c 的规矩编译，cpp 支持重载，因此在编译后函数在库中的名字和 c 编译后函数在库中的名字不一样
    * c中的函数签名，经过符号修饰后，会在前面加上_。例如：foo => _foo
    * c++中的函数签名，经过符号修饰后，会加上空间名，类名，函数名，函数参数等信息。（具体见《程序员自我修养》）例如：N::C::func(int) => _ZN1N1C4funcEi

* new 与 malloc的区别
  * malloc 与 free是 C++/C 语言的标准库函数(stdlib.h)，new/delete 是 C++ 的运算符。它们都可用于申请动态内存和释放内存。
  * 对于非内部数据类型的对象而言，光用 malloc/free 无法满足动态对象的要求。对象在创建的同时要自动执行构造函数，对象在消亡之前要自动执行析构函数。
  * new 可以认为是 malloc 加构造函数的执行。new 出来的指针是直接带类型信息的。而 malloc 返回的都是 void 指针。

* typedef 和 #define 的区别
  * #define 宏定义发生在预处理阶段，简单地进行字符串替换，没有类型检查
  * typedef 发生在编译阶段，会进行类型检查
  * typedef 只是定义类型的别名，定义与平台无关的数据类型，#define 不只是可以为类型取别名，还可以定义常量等
  * #define 没有作用域，只要之前预定义过的宏在之后都可以使用；typedef 有作用域
  * [typedef 和 #define 的区别](https://blog.csdn.net/luoweifu/article/details/41630195)

## 虚函数

* c++虚函数
  * 虚函数是一种在基类定义为 virtual 的函数，并在一个或多个派生类中再定义的函数。虚函数的特点是，只要定义一个基类的指针，就可以指向派生类的对象。
  * 注：无虚函数时，遵循以下规则：C++ 规定，定义为基类的指针，也能作指向派生类的指针使用，并可以用这个指向派生类对象的指针访问继承来的基类成员；但不能用它访问派生类的成员
  * 使用虚函数实现运行时的多态性的关键在于：必须通过基类指针访问这些函数
  * 含有纯虚函数的基类称为抽象基类。抽象基类又一个重要特性：抽象类不能建立对象。但是抽象基类可以有指向自己的指针，以支持运行时的多态性。
  * 它虚就虚在所谓“推迟联编”或者“动态联编”上，一个类函数的调用并不是在编译时刻被确定的，而是在运行时刻被确定的。由于编写代码的时候并不能确定被调用的是基类的函数还是哪个派生类的函数，所以被成为“虚”函数。
  * 纯虚函数一定没有定义，纯虚函数用来规范派生类的行为，即接口。包含纯虚函数的类是抽象类

* 虚函数覆盖的条件
  * 只有类的成员函数才能被声明为虚函数，全局函数和类的静态成员函数都不能被声明为虚函数。
  * 只有在基类中被冠以virtual关键字的成员函数才能作为虚函数被子类覆盖，而与子类中virtual关键字无关。

* c++ 析构函数为何需要使用Visual修饰
  * C++ 中基类采用 virtual 虚析构函数是为了防止内存泄漏
  * 如果派生类中申请了内存空间，并在其析构函数中对这些内存空间进行释放。假设基类中采用的是非虚析构函数，当删除基类指针指向的派生类对象时就不会触发动态绑定，因而只会调用基类的析构函数，而不会调用派生类的析构函数。那么在这种情况下，派生类中申请的空间就得不到释放从而产生内存泄漏

## STL

* set 的实现原理

* map 数据结构，时间复杂度
  * 基于红黑树的实现方式，添加到一个有序列表，在 O(log n) 的复杂度内通过 key 找到 value，优点是空间复杂度小，时间性能不如 unordered_map
  * 数据有序

* unordered_map 数据结构，时间复杂度
  * 基于哈希的实现方式，空间复杂度大但时间复杂度小
  * 数据无序

* vector 的实现原理
  * vector push_back() 方法增加元素时进行了拷贝操作，原来数据不影响 vector 元素，当元素为指针时，拷贝的是指针本身不是指针指向的对象
  * vector 内部是一个数组，是一块连续内存，有三个迭代器，一个指向数据段头，一个指向数据段尾，一个指向可用空间的尾
  * vector 插入时如果空间不够，会开辟一块2倍的内存空间，拷贝数据，释放原空间

* list 的实现原理
  * 底层实现：带头结点的双向循环链表

## 内存

* **c++内存管理模型**
* stl 内存分配
* c++ 内存
  * c++ 内存分为栈、堆、程序代码区、静态区、文字常量区
  * 栈存放临时变量、函数
  * 堆存放 new 的对象
  * 虚函数表在静态区

## 其他

* c++11 新特性
  * nullptr：解决 NULL 无法正确重载问题
  * 类型推导 auto，可以写出简单的迭代器循环
  * 基于范围的 for 循环，例如：for(auto index : arr){}
  * thread：语言级线程支持
  * 新增容器：array、unorder_map、unorder_set
  * [c++11 常用特性总结](https://www.cnblogs.com/feng-sc/p/5710724.html)
  * [c++11 新特性一览](https://blog.csdn.net/jiange_zh/article/details/79356417)

* go 与 cpp 的区别
  * go 用于网络编程，语法统一，没有 cpp 那些炫技的地方，开发速度快
  * go 内存管理由 gc 负责，不用程序员自己负责
  * go 并发编程更简单，协程的使用使得代码更易于理解，而且协程信息交流基于 channel，简单方便
  * 思考问题的思路：go 被用在什么地方？服务器开发；写起来和 cpp 有什么不同？没有 delete 这些内存操作；go 有什么特别的地方？协程和管道；为什么有协程和管道？便于并发编程
  * [go 优势](https://blog.csdn.net/QcloudCommunity/article/details/80735770)

* 参考书籍《深度探索c++对象模型》、《STL源码剖析》
* [常见c++面试题](https://www.cnblogs.com/LUO77/p/5771237.html)
* [胡子昂腾讯面试笔记](https://github.com/bilibiliChangKai/My-Note/blob/master/%E9%9D%A2%E8%AF%95/TX%E9%9D%A2%E8%AF%95.md)