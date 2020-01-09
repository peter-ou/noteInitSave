# java 基础 01

- [java 基础 01](#java-基础-01)
  - [java 取模与取余的区别](#java-取模与取余的区别)
  - [你知道HashMap底层的数据结构是什么吗?](#你知道hashmap底层的数据结构是什么吗)
  - [JDK 1.8中对hash算法和寻址算法是如何优化的呢?](#jdk-18中对hash算法和寻址算法是如何优化的呢)
  - [你知道HashMap是如何解决hash碰撞问题的吗？](#你知道hashmap是如何解决hash碰撞问题的吗)
  - [说说HashMap是如何进行扩容的可以吗？](#说说hashmap是如何进行扩容的可以吗)
  - [说说synchronized关键字的底层原理是什么？](#说说synchronized关键字的底层原理是什么)
  - [能聊聊你对CAS的理解以及其底层实现原理可以吗 ?](#能聊聊你对cas的理解以及其底层实现原理可以吗-)
  - [ConcurrentHashMap实现线程安全的底层原理到底是什么？](#concurrenthashmap实现线程安全的底层原理到底是什么)
  - [你对JDK中的AQS理解吗？AQS的实现原理是什么？](#你对jdk中的aqs理解吗aqs的实现原理是什么)
  - [说说线程池的底层工作原理可以吗？](#说说线程池的底层工作原理可以吗)
  - [说说线程池的核心配置参数都是干什么的？平时我们应该怎么用？](#说说线程池的核心配置参数都是干什么的平时我们应该怎么用)
  - [如果线上机器突然宕机，线程池的阻塞队列中的请求怎么办？](#如果线上机器突然宕机线程池的阻塞队列中的请求怎么办)
  - [谈谈你对Java内存模型的理解可以吗？](#谈谈你对java内存模型的理解可以吗)
  - [你知道Java内存模型中的原子性、有序性、可见性是什么吗？](#你知道java内存模型中的原子性有序性可见性是什么吗)
  - [能从Java底层角度聊聊volatile关键字的原理吗？](#能从java底层角度聊聊volatile关键字的原理吗)
  - [你知道指令重排以及happens-before原则是什么吗？](#你知道指令重排以及happens-before原则是什么吗)
  - [volatile底层是如何基于内存屏障保证可见性和有序性的？](#volatile底层是如何基于内存屏障保证可见性和有序性的)
  - [说说你对Spring的IOC机制和AOP机制的理解可以吗？](#说说你对spring的ioc机制和aop机制的理解可以吗)
  - [说说你对Spring的AOP机制的理解可以吗？](#说说你对spring的aop机制的理解可以吗)
  - [了解过cglib动态代理吗？他跟jdk动态代理的区别是什么？](#了解过cglib动态代理吗他跟jdk动态代理的区别是什么)
  - [能说说Spring中的Bean是线程安全的吗？](#能说说spring中的bean是线程安全的吗)
  - [Spring的事务实现原理是什么？能聊聊你对事务传播机制的理解吗？](#spring的事务实现原理是什么能聊聊你对事务传播机制的理解吗)

## java 取模与取余的区别

1, 对于整型数a，b来说，取模运算或者求余运算的方法都是：
  
- 第一步：求 整数商： c = a/b;

- 第二步：计算模或者余数： r = a - c * b

> 求模运算和求余运算在第一步不同: 取余运算在取c的值时，向0 方向舍入(fix()函数)；而取模运算在计算c的值时，向负无穷方向舍入(floor()函数)。

例如计算：-7 Mod 4
那么：a = -7；b = 4；

- 第一步：求整数商c，如进行求模运算c = -2（**向负无穷方向舍入**），求余c = -1（**向0方向舍入**）；

- 第二步：计算模和余数的公式相同，但因c的值不同，求模时r = 1，求余时r = -3。

2, 归纳：当a和b符号一致时，求模运算和求余运算所得的c的值一致，因此结果一致。
  
- 当符号不一致时，结果不一样。**求模运算结果的符号和b（除数）一致，求余运算结果的符号和a（被除数）一致**。

- 另外各个环境下%运算符的含义不同，比如c/c++，java 为取余，而python则为取模。

3，Java程序示例：

```
public static void main(String[] args) {
        System.out.println("-3,2取模"+Math.floorMod(-3,2));
        System.out.println("-3,2取余"+ -3%2);
        System.out.println("3,-2取模"+Math.floorMod(3,-2));
        System.out.println("3,-2取余"+ 3%-2);
    }

```

运行结果：

``` 
-3,2取模=1
-3,2取余=-1
3,-2取模=-1
3,-2取余=1

```

## 你知道HashMap底层的数据结构是什么吗?

1，底层的数据结构是：**数组**

详情说明(举例)：

``` 
HashMap<String, String> map = new HashMap<String, String>();
map.put(“张三”, “测试数据”);
map.put(“李四”, “测试数据”);
```

- hashmap的存值过程：
  
对key"张三"计算出来一个hash值，根据这个hash值对数组进行取模，就会定位到数组里的一个元素中去.

``` 
[<>, <>, <>, <>,<张三, 测试数据>, <>,<>,<李四, 测试数据>,<>, <>, <>, <>,<>, <>, <>, <>]

array[4] = <张三, 测试数据>
```

- hashmap取值的过程

map.get(“张三”) -> hash值 -> 对数组长度进行取模 -> return array[4]

## JDK 1.8中对hash算法和寻址算法是如何优化的呢?

1,hash算法的优化：对每个hash值，在他的低16位中，让高低16位进行了异或，让他的低16位同时保持了高低16位的特征，尽量避免一些hash值后续出现冲突，大家可能会进入数组的同一个位置

2,寻址算法的优化：用与运算替代取模，提升性能

- hash算法优化详情：

源码

``` 
      // JDK 1.8以后的HashMap里面的一段源码
    static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}

```

``` 
// key.hashCode()比如说：有一个key的hash值 A
1111 1111 1111 1111 1111 1010 0111 1100
// A >>> 16 这是向右移动16位后的值 B
0000 0000 0000 0000 1111 1111 1111 1111 
// 这是 A ^ B  (A 异或 B) 的值
1111 1111 1111 1111 0000 0101 1000 0011 -> int值，32位。
```

这样就实现了：让他的低16位同时保持了高低16位的特征，尽量避免一些hash值后续出现的冲突

>因为hash值一样 -> 他们其实都会在数组里放在一个位置，进行复杂的hash冲突的处理，将影响性能。

hash值对数组长度取模，定位到数组的一个位置，将key-value塞数组里面就ok了

- 寻址算法优化

优化的核心点是：

```
将hash对n取模 -> 数组里的一个位置 

改成：
(n - 1) & hash -> 数组里的一个位置

得到的数组位置是一样的，(n - 1) & hash的性能更高。

同时注意：（n-1）主要是和 hash 的低16位进行与运算。
高16位之间的与运算，是可以忽略的，核心点在于低16位的与运算，hash值的高16位没有参与到与运算里来。

// 当只有高16位不一样时，我们进行高16位低16位异或运算，从而实现hash的低16位不一样。
1111 1111 1111 1111 1111 1010 0111 1100 -> （异或后：）1111 1111 1111 1111 0000 0101 1000 0011

1111 1111 1111 1110 1111 1010 0111 1100 -> （异或后：）1111 1111 1111 1110 0000 0101 1000 0010

```

hash & (n - 1) -> 效果是跟hash对n取模，效果是一样的，但是与运算的性能要比hash对n取模要高很多，数学问题，**数组的长度会一直是2的n次方**。

>说明: hash & (n - 1) 中的hash值也是经过上面优化过后的值（即：他的低16位同时保持了高低16位的特征）

## 你知道HashMap是如何解决hash碰撞问题的吗？

1，hash冲突问题，链表+红黑树，O(n)和O(logn)。

> 尽管jdk1.8做了 map.put和map.get -> hash算法优化（避免hash冲突），寻址性能优化。

> 算出key的hash值，到数组中寻址，找到一个位置，把key-value对放进数组，或者从数组里取出来

> 但是两个key，多个key，他们算出来的hash的值，和n-1，与运算之后，发现定位出来的数组的位置还是一样的，还是会出现hash碰撞或者叫hash冲突。

- 这时怎么处理呢？
  
1，会在这个位置挂一个链表，这个链表里面放入多个元素，让多个key-value对，同时放在数组的一个位置里。例如：

```
[<> -> <> -> <>, ]
array[0]这个位置，就是一个链表
```

get取值时，如果定位到数组里发现这个位置挂了一个链表，此时遍历链表O(n)，从里面找到自己的要找的那个key-value对就可以了

2，假设你的链表很长，可能会导致遍历链表性能会比较差。

所以链表的长度达到了一定的长度之后，其实会把**链表转换为红黑树**，遍历一颗红黑树找一个元素，此时O(logn)，性能会比链表遍历O(n)高一些。

## 说说HashMap是如何进行扩容的可以吗？

1，2倍扩容
HashMap底层是一个数组，当这个数组满了之后，他就会自动进行扩容，变成一个更大的数组，让你在里面可以去放更多的元素。

2，判断新数组（n-1）& hash 的二进制结果中是否多出一个bit的1，如果没多，那么就是原来的index位置，如果多了出来，那么就是 **原index + oldCap（为原数组length）**，通过这个方式，就避免了rehash的时候，用每个hash对新数组.length取模，取模性能不高，位运算的性能比较高。

3，扩容可能带来的变化过程
[16位的数组，<> -> <> -> <>]
[32位的数组，<> -> <>, <>]
扩容后，原来数组同一位置上的链结构可能被拆分到另外的位置上去。

例如：数组长度是 16时, hash1和hash2会在数组的同一个位置上，出现一个hash冲突的问题，用链表来处理。

``` 
n - 1     0000 0000 0000 0000 0000 0000 0000 1111
hash1     1111 1111 1111 1111 0000 1111 0000 0101
&结果     0000 0000 0000 0000 0000 0000 0000 0101    = 5（index = 5的位置）
 
n - 1    0000 0000 0000 0000 0000 0000 0000 1111
hash2    1111 1111 1111 1111 0000 1111 0001 0101
&结果    0000 0000 0000 0000 0000 0000 0000 0101 = 5（index = 5的位置）

```

如果数组的长度扩容之后 = 32，重新对每个hash值进行寻址，也就是用每个hash值跟新数组的length - 1进行与操作：

```
n-1       0000 0000 0000 0000 0000 0000 0001 1111
hash1     1111 1111 1111 1111 0000 1111 0000 0101
&结果     0000 0000 0000 0000 0000 0000 0000 0101 = 5（index = 5的位置）
 
n-1       0000 0000 0000 0000 0000 0000 0001 1111
hash2     1111 1111 1111 1111 0000 1111 0001 0101
&结果     0000 0000 0000 0000 0000 0000 0001 0101 = 21（index = 21的位置）

```

hash1和hash2将在新数组中的不同位置。位置变化规律请参考第2点。

## 说说synchronized关键字的底层原理是什么？

1,synchronized底层的原理，是跟jvm指令和monitor有关系的。
如果我要是对synchronized往深了讲，他是可以很深很深的，内存屏障的一些东西，cpu之类的硬件级别的原理，原子性、可见性、有序性，指令重排，JDK对他实现了一些优化，偏向锁。

<div align='center'>
<img src=./images/java-基础01/java-基础01_2020-01-06-16-48-48.png width='80%'/></div>
<br/>

你如果用到了synchronized关键字，在底层编译后的jvm指令中，会有monitorenter和monitorexit两个指令

```
monitorenter
 
// 代码对应的指令
 
monitorexit
```

2,那么monitorenter和monitorexit指令执行的时候会干什么呢？

- 他里面的原理和思路大概是这样的，monitor里面有一个计数器，从0开始的。如果一个线程要获取monitor的锁，就看看他的计数器是不是0，如果是0的话，那么说明没人获取锁，他就可以获取锁了，然后对计数器加1
  
- 这个monitor的锁是支持重入加锁的，什么意思呢，好比下面的代码片段。
如果一个线程第一次synchronized那里，获取到了myObject对象的monitor的锁，计数器加1，然后第二次synchronized那里，会再次获取myObject对象的monitor的锁，这个就是重入加锁了，然后计数器会再次加1，变成2

```
// 线程1
synchronized(myObject) {  // 类的class对象来走的,加锁，一般来说都是必须对一个对象进行加锁
// 一大堆的代码
    synchronized(myObject) {
    // 一大堆的代码
    }
}

```

- 每个对象都有一个关联的monitor，比如一个对象实例就有一个monitor，一个类的Class对象也有一个monitor，如果要对这个对象加锁，那么必须获取这个对象关联的monitor的lock锁

- 这个时候，其他的线程在第一次synchronized那里，执行monitorenter会发现说myObject对象的monitor锁的计数器是大于0的，意味着被别人加锁了，然后此时线程就会进入block阻塞状态，什么都干不了，就是等着获取锁
  
- 接着如果线程1出了synchronized修饰的代码片段的范围，在底层，就会有一个monitorexit的指令。此时获取锁的线程就会对那个对象的monitor的计数器减1，如果有多次重入加锁就会对应多次减1，直到最后，计数器是0

- 然后后面block住阻塞的线程，会再次尝试获取锁，但是只有一个线程可以获取到锁。

## 能聊聊你对CAS的理解以及其底层实现原理可以吗 ? 

<div align='center'><img src=./images/java-基础01/java-基础01_2020-01-06-17-19-05.png width='80%'/></div><br/>

<div align='center'><img src=./images/java-基础01/java-基础01_2020-01-06-17-19-23.png width='100%'/></div><br/>

CAS:取值，询问，修改

多个线程他们可能要访问同一个数据
 
HashMap map = new HashMap();
 
此时有多个线程要同时读写类似上面的这种内存里的数据，此时必然出现多线程的并发安全问题。
 
我们可能要用到并发包下面的很多技术，例如：synchronized

synchronized(map) {
   // 对map里的数据进行复杂的读写处理
}
 
并发包下面的其他的一些技术，例如CAS
 
一段代码（非CAS实现）：
 
<div align='center'><img src=./images/java-基础01/java-基础01_image.png.png width='80%'/></div><br/>
 
此时，synchronized他的意思就是针对当前执行这个方法的myObject对象进行加锁
 
只有一个线程可以成功的堆myObject加锁，可以对他关联的monitor的计数器去加1，加锁，一旦多个线程并发的去进行synchronized加锁，串行化，效率并不是太高，很多线程，都需要排队去执行。

CAS的全称：compare and set 比较和设置。
CAS去进行安全的累加代码：

<div align='center'><img src=./images/java-基础01/java-基础01_2020-01-06-17-21-00.png width='80%'/></div><br/>
 
CAS在底层的硬件级别给你保证一定是原子的，同一时间只有一个线程可以执行CAS，先比较再设置，其他的线程的CAS同时间去执行此时会失败

## ConcurrentHashMap实现线程安全的底层原理到底是什么？

1，JDK 1.8以前，多个数组，分段加锁，一个数组一个锁。

2，JDK 1.8以后，优化细粒度，一个数组，每个元素先进行CAS，如果失败说明有人put过值了，此时synchronized对这个数组元素加锁，链表+红黑树处理。jdk1.8+是对数组每个元素加锁。

- 多个线程要访问同一个数据，synchronized加锁，CAS去进行安全的累加，去实现多线程场景下的安全的更新一个数据的效果，比较多的一个场景下，可能就是多个线程同时读写一个HashMap,如果给HashMap加一个synchronized，效率将会很低，也没这个必要。
  
```
// 多个线程过来，线程1要put的位置是数组[5]，线程2要put的位置是数组[21]，
//这种情况下根本不需要synchronized同步，同步了反而会降低效率，明显不好。
map.put(xxxxx,xxx);
//数组里有很多的元素，除非是对同一个元素执行put操作，此时的多线程是需要进行同步的
```

- 所以 JDK并发包里推出了一个ConcurrentHashMap，他默认实现了线程安全性。实现过程如下：

> HashMap的一个底层的原理，本身是一个大的一个数组，[有很多的元素]

（1），在JDK 1.7以及之前的版本里，将这个大的数组分段成多个数组
 
[数组1] , [数组2]，[数组3] -> 每个数组都对应一个锁，分段加锁
 
// 多个线程过来，线程1要put的位置是数组1[5]，线程2要put的位置是数组2[21]

（2），JDK 1.8以及之后，做了一些优化和改进，锁粒度的细化。
 
[一个大的数组]，数组里每个元素进行put操作，都是有一个不同的锁，刚开始进行put的时候，如果两个线程都是在数组[5]这个位置进行put，这个时候，对数组[5]这个位置进行put的时候，采取的是CAS的策略
 
同一个时间，只有一个线程能成功执行这个CAS，就是说他刚开始先获取一下数组[5]这个位置的值，null，线程1然后执行CAS，put进去我的这条数据，同时，其他的线程执行CAS，都会失败
 
通过对数组每个元素执行CAS的策略，如果是很多线程对数组里不同的元素执行put，大家是没有关系的，如果其他人失败了，其他人此时会发现说，数组[5]这位置，已经有人放进去值了，synchronized对数组这个元素加锁。

## 你对JDK中的AQS理解吗？AQS的实现原理是什么？

1，AQS ：Abstract Queue Synchronizer，抽象队列同步器。

```
ReentrantLock lock = new ReentrantLock(true);  => 公平锁
//ReentrantLock lock = new ReentrantLock();  => 非公平锁，默认是非公平锁
// 多个线程过来，都尝试
lock.lock();
// 进来的线程 执行的一堆代码TODO
lock.unlock();

```

上面代码ReentrantLock对象使用时的执行过程
state变量 -> CAS -> 失败后进入队列等待 -> 释放锁后唤醒

2， 实现原理图

<div align='center'><img src=./images/java-基础01/java-基础01_2020-01-07-16-08-08.png width='80%'/></div><br/>

<div align='center'><img src=./images/java-基础01/java-基础01_2020-01-07-16-21-22.png width='80%'/></div><br/>

<div align='center'><img src=./images/java-基础01/java-基础01_2020-01-07-16-28-09.png width='80%'/></div><br/>

> 公平锁执行原理图中： 线程3 先看一下等待队列是否 有人排队，有，直接进入等待队列排队。没有，当然是去获取锁咯。

## 说说线程池的底层工作原理可以吗？

原理图1：

<div align='center'><img src=./images/java-基础01/java-基础01_2020-01-07-17-37-28.png width='80%'/></div><br/>

原理图2：//线程释放后，线程会去队列中争抢获取任务执行的

<div align='center'><img src=./images/java-基础01/java-基础01_2020-01-07-17-40-24.png width='80%'/></div><br/>

代码：

```
ExecutorService threadPool = Executors.newFixedThreadPool(3) // 3: corePoolSize
 
threadPool.submit(new Callable() {
       public void run() {}
})；
```

提交任务，先看一下线程池里的线程数量是否小于corePoolSize，也就是3，如果小于，直接创建一个线程出来执行你的任务
 
如果执行完你的任务之后，这个线程是不会死掉的，他会尝试从一个无界的LinkedBlockingQueue里获取新的任务，如果没有新的任务，此时就会阻塞住，等待新的任务到来
 
你持续提交任务，上述流程反复执行，只要线程池的线程数量小于corePoolSize，都会直接创建新线程来执行这个任务，执行完了就尝试从无界队列里获取任务，直到线程池里有corePoolSize个线程
 
接着再次提交任务，会发现线程数量已经跟corePoolSize一样大了，此时就直接把任务放入队列中就可以了，**线程会争抢获取任务执行的**，如果所有的线程此时都在执行任务，那么无界队列里的任务就可能会越来越多
 
fixed，队列，LinkedBlockingQueue，无界阻塞队列

## 说说线程池的核心配置参数都是干什么的？平时我们应该怎么用？

1，先看一下fixed线程 Executors.newFixedThreadPool(3)的内部代码实现。一般比较常用的也是：fixed线程(任务很多很多会内存溢出)，
其中参数new LinkedBlockingQueue\<Runnable>() 就是设置一个无界阻塞队列的参数。

<div align='center'><img src=./images/java-基础01/java-基础01_2020-01-07-21-59-13.png width='80%'/></div><br/>

2，从上面代码可以看出，代表线程池的类是ThreadPoolExecutor。
而fixed之类的线程池也是在其基础上再封装一层，所以我们完全可以通过这个构造函数就创建自己的线程池配合其参数corePoolSize，maximumPoolSize，keepAliveTime，queue

3，我们用线程池的类ThreadPoolExecutor自定义一个线程池，来说明一下其各个参数的含义：

```

new ThreadPoolExecutor(3,Integer.MAX_VALUE0,60s,new ArrayBlockingQueue<Runnable>(200))

corePoolSize：3  // 线程池 大小为 3
maximumPoolSize：Integer.MAX_VALUE //最多可以创建的线程数，此处暂时设置为 无限个。这样设置，任务太多太多时，会内存溢出或者cpu负载过大而宕机
keepAliveTime：60s  // 当任务全部执行完后，线程池中超过corePoolSize的线程存活时间为60s后，再自动销毁。
new ArrayBlockingQueue<Runnable>(200) // 线程中的队列可以存放200个任务

```

- 如果说你把queue做成有界队列，比如说new ArrayBlockingQueue\<Runnable>(200)，那么假设corePoolSize个线程都在繁忙的工作，大量任务进入有界队列，队列满了，此时怎么办？

> 这个时候假设你的maximumPoolSize是比corePoolSize大的，此时会继续创建额外的线程放入线程池里，来处理这些任务，然后超过corePoolSize数量的线程如果处理完了一个任务也会尝试从队列里去获取任务来执行

- 如果maximumPoolSize参数设置为 300 时 额外线程都创建完了去处理任务，队列还是满的，此时还有新的任务来怎么办？

> 只能reject掉，他有几种reject策略，可以传入RejectedExecutionHandler
(1)AbortPolicy   // 抛异常 将抛出 RejectedExecutionException
(2)DiscardPolicy   // 抛弃新任务
(3)DiscardOldestPolicy  // 抛弃旧任务
(4)CallerRunsPolicy  
(5)自定义  // 例如将任务写入磁盘，待线程空闲时再处理

根据上述原理去定制自己的线程池，考虑到corePoolSize的数量，队列类型，最大线程数量，拒绝策略，线程释放时间

4，最后我没看一下执行原理图：

<div align='center'><img src=./images/java-基础01/java-基础01_2020-01-07-23-07-06.png width='80%'/></div><br/>

- 面试题： 在远程服务异常的情况下，使用无界阻塞队列，是否会导致内存异常飙升？（如果在线程池中使用无界阻塞队列会发生什么问题？）
  
> 调用超时，队列变得越来越大，此时会导致内存飙升起来，而且还可能会导致你会OOM，内存溢出。

- 你知道如果线程池的队列满了之后，会发生什么事情吗？

> 由题可知，此线程池是个有界队列。
>
> 第一种情况，如果maximumPoolSize设置的是（Integer.MAX_VALUE ）无限大，则可能会出现：
> 由于可以无限制的不停的创建额外的线程出来，一台机器上，有几千个线程，甚至是几万个线程，每个线程都有自己的栈内存，占用一定的内存资源，会导致内存资源耗尽，系统也会崩溃掉，就算内存没有崩溃，也会导致你的机器的cpu load，负载，特别的高。
> 
> 第二种情况，如果maximumPoolSize设置了一个上限值。很有可能会抛异常和丢掉很多未执行的任务。
> 
> 解决第二种情况的方案可以参考这样的：
> 自定义一个reject策略，如果线程池无法执行更多的任务了，此时建议你可以把这个任务信息持久化写入磁盘里去，后台专门启动一个线程，后续等待你的线程池的工作负载降低了，他可以慢慢的从磁盘里读取之前持久化的任务，重新提交到线程池里去执行。

## 如果线上机器突然宕机，线程池的阻塞队列中的请求怎么办？

1，必然会导致线程池里的积压的任务实际上来说都是会丢失的

2，针对上面的问题我们如何解决呢？

> 如果说你要提交一个任务到线程池里去，在提交之前，麻烦你先在数据库里插入这个任务的信息，更新他的状态：未提交、已提交、已完成。提交成功之后，更新他的状态是已提交状态
系统重启，后台线程去扫描数据库里的未提交和已提交状态的任务，可以把任务的信息读取出来，重新提交到线程池里去，继续进行执行

## 谈谈你对Java内存模型的理解可以吗？

1，用下面的 2个线程来说明java内存模型的过程

代码图:
<div align='center'><img src=./images/java-基础01/java-基础01_2020-01-08-15-37-07.png width='80%'/></div><br/>

执行过程图(默认为未加锁的执行图):

<div align='center'><img src=./images/java-基础01/java-基础01_2020-01-08-15-43-26.png width='100%'/></div><br/>

2, 内存间的交互操作 read、load、use、assign、store、write 这6个指令的含义

- lock（锁定）：作用于主内存的变量，把一个变量标识为一条线程独占状态。
- unlock（解锁）：作用于主内存变量，把一个处于锁定状态的变量释放出来，释放后的变量才可以被其他线程锁定。
- read（读取）：作用于主内存变量，把一个变量值从主内存传输到线程的工作内存中，以便随后的load动作使用 <font color="red"> *工作内存（cpu级别的缓存空间）* </font>的变量，它把read操作从主内存中得到的变量值放入工作内存的变量副本中。
- use（使用）：作用于工作内存的变量，把工作内存中的一个变量值传递给执行引擎，每当虚拟机遇到一个需要使用变量的值的字节码指令时将会执行这个操作。
- assign（赋值）：作用于工作内存的变量，它把一个从执行引擎接收到的值赋值给工作内存的变量，每当虚拟机遇到一个给变量赋值的字节码指令时执行这个操作。
- store（存储）：作用于工作内存的变量，把工作内存中的一个变量的值传送到主内存中，以便随后的write的操作。
- write（写入）：作用于主内存的变量，它把store操作从工作内存中一个变量的值传送到主内存的变量中。

## 你知道Java内存模型中的原子性、有序性、可见性是什么吗？

1,也就是并发编程过程中，可能会产生的三类问题
默认是没有可见性，没有原子性，没有有序性

2，什么是可见性，原子性，有序性？并发编程时又如何去保证这三性呢？

- 可见性：可见性是一种复杂的属性，因为可见性中的错误总是会违背我们的直觉。通常，我们无法确保执行读操作的线程能适时地看到其他线程写入的值，有时甚至是根本不可能的事情。为了确保多个线程之间对内存写入操作的可见性，必须使用同步机制。
<font color="red">可见性： 是指线程之间的可见性，一个线程修改的状态对另一个线程是可见的。</font>也就是一个线程修改的结果。另一个线程马上就能看到。比如：用volatile修饰的变量，就会具有可见性。volatile修饰的变量不允许线程内部缓存和重排序，即直接修改内存。所以对其他线程是可见的。但是这里需要注意一个问题，volatile只能让被他修饰内容具有可见性，但不能保证它具有原子性。比如 volatile int a = 0；之后有一个操作 a++；这个变量a具有可见性，但是a++ 依然是一个非原子操作，也就是这个操作同样存在线程安全问题。

>在 Java 中 volatile、synchronized 和 final 实现可见性。

- 原子性：原子是世界上的最小单位，**具有不可分割性**。比如 a=0；（a非long和double类型） 这个操作是不可分割的，那么我们说这个操作时原子操作。再比如：a++； 这个操作实际是a = a + 1；是可分割的，所以他不是一个原子操作。非原子操作都会存在线程安全问题，需要我们使用同步技术（sychronized）来让它变成一个原子操作。一个操作是原子操作，那么我们称它具有原子性。java的concurrent包下提供了一些原子类，我们可以通过阅读API来了解这些原子类的用法。比如：AtomicInteger、AtomicLong、AtomicReference等。

>在 Java 中 synchronized 和在 lock、unlock 中操作保证原子性。

- 有序性：线程内的所有操作都是有序的，既程序执行的顺序按照代码的先后顺序执行。

> 重排序是对内存访问操作的一种优化，他可以在不影响单线程程序正确性的前提下进行一定的调整，进而提高程序的性能，但是对于多线程场景下，就可能产生一定的问题

多线程场景下的代码，可能会出现指令重排序编译器和指令器，有的时候为了提高代码执行效率，会将指令重排序，就是说比如下面的代码。

<div align='center'><img src=./images/java-基础01/java-基础01_2020-01-08-17-13-39.png width='80%'/></div><br/>

如果重排序之后，让flag = true先执行了，会导致线程2直接跳过while等待，执行某段代码，结果prepare()方法还没执行，资源还没准备好呢，此时就会导致代码逻辑出现异常。

>Java 语言提供了 volatile 和 synchronized 两个关键字来保证线程之间操作的有序性，volatile 是因为其本身包含“禁止指令重排序”的语义，synchronized 是由“一个变量在同一个时刻只允许一条线程对其进行 lock 操作”这条规则获得的，此规则决定了持有同一个对象锁的两个同步块只能串行执行。

## 能从Java底层角度聊聊volatile关键字的原理吗？

1,volatile关键字是用来解决可见性和有序性,可以有限的保证原子性。
当volatile修饰的变量被某一个线程修改后，则其他线程中的这个变量原来的值将会**失效**，从而迫使这些线程从新去主内存中读取加载这个变量的新值。从而保证了可见性。

2，下面举例说明：

<div align='center'><img src=./images/java-基础01/java-基础01_2020-01-09-09-53-11.png width='80%'/></div><br/>

volatile修饰的变量被一个线程修改后，其他线程中原来的值将会失效。

<div align='center'><img src=./images/java-基础01/java-基础01_2020-01-09-09-40-21.png width='80%'/></div><br/>

3，经常应用的场景：volatile修饰的变量 有线程修改其值，有线程要读取其值。
例如：在很多的开源中间件系统的源码里，大量的使用了volatile，每一个开源中间件系统，或者是大数据系统，都多线程并发，volatile

<div align='center'><img src=./images/java-基础01/java-基础01_2020-01-09-09-51-30.png width='80%'/></div><br/>

## 你知道指令重排以及happens-before原则是什么吗？

1，指令重排：编译器、指令器可能对代码重排序，乱排，但要守happens-before原则，只要符合happens-before的原则，那么就不能重排，如果不符合这些规则的话，那就可以自己排序，以提高执行效率。
> 计算机在执行程序过程：
> 源代码 -> 编译器优化的重排 -> 指令并行的重排 -> 内存系统的重排 -> 最终执行的指令

2，happens-before原则：

- **程序次序规则：** 一个线程内，按照代码顺序，书写在前面的操作先行发生于书写在后面的操作（所以<font color =red>单线程不会出现指令重排</font>）
- **锁定规则：** 一个unLock操作先行发生于后面对同一个锁的lock操作，比如说在代码里有先对一个lock.lock()，lock.unlock()，lock.lock()
- **volatile变量规则：** 对一个volatile变量的写操作先行发生于后面对这个volatile变量的读操作，volatile变量写，再是读，必须保证是先写，再读
- **传递规则：** 如果操作A先行发生于操作B，而操作B又先行发生于操作C，则可以得出操作A先行发生于操作C
- **线程启动规则：** Thread对象的start()方法先行发生于此线程的每个一个动作，thread.start()，thread.interrupt()
- **线程中断规则：** 对线程interrupt()方法的调用先行发生于被中断线程的代码检测到中断事件的发生
- **线程终结规则：** 线程中所有的操作都先行发生于线程的终止检测，我们可以通过Thread.join()方法结束、Thread.isAlive()的返回值手段检测到线程已经终止执行
- **对象终结规则：** 一个对象的初始化完成先行发生于他的finalize()方法的开始

> 上面这8条原则的意思很显而易见，就是程序中的代码如果满足这个条件，就一定会按照这个规则来保证指令的顺序，即不允许编译器、指令器对你写的代码进行指令重排，必须保证你的代码的有序性。但是如果没满足上面的规则，那么就可能会出现指令重排，就这个意思。

3，volatile三个特性，保证可见性，不保证原子性，禁止指令重排。

- 比如这个例子，如果用volatile来修饰flag变量，一定可以让prepare()指令在flag = true之前先执行，这就禁止了指令重排。

<div align='center'><img src=./images/java-基础01/java-基础01_2020-01-09-10-44-08.png width='60%'/></div><br/>

>因为volatile要求的是，volatile前面的代码一定不能指令重排到volatile变量操作的后面，volatile后面的代码也不能指令重排到volatile变量操作的前面。

## volatile底层是如何基于内存屏障保证可见性和有序性的？

1，volatile保证可见性和有序性，不保证原子性。

2，volatile底层原理，如何实现保证可见性的呢？

> 通过 lock前缀指令 + MESI缓存一致性协议
> - lock指令：volatile保证可见性 
对volatile修饰的变量，执行写操作的话，JVM会发送一条lock前缀指令给CPU，CPU在计算完之后会立即将这个值写回主内存，同时因为有MESI缓存一致性协议，所以各个CPU都会对总线进行嗅探，自己本地缓存中的数据是否被别人修改
> - 如果发现别人修改了某个缓存的数据，那么CPU就会将自己本地缓存的数据过期掉(失效)，然后这个CPU上执行的线程在读取那个变量的时候，就会从主内存重新加载最新的数据了

3, volatile底层原理，如何实现保证有序性的呢？

> 通过插入内存屏障又叫内存栅栏(是一个CPU指令)，就能禁止在内存屏障前后的指令执行重排优化。内存屏障另外一个作用就是强制刷出各种CPU的缓存数据。
> 内存屏障有：LoadLoad屏障, StoreStore屏障, LoadStore屏障, StoreLoad屏障等等。

## 说说你对Spring的IOC机制和AOP机制的理解可以吗？

1,Spring IOC框架，控制反转，依赖注入。

<div align='center'><img src=./images/java-基础01/java-基础01_2020-01-09-15-45-54.png width='100%'/></div><br/>

- spring ioc的实现过程

> 我们只要在这个工程里通过maven引入一些spring框架的依赖，ioc功能
> tomcat在启动的时候，直接会启动spring容器
> spring ioc，spring容器，根据xml配置，或者是你的注解，去实例化你的一些bean对象，然后根据xml配置或者注解，去对bean对象之间的引用关系，去进行依赖注入，某个bean依赖了另外一个bean。

- spring ioc底层实现的核心技术是
  
> 反射，他会通过反射的技术，直接根据你的类去自己构建对应的对象出来，用的就是反射技术

- spring ioc 主要解决了什么问题

> 系统的类与类之间彻底的解耦合

2, 从没有 IOC 的代码实现 和 有 IOC 代码实现的 对比来看ioc的意义。

- 无ioc时的代码
写一套系统，web服务器，tomcat，一旦启动之后，他就可以监听一个端口号的http请求，然后可以把请求转交给你的servlet，jsp，配合起来使用的，servlet处理请求

<div align='center'><img src=./images/java-基础01/java-基础01_2020-01-09-16-04-22.png width='80%'/></div><br/>

> 在我们的一个tomcat+servlet的这样的一个很low的系统里，有几十个地方，都是直接用MyService myService = new MyServiceImpl()，直接创建、引用和依赖了一个MyServiceImpl这样的一个类的对象。这就有几十个地方，都跟MyServiceImpl类直接耦合在一起了。

> 我现在不想要用MyServiceImpl了，我们希望用的是NewServiceManagerImpl，implements MyService这个接口的，所有的实现逻辑都不同了，此时我们很麻烦，我们需要在很low的系统里，几十个地方，都去修改对应的MyServiceImpl这个类，切换为NewServiceManagerImpl这个类

> 这样写的代码的缺点是 ：
改动代码成本很大，改动完以后的测试的成本很大，改动的过程中可能很复杂，出现一些bug，此时就会很痛苦，归根结底，代码里，各种类之间完全耦合在一起，出现任何一丁点的变动，都需要改动大量的代码，重新测试，可能还会有bug

- 有ioc 的代码，xml文件来进行一个配置，进化到了基于注解来进行自动依赖注入。（实现类与类之间的解耦）
  
  <div align='center'><img src=./images/java-基础01/java-基础01_2020-01-09-16-11-50.png width='80%'/></div><br/>

> 现在这套比较高大上的一点系统里，有几十个类都使用了@Resource这个注解去标注MyService myService，几十个地方都依赖了这个类，如果要修改实现类MyServiceImpl为NewServiceManagerImpl时，只有将 @Service注解改到NewServiceManagerImpl类上即可。

## 说说你对Spring的AOP机制的理解可以吗？

1，AOP（Aspect-OrientedProgramming，面向方面编程），可以说是OOP（Object-Oriented Programing，面向对象编程）的补充和完善。
用于处理系统中分布于各个模块的横切关注点，比如事务管理、日志、缓存等等。AOP实现的关键在于AOP框架自动创建的AOP代理，AOP代理主要分为静态代理和动态代理，静态代理的代表为AspectJ；而动态代理则以Spring AOP为代表。静态代理是编译期实现，动态代理是运行期实现，可想而知前者拥有更好的性能。

2，实现AOP的技术
主要分为两大类：一是采用**动态代理技术**，利用截取消息的方式，对该消息进行装饰，以取代原有对象行为的执行；二是采用**静态织入**的方式，引入特定的语法创建“方面”，从而使得编译器可以在编译期间织入有关“方面”的代码。

3，AOP用来封装横切关注点，具体可以在下面的场景中使用:

Authentication 权限,
Caching 缓存,
Context passing 内容传递,
Error handling 错误处理,
Lazy loading　懒加载,
Debugging　　调试,
logging, tracing, profiling and monitoring　记录跟踪　优化　校准,
Performance optimization　性能优化,
Persistence　　持久化,
Resource pooling　资源池,
Synchronization　同步,
Transactions 事务,

4，AOP 的作用

> AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。

5, AOP中相关的概念定义

- Aspect（切面）： Aspect 声明类似于 Java 中的类声明，在 Aspect 中会包含着一些 Pointcut 以及相应的 Advice。
- Joint point（连接点）：表示在程序中明确定义的点，典型的包括方法调用，对类成员的访问以及异常处理程序块的执行等等，它自身还可以嵌套其它 joint point。
- Pointcut（切点）：表示一组 joint point，这些 joint point 或是通过逻辑关系组合起来，或是通过通配、正则表达式等方式集中起来，它定义了相应的 Advice 将要发生的地方。
- Advice（增强）：Advice 定义了在 Pointcut 里面定义的程序点具体要做的操作，它通过 before、after 和 around 来区别是在每个 joint point 之前、之后还是代替执行的代码。
- Target（目标对象）：织入 Advice 的目标对象.。
- Weaving（织入）：将 Aspect 和其他对象连接起来, 并创建 Adviced object 的过程

这些概念之间关系的图：

<div align='center'><img src=./images/java-基础01/java-基础01_2020-01-09-17-08-00.png width='100%'/></div><br/>

6，Advice 的类型

- before advice, 在 join point 前被执行的 advice. 虽然 before advice 是在 join point 前被执行, 但是它并不能够阻止 join point 的执行, 除非发生了异常(即我们在 before advice 代码中, 不能人为地决定是否继续执行 join point 中的代码)
- after return advice, 在一个 join point 正常返回后执行的 advice
- after throwing advice, 当一个 join point 抛出异常后执行的 advice
- after(final) advice, 无论一个 join point 是正常退出还是发生了异常, 都会被执行的 advice.
- around advice, 在 join point 前和 joint point 退出后都执行的 advice. 这个是最常用的 advice.
- introduction，introduction可以为原有的对象增加新的属性和方法。

7，Spring AOP的两种代理实现机制**JDK动态代理**和**CGLIB动态代理**

- 静态代理是编译阶段生成AOP代理类，也就是说生成的字节码就织入了增强后的AOP对象；动态代理则不会修改字节码，而是在内存中临时生成一个AOP对象，这个AOP对象包含了目标对象的全部方法，并且在特定的切点做了增强处理，并回调原对象的方法。

- Spring AOP中的动态代理主要有两种方式，JDK动态代理和CGLIB动态代理。JDK动态代理通过反射来接收被代理的类，并且要求被代理的类必须实现一个接口。JDK动态代理的核心是InvocationHandler接口和Proxy类。

- 如果目标类没有实现接口，那么Spring AOP会选择使用CGLIB来动态代理目标类。CGLIB（Code Generation Library），是一个代码生成的类库，可以在运行时动态的生成某个类的子类，注意，CGLIB是通过继承的方式做的动态代理，因此如果某个类被标记为final，那么它是无法使用CGLIB做动态代理的，诸如private的方法也是不可以作为切面的。

8，Spring AOP中的JDK动态代理的举例。AOP的核心技术，就是动态代理

<div align='center'><img src=./images/java-基础01/java-基础01_2020-01-09-17-30-58.png width='80%'/></div><br/>

<div align='center'><img src=./images/java-基础01/java-基础01_2020-01-09-17-31-43.png width='80%'/></div><br/>

## 了解过cglib动态代理吗？他跟jdk动态代理的区别是什么？

1，spring里使用aop，比如说你对一批类和他们的方法做了一个切面，定义好了要在这些类的方法里增强的代码，spring必然要对那些类生成动态代理，在动态代理中去执行你定义的一些增强代码。

2，其实就是动态的创建一个代理类出来，创建这个代理类的实例对象，在这个里面引用你真正自己写的类，所有的方法的调用，都是先走代理类的对象，他负责做一些代码上的增强，再去调用你写的那个类。优先是jdk动态代理，其次是cglib动态代理

3，如果你的类是实现了某个接口的，spring aop会使用jdk动态代理，生成一个跟你实现同样接口的一个代理类，构造一个实例对象出来，jdk动态代理，他其实是在你的类有接口的时候，就会来使用。

4，很多时候我们可能某个类是没有实现接口的，spring aop会改用cglib来生成动态代理，他是生成你的类的一个子类，他可以动态生成字节码，覆盖你的一些方法，在方法里加入增强的代码

5，代码示例链接

- [JDK动态代理代码示例](https://www.cnblogs.com/muscleape/p/9018302.html)
- [cglib动态代理代码示例](https://www.cnblogs.com/muscleape/p/9018308.html)

## 能说说Spring中的Bean是线程安全的吗？

1,答案是否定的，绝对不可能是线程安全的，spring bean默认来说，singleton，都是线程不安全的，java web系统，一般来说很少在spring bean里放一些实例变量，一般来说他们都是多个组件互相调用，最终去访问数据库的

2,Spring容器中的bean可以分为5个范围：
 
- singleton：默认，每个容器中只有一个bean的实例

- prototype：为每一个bean请求提供一个实例
 
一般来说下面几种作用域，在开发的时候一般都不会用，99.99%的时候都是用singleton单例作用域

- request：为每一个网络请求创建一个实例，在请求完成以后，bean会失效并被垃圾回收器回收
- session：与request范围类似，确保每个session中有一个bean的实例，在session过期后，bean会随之失效
- global-session

图解

<div align='center'><img src=./images/java-基础01/java-基础01_2020-01-09-19-35-50.png width='100%'/></div><br/>

上图解对应的代码：（不能这样写）

<div align='center'><img src=./images/java-基础01/java-基础01_2020-01-09-19-40-38.png width='100%'/></div><br/>

一般我们都是这样写的，最后多线程访问数据库。

<div align='center'><img src=./images/java-基础01/java-基础01_2020-01-09-19-17-46.png width='80%'/></div><br/>

## Spring的事务实现原理是什么？能聊聊你对事务传播机制的理解吗？
