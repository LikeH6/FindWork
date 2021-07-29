# 什么是面向对象？对其理解



==面向过程==是将任务分解成一系列的步骤，每个步骤都是一个函数，更注重每个一步骤和顺序

==面向对象==更注重事情的参与者（对象），以及各自做什么

对比：面向过程比较直接高效，面向对象更易于复用、扩展和维护

---

**三大特性**：

1. **封装**：封装的意义在于明确标识出允许外部使用的所有成员函数和数据项

   内部细节对外部调用透明，外部调用无需修改或关心内部实现

   两个实现：

   + JavaBean的属性私有，提供get set方法访问。
   + orm框架，不需要关心连接怎么建立、sql如何执行，直接使用就好了

2. **继承**：继承基类的方法，并作出自己的改变或扩展

   子类共性的方法或者属性直接使用父类的，而不需要自己再定义

3. **多态**：基于对象所属类的不同，外部对同一个方法的调用，实际执行的逻辑不同

   多态的三个条件：继承、方法重写、父类引用指向子类对象

   ```shell
   父类类型 变量名 = new 子类对象  # 扩展子类就行了
   变量名.方法名()  # 调用的是子类的方法
   ```

   但是父类无法调用子类的特有的功能，必须在父类中有才能调用，即父类需要重写。

# JDK、JRE、JVM的联系和区别

> JDK：Java development Kit    Java开发工具
>
> JRE：Java Runtime Environment   Java运行时环境
>
> JVM：Java Virtual Machine    Java虚拟机

![image-20210514142301870](.\image\image-20210514142301870.png)

# ==和equals

== 对比的是栈中的值，基本数据类型是变量值，引用类型是堆中内存对象的地址

equals object中默认也是采用==比较，通常会重写。String类中被重写的equals()方法其实是比较两个字符串的内容

![image-20210514142859143](.\image\image-20210514142859143.png)

# 集合

![image-20210609154942164](.\image\image-20210609154942164.png)

![image-20210609155254103](.\image\image-20210609155254103.png)

# 引用类型----传值

![image-20210615100200740](.\image\image-20210615100200740.png)

```java
// 上述代码结果为 
// 注意划分作用域，即所属的方法
20   //  基本类型传复印件，在方法中修改的是复印件中的age，并不是main方法中的age
xxx   //  引用类型传同样的内存地址，因此会修改对象属性的值
abc   // 虽然String也是引用类型，但是由于字符串常量池，此时常量池中有abc对应的内存地址，但是没有xxx，因此会新建一个内存地址指向xxx，因此main方法中的str还是abc
```

下图左边是对应Person，右边是对应String

![image-20210615100832103](.\image\image-20210615100832103.png)

# 集合

一：集合概念

对象的容器，实现了对对象常用的操作，类似数组功能

二：集合和数组的区别

（1）数组长度是固定的，集合的长度不固定

（2）数据可以存储基本类型和引用类型，集合只能存储引用类型

三：Collection体系集合

![image-20210702194145154](.\image\image-20210702194145154.png)

# Set

特点：无序、无下标、元素不可以重复

方法：全部继承自Collection中的方法

两种实现类：

==HashSet（重点）==：基于HashCode实现元素不重复；当存入元素的哈希码相同时，会调用equals进行确认，如结果为true，则拒绝后者存入

存储结构：哈希表（数组+链表+红黑树）

存储过程（重复依据）

1. 根据hashCode计算保存的位置，如果位置为空，直接保存，若不为空，进行第二步
2. 再执行equals方法，如果equals为true，则认为是重复，否则形成链表

```java
Set<String> set = new HashSet<>();
```

==TreeSet==：基于排列顺序实现元素不重复

特点：

- 基于排列顺序实现元素不重复
- 实现SortedSet接口，对集合元素自动排序
- 元素对象的类型必须实现Comparable接口，指定排序规则
- 通过CompareTo方法确定是否为重复元素

存储结构：红黑树

