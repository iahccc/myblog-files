---
title: Java笔记
cover: 'https://cdn.jsdelivr.net/gh/iahccc/PicBed/img/archive_img.jpeg'
date: 2020-12-12 11:05:07
tags: java
categories:
---

# Java基础

## "=="和equals有什么区别？
1. “==”对于基本类型来说比较的是值是否相同，对于引用类型来说比较多是引用的地址是否相同。

2. equals本质上就是“==”，比较的是引用，String、Integer等类重写了equals方法使其变成了值比较。

## 两个对象的 hashCode()相同，则 equals()也一定为 true，对吗？
错误，两个不相同的对象有可能会发生hash冲突。

## final 在 java 中有什么作用？
* final修饰的类叫最终类，该类不能被继承。
* final修饰的方法不能被重写。
* final修饰的变量叫常量，常量必须初始化，初始化之后值就不能再被更改了。

## java 中的 Math.round(-1.5) 等于多少？
等于-1，该方法原理是原数+0.5再向下取整。

## String 属于基础的数据类型吗？
不属于，String属于引用类型，基础数据类型有8种：byte,short,int,long,char,boolean,float,double。

## java 中操作字符串都有哪些类？它们之间有什么区别？
String,StringBuilder,StringBuffer。String声明的是不可变对象，其余两个可以在原有对象的基础上进行操作，所以经常对字符串进行操作的情况下最好不要使用String类型。

StringBuilder和Stringbuffer的区别是前者线程不安全，且性能较高，后者线程安全，但性能较弱。

## String str="i"与 String str=new String(“i”)一样吗？
不一样，虚拟机会将前者分配到常量池中，而后者会被分配到堆内存中。

## 如何将字符串反转？
使用StringBuffer或StringBuilder中的reverse()方法。

## String 类的常用方法都有哪些？
* indexof
* split
* replace
* chartAt
* length
* toLowerCase
* toUpperCase
* substring
* equals
* trim
* getBytes

## 抽象类必须要有抽象方法吗？
不是必须的。

## 抽象类能使用 final 修饰吗？
不能。抽象类本来就是要让其他类继承的，抽象类使用final修饰就代表了不能被继承，这与抽象类的设计理念产生矛盾。

## 接口和抽象类有什么区别？
...

## java 中 IO 流分为几种？
* 按功能分：输入流、输出流。
* 按类型分：字节流、字符流。

## BIO、NIO、AIO 有什么区别？
* BIO: Block IO同步阻塞式IO。
* NIO：New IO同步非阻塞试IO，是BIO的升级，客户端和服务端通过Channel通讯，实现多路复用。
* AIO：Asynchronous IO是NIO的升级，也叫NIO2。实现了异步非阻塞IO，异步IO的操作是基于事件和回调机制。

## Files的常用方法都有哪些？
* exits
* createFile
* createDirectory
* delete
* copy
* move
* size
* read
* write

# Java容器
## java 容器都有哪些？
* List
    1. ArrayList
    2. LinkedList
    3. Vector
    4. CopyOnWriteArrayList
    
* Queue
    * 非阻塞队列
        1. LinkedList
        2. PriorityQueue
        3. ConcurrentLnkedQueue
    * 阻塞队列
        1. ArrayBlockingQueue
        2. LinkedBlockingQueue
        3. PriorityBlockingQueue
        4. DelayQueue
        5. SynchronousQueue
        
* Map
    1. HashMap
    2. LinkedHashMap
    3. HashTable
    4. TreeMap
    5. ConCurrentHashMap

* Set
    1. HashSet
    2. LinkedHashSet
    3. TreeSet

## Collection 和 Collections 有什么区别？
Collection是集合的顶级接口，Collections是集合的一个工具类，提供了对集合进行排序、搜索以及线程安全操作等方法。

## HashMap 和 Hashtable 有什么区别？
1. HashTable是线程安全的，但是效率比HashMap低。
2. HashMap允许空键值，而HashTable不允许。