```java
Set<String> set = new TreeSet<>();
```

# HashMap

存储结构：哈希表（数组+链表+红黑树）

使用key可使hashcode和equals作为重复

HashMap存数据的过程是：

   HashMap内部维护了一个存储数据的Entry数组，HashMap采用链表解决冲突，每一个Entry本质上是一个单向链表。当准备添加一个key-value对时，首先通过hash(key)方法计算hash值，然后通过indexFor(hash,length)求该key-value对的存储位置，计算方法是先用hash&0x7FFFFFFF后，再对length取模，这就保证每一个key-value对都能存入HashMap中，当计算出的位置相同时，由于存入位置是一个链表，则把这个key-value对插入链表头。

   HashMap中key和value都允许为null。key为null的键值对永远都放在以table[0]为头结点的链表中。

原码分析总结：

1. HashMap刚创建时，table是null，节省空间，当添加第一个元素时，table容量调整为16
2. 当元素个数大于阈值（16*0.75 = 12）时，会进行扩容，扩容后的大小为原来的两倍，目的是减少调整元素的个数
3. jdk1.8 当每个链表长度 >8 ，并且数组元素个数 ≥64时，会调整成红黑树，目的是提高效率
4. jdk1.8 当链表长度 <6 时 调整成链表
5. jdk1.8 以前，链表时头插入，之后为尾插入

# HashMap和HashTable的区别

https://www.cnblogs.com/williamjie/p/9099141.html

1、继承的父类不同

Hashtable继承自Dictionary类，而HashMap继承自AbstractMap类。但二者都实现了Map接口。

2、线程安全性不同

HashMap是线程不安全的，HashTable是线程安全的。

Hashtable 线程安全很好理解，因为它每个方法中都加入了Synchronize。

HashMap底层是一个Entry数组，当发生hash冲突的时候，hashmap是采用链表的方式来解决的，在对应的数组位置存放链表的头结点。对链表而言，新加入的节点会从头结点加入。

Hashtable 中的方法是Synchronize的，而HashMap中的方法在缺省情况下是非Synchronize的。在多线程并发的环境下，可以直接使用Hashtable，不需要自己为它的方法实现同步，但使用HashMap时就必须要自己增加同步处理。

3、是否提供contains方法

HashMap把Hashtable的contains方法去掉了，改成containsValue和containsKey，因为contains方法容易让人引起误解。

Hashtable则保留了contains，containsValue和containsKey三个方法，其中contains和containsValue功能相同。

4、key和value是否允许null值

Hashtable中，key和value都不允许出现null值。但是如果在Hashtable中有类似put(null,null)的操作，编译同样可以通过，因为key和value都是Object类型，但运行时会抛出NullPointerException异常，这是JDK的规范规定的。
HashMap中，null可以作为键，这样的键只有一个；可以有一个或多个键所对应的值为null。当get()方法返回null值时，可能是 HashMap中没有该键，也可能使该键所对应的值为null。因此，在HashMap中不能由get()方法来判断HashMap中是否存在某个键， 而应该用containsKey()方法来判断。

5、两个遍历方式的内部实现上不同

 Hashtable、HashMap都使用了 Iterator。而由于历史原因，Hashtable还使用了Enumeration的方式 。

6、hash值不同

哈希值的使用不同，HashTable直接使用对象的hashCode。而HashMap重新计算hash值。

hashCode是jdk根据对象的地址或者字符串或者数字算出来的int类型的数值。

Hashtable计算hash值，直接用key的hashCode()，而HashMap重新计算了key的hash值，Hashtable在求hash值对应的位置索引时，用取模运算，而HashMap在求位置索引时，则用与运算，且这里一般先用hash&0x7FFFFFFF后，再对length取模，&0x7FFFFFFF的目的是为了将负的hash值转化为正值，因为hash值有可能为负数，而&0x7FFFFFFF后，只有符号外改变，而后面的位都不变。

7、内部实现使用的数组初始化和扩容方式不同

HashTable在不指定容量的情况下的默认容量为11，而HashMap为16，Hashtable不要求底层数组的容量一定要为2的整数次幂，而HashMap则要求一定为2的整数次幂。
Hashtable扩容时，将容量变为原来的2倍加1，而HashMap扩容时，将容量变为原来的2倍。

Hashtable和HashMap它们两个内部实现方式的数组的初始大小和扩容的方式。HashTable中hash数组默认大小是11，增加的方式是 old*2+1。

# 多线程

## 进程和线程

![image-20210715100833654](.\image\image-20210715100833654.png)

![image-20210715101707904](.\image\image-20210715101707904.png)

![image-20210715102114911](.\image\image-20210715102114911.png)

## 创建线程的方法

==**第一种方法：**==将类声明为Thread的子类，这个子类应该重写run类的方法Thread，然后可以分配并启动子类的实例

步骤：

- 自定义线程类继承<font color="red">Thread类</font>
- 重写<font color="red">run()</font>方法，编写线程执行体
- 创建线程对象，调用<font color="red">star t()</font>方法启动线程

总结：注意，线程开启不一定立即执行，由CPU调度执行（Thread内部也是继承了Runnable接口）

![image-20210715143537640](.\image\image-20210715143537640.png)

==**第二种方法：**==（推荐使用这种方法）声明实现类Runnable接口，那个类实现了run方法，然后可以分配类的实例，在创建Thread时作为参数传递，并启动

步骤：

- 定义MyRunnable类实现<font color="red">Runnable</font>接口
- <font color="red">实现run()</font>方法，编写线程执行体
- 创建线程对象，调用<font color="red">start()</font>方法启动线程

![image-20210715143714804](.\image\image-20210715143714804.png)

**两种方法对比：**

![image-20210715144154611](.\image\image-20210715144154611.png)

==**第三种方法：**==实现Callable接口（了解即可）

步骤：

![image-20210715145951311](.\image\image-20210715145951311.png)

好处：可以定义返回值；可以抛出异常。

## 线程五大状态

![image-20210715154622994](.\image\image-20210715154622994.png)



![image-20210715154655172](.\image\image-20210715154655172.png)

![image-20210715154904975](.\image\image-20210715154904975.png)

![image-20210715155203115](.\image\image-20210715155203115.png)

![image-20210715155642153](.\image\image-20210715155642153.png)

![image-20210715160408218](.\image\image-20210715160408218.png)

![image-20210715160747074](.\image\image-20210715160747074.png)

![image-20210715160834528](.\image\image-20210715160834528.png)

## 线程同步

多个线程操作一个资源

并发：同一个对象被多个线程同时操作

![image-20210715163511139](.\image\image-20210715163511139.png)

![image-20210715163707349](.\image\image-20210715163707349.png)

![image-20210715164749865](.\image\image-20210715164749865.png)

![image-20210715164937832](.\image\image-20210715164937832.png)

![image-20210715182212118](.\image\image-20210715182212118.png)

![image-20210715182540182](.\image\image-20210715182540182.png)

![image-20210715182604048](.\image\image-20210715182604048.png)

## 死锁

![image-20210715170728004](.\image\image-20210715170728004.png)

![image-20210715171247307](.\image\image-20210715171247307.png)

## 线程池

![image-20210715183618972](.\image\image-20210715183618972.png)

![ ](.\image\image-20210715183740499.png)

![](.\image\image-20210715183908052.png)

# Lamda表达式

为什么要是用lamda表达式呢？

1、避免匿名内部类定义过多

2、可以让你的代码看起来很简洁

3、去掉了一堆没有意义的代码，只留下核心的逻辑

![image-20210715153548230](.\image\image-20210715153548230.png)

![image-20210715154138885](.\image\image-20210715154138885.png)

![image-20210715154155848](.\image\image-20210715154155848.png)

![image-20210715154224560](.\image\image-20210715154224560.png)

![image-20210715154246676](.\image\image-20210715154246676.png)