## 如何决定使用 HashMap 还是 TreeMap？
如果需要对一个有序的key集合进行排序操作，则选择TreeMap，否则选择HashMap。

## 说一下 HashMap 的实现原理？
HashMap实际上是一个数组和链表的结合体。当我们往HashMap中put元素时，它首先会根据key的hashcode计算hash值，然后根据hash值得到这个元素在数组中的位置。如果该位置没有元素，则直接将该元素放到数组的该位置上。如果该位置已经有了其他元素，这种情况就叫hash冲突，HashMap会将这个位置上的元素以链表的形式存放，jdk1.8中以尾插法的方式插入元素，如果链表中的元素超过8个，该链表会转为红黑树来提高查询效率。

## 说一下 HashSet 的实现原理？
HashSet的底层由HashMap实现，它的值存在HashMap的key上，HashMap的value统一为PRESENT。


## ArrayList 和 LinkedList 的区别是什么？
ArrayList底层数据结构是数组，支持随机访问。LinkedList底层是双向循环链表。ArrayList查找元素时，时间复杂度为O(1),LinkedList为O(n)。

## 如何实现数组和 List 之间的转换？
* List转数组：调用List的toArray方法。
* 数组转List：调用Arrays的asList方法。

## ArrayList 和 Vector 的区别是什么？
Vector是线程安全的，ArrayList不是。一般使用ArrayList较多，而且它可以使用Collections工具类将其变为同步集合。

## Array 和 ArrayList 有何区别？
1. Array大小固定，ArrayList大小可变。
2. Array中能容纳基本类型和对象，而ArrayList只能容纳对象。
3. ArrayList提供更多方法。

## 在 Queue 中 poll()和 remove()有什么区别？
两者都是从队列中取出一个元素，前者当获取元素失败时返回null，后者会抛出异常。

## 哪些集合类是线程安全的？
1. Vector/HashTable
2. CopyOnWriteArrayList/ConcurrentHashMap

## 迭代器 Iterator 是什么？
迭代器是一种设计模式，他是一个对象，它可以遍历并选择序列中的对象，而开发人员不需要了解该序列的底层结构。迭代器通常被称为“轻量级”对象，因为创建它的代价小。

## Iterator 怎么使用？有什么特点？
Java中的Iterator功能比较简单，并且只能单向移动：

(1) 使用方法iterator()要求容器返回一个Iterator。第一次调用Iterator的next()方法时，它返回序列的第一个元素。注意：iterator()方法是java.lang.Iterable接口,被Collection继承。

(2) 使用next()获得序列中的下一个元素。

(3) 使用hasNext()检查序列中是否还有元素。

(4) 使用remove()将迭代器新返回的元素删除。　

Iterator是Java迭代器最简单的实现，为List设计的ListIterator具有更多的功能，它可以从两个方向遍历List，也可以从List中插入和删除元素。

## Iterator 和 ListIterator 有什么区别？
* Iterator可用来遍历Set和List集合，但是ListIterator只能用来遍历List。 

* Iterator对集合只能是前向遍历，ListIterator既可以前向也可以后向。 

* ListIterator实现了Iterator接口，并包含其他的功能，比如：增加元素，替换元素，获取前一个和后一个元素的索引，等等。

## 怎么确保一个集合不能被修改？
使用Collectioins类的unmodifiableCollection方法。

# 多线程
## 并行和并发有什么区别？
* 并行指两个或多个事件在同一时刻发生。
* 并发指两个或多个事件在同一时间间隔内发生。

## 线程和进程的区别？
* 进程：是执行中的一段程序，一旦程序被载入内存中请准备执行，他就是一个进程。
* 线程：单个进程中执行的任务就是一个线程，线程是进程中执行运算的最小单位。
* 一个程序至少有一个进程，一个进程至少有一个线程。进程在运行过程中有独立的内存单元，而多线程共享内存，极大提高了程序的执行效率。

## 守护线程是什么？
守护线程是为了服务于主线程，当主线程消亡，它也会被销毁。

## 创建线程有哪几种方式？
1. 继承Thread类
2. 实现Runable接口
3. 实现Callable接口

## 说一下 runnable 和 callable 有什么区别？
* Runcable接口中run方法的返回值是void
* Callable接口中run方法的返回值为泛型。通过与Future、FutureTask配合可以获取异步执行结果。

## 线程有哪些状态？
1. 创建状态：生成线程对象，还没有调用start方法。
2. 就绪状态：调用线程对象的start方法后进入就绪状态。线程阻塞结束之后也会进入就绪状态。
3. 运行状态：当线程获取到CPU时间片之后，进入运行状态。
4. 阻塞状态：调用sleep/join方法时进入阻塞状态。
5. 死亡状态：run方法执行结束或异常退出。

## sleep() 和 wait() 有什么区别？
调用sleep方法会使线程进入休眠状态，休眠结束之后，会进入就绪状态。需要注意点事sleep方法不会释放锁。
wait方法是Object类中的方法，当调用该方法后，该线程就会进入该对象相关的等待池，同时会释放所持有的锁，可以通过notify/notifyAll方法来唤醒等待的线程。

## notify()和 notifyAll()有什么区别？
notify方法随机唤醒一个等待池中的线程，而notifyAll会将所有等待池中等的线程一起唤醒。

## 线程的 run()和 start()有什么区别？
start方法是多线程的启动方法，直接调用run方法就等于直接调用了一个普通的函数。

## 创建线程池有哪几种方式？
1. newFixedThreadPool
    这是一个长度固定的线程池。
2. newCachedThreadPool
    可缓存的线程池。
3. newSingleThreadExecutor
    单线池。
4. newScheduledThreadPoll
    长度固定的线程池，并且以延迟或者定时的方式来执行任务，类似于Timer。

## 线程池都有哪些状态？
...

## 线程池中 submit()和 execute()方法有什么区别？
1. 接受参数不一样。
2. suubmit有返回值，execute没有。
3. submit方便Exception处理。

## 在 java 程序中怎么保证多线程的运行安全？
线程安全在三个方面体现：

* 原子性：提供互斥访问，同一时刻只能有一个线程对数据进行操作，（atomic,synchronized）；

* 可见性：一个线程对主内存的修改可以及时地被其他线程看到，（synchronized,volatile）；

* 有序性：一个线程观察其他线程中的指令执行顺序，由于指令重排序，该观察结果一般杂乱无序，（happens-before原则）。

## 多线程锁的升级原理是什么？
...

## 什么是死锁？
指两个或两个以上的线程互相持有对方所需要的锁，导致这些线程一直处于等待状态，无法向下执行。

## 怎么防止死锁？
死锁的四个必要条件：

    * 互斥条件：进程对所分配到的资源不允许其他进程进行访问，若其他进程访问该资源，只能等待，直至占有该资源的进程使用完成后释放该资源

    * 请求和保持条件：进程获得一定的资源之后，又对其他资源发出请求，但是该资源可能被其他进程占有，此事请求阻塞，但又对自己获得的资源保持不放

    * 不可剥夺条件：是指进程已获得的资源，在未完成使用之前，不可被剥夺，只能在使用完后自己释放

    * 环路等待条件：是指进程发生死锁后，若干进程之间形成一种头尾相接的循环等待资源关系

这四个条件是死锁的必要条件，只要系统发生死锁，这些条件必然成立，而只要上述条件之 一不满足，就不会发生死锁。

理解了死锁的原因，尤其是产生死锁的四个必要条件，就可以最大可能地避免、预防和 解除死锁。
避免死锁的方式：
    * 加锁顺序（线程按照一定的顺序加锁）
    * 加锁时限（线程尝试获取锁的时候加上一定的时限，超过时限则放弃对该锁的请求，并释放自己占有的锁）
    * 死锁检测
## ThreadLocal 是什么？有哪些使用场景？
ThreadLocal是维护线程内变量的类，该变量只能在当前线程访问，相对于其他线程是隔离的。例如可将JDBC的连接对象Connection放入ThreadLocal中。

## 说一下 synchronized 底层实现原理？
* 普通同步方法：锁是当前对象。
* 静态同步方法：锁是当前类。
* 同步方法快：锁是括号中的对象。

## synchronized 和 volatile 的区别是什么？
* volatile本质是告诉jvm当前变量在寄存器（工作内存）中的值是不确定的，需要从主存中读取；synchronize这是锁定当前变量，只有当前线程可以访问改变量。
* volatile仅能使用在变量级别；synchronized则可以使用在变量、方法、和类级别的。

* volatile仅能实现变量的修改可见性，不能保证原子性；而synchronized则可以保证变量的修改可见性和原子性。

* volatile不会造成线程的阻塞；synchronized可能会造成线程的阻塞。

* volatile标记的变量不会被编译器优化；synchronized标记的变量可以被编译器优化。

## synchronized 和 Lock 有什么区别？
首先synchronized是java内置关键字，在jvm层面，Lock是个java类；

synchronized无法判断是否获取锁的状态，Lock可以判断是否获取到锁；

synchronized会自动释放锁(a 线程执行完同步代码会释放锁 ；b 线程执行过程中发生异常会释放锁)，Lock需在finally中手工释放锁（unlock()方法释放锁），否则容易造成线程死锁；

用synchronized关键字的两个线程1和线程2，如果当前线程1获得锁，线程2线程等待。如果线程1阻塞，线程2则会一直等待下去，而Lock锁就不一定会等待下去，如果尝试获取不到锁，线程可以不用一直等待就结束了；

synchronized的锁可重入、不可中断、非公平，而Lock锁可重入、可判断、可公平（两者皆可）；

Lock锁适合大量同步的代码的同步问题，synchronized锁适合代码少量的同步问题。

## synchronized 和 ReentrantLock 区别是什么？
synchronized是和if、else、for、while一样的关键字，ReentrantLock是类，这是二者的本质区别。既然ReentrantLock是类，那么它就提供了比synchronized更多更灵活的特性，可以被继承、可以有方法、可以有各种各样的类变量，ReentrantLock比synchronized的扩展性体现在几点上： 

ReentrantLock可以对获取锁的等待时间进行设置，这样就避免了死锁 

ReentrantLock可以获取各种锁的信息

ReentrantLock可以灵活地实现多路通知 

另外，二者的锁机制其实也是不一样的:ReentrantLock底层调用的是Unsafe的park方法加锁，synchronized操作的应该是对象头中mark word。

## 说一下 atomic 的原理？
JDK Atomic开头的类，是通过 CAS 原理解决并发情况下原子性问题。

CAS 包含 3 个参数，CAS(V, E, N)。V 表示需要更新的变量，E 表示变量当前期望值，N 表示更新为的值。只有当变量 V 的值等于 E 时，变量 V 的值才会被更新为 N。如果变量 V 的值不等于 E ，说明变量 V 的值已经被更新过，当前线程什么也不做，返回更新失败。

当多个线程同时使用 CAS 更新一个变量时，只有一个线程可以更新成功，其他都失败。失败的线程不会被挂起，可以继续重试 CAS，也可以放弃操作。

CAS 操作的原子性是通过 CPU 单条指令完成而保障的。JDK 中是通过 Unsafe 类中的 API 完成的。

在并发量很高的情况，会有大量 CAS 更新失败，所以需要慎用。


参考博文：[最新Java面试题，常见面试题及答案汇总](https://blog.csdn.net/fangchao2011/article/details/89203535)
