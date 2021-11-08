# 1. Java基础

## 1.1 Java基础语法特性

### 1 ) == 和equals的区别是什么？

“==” 对于基本数据类型，就是比较值是否相等，对于引用类型，对比两个对象的引用是否完全相同。

而equals 默认情况下是引用比较，但是可以重写equals方法，比如String、Integer等类把它变成了值比较。

### 2)  两个对象的hashCode()相同，则equals() 也一定为true, 对吗?

不对，不同的对象的引用地址，在散列表中，通过哈希算法可能会落到同一位置上，但是值并不一定相等。

### 3）String、StringBuffer、StringBuilder的区别

String 和 StringBuffffer、StringBuilder 的区别在于 String 声明的是不可变的对象， 每次操作都会⽣成 

新的 String 对象， 然后将指针指向新的 String 对象， ⽽StringBuffffer、StringBuilder 可以在原有对 

象的基础上进⾏操作，所以在经常改变字符串内容的情况下最好不要使⽤ String。 

StringBuffffer 和 StringBuilder 最⼤的区别在于，StringBuffffer 是线程安全的， 

⽽ StringBuilder 是 ⾮ 线 程 安 全 的 ， 但 StringBuilder 的 性 能 却 ⾼ 于StringBuffffer，所以在单线 

程环境下推荐使⽤StringBuilder，多线程环境下推荐使⽤ StringBuffffer

### 4）i++和++i的区别

> 考察点：1. 使用方式上，值的区别；2.从JVM的层面解释底层原理

## 1.2 关键字

### 1）final在Java中的作用

final修饰的类叫最终类，不能为继承

final修饰的方法不能为重写

final修饰的属性变量叫常量，常量必须初始化，初始化之后值就不能被修改。

回答这个问题的时候，可以深入说下底层实现原理。涉及到类加载、对象实例化过程

## 1.3 面向对象

## 1.4 异常分类处理

## 1.5 Java反射

## 1.6 Java注解

## 1.7 Java泛型

## 1.8 Java序列化

## 1.9 Java复制

## 1.10 动态代理

# 2. Java进阶

## 2.1 集合

# 3. kafka

## 3.1 kafka中的ISR和AR分别代表什么？

ISR：与leader保持同步的follower集合

  AR：分区的所有副本

# 4.MYSQL

## 4.1 Mysql是如何保证事务的ACID的？

答：ACID就是事务的特性。既原子性（A） 一致性（C） 隔离性（I） 持久性（D）
其中undolog保证原子性，undo就是回滚日志，会记录数据修改之前的逻辑日志，一旦发生异常，就可以用undoLog来进行数据回滚
事务的隔离级别来保证隔离性 ，隔离级别有RU、RC、RR、串行化，其中RU存在脏读、不可重复读、幻读问题、RC解决了脏读问题。
在InnoDB中、RR解决了脏读、不可重复读、幻读问题、串行化不存在并发，也就没有脏读、不可重复读、幻读问题。
其中innoDB中RR解决幻读是通过MVCC或者LBCC去解决，MVCC在读场景通过快照版本去解决幻读，LBCC通过加间隙锁去解决幻读
Mysql的持久性是通过RedoLog和double write来保证数据写了就一定要做到，redolog就是恢复日志，为了性能，在内存也会有个redologbuffer内存区间，然后再跟磁盘交互，所以，redolog也会存在数据丢失的场景，如果要保证不丢失，必须要保证redologbuffer里的数据写到磁盘，才commit成功！！
一致性 就是我的数据要完整不被破坏 mysql中的AID，都是为了保证数据的一致性。

# 5.JVM

## 5.1 平时有JVM调优的经验吗？

答：有过，这里简单的记住几个常见的例子，调优是没有一个完全正确的答案的，这里面试官考的是三点：第一点是看你是否真的对JVM有研究过，而不是单纯是背出来的；第二点是看你的解决问题的思路是否清晰，只要你能分析出一两个问题就很好了；第三点就是看你之前的工作经验了。
JVM调优我给一个大体的步骤，但是不同的问题也要具体看待：
1）分析GC日志，通过printGCDetails参数打印出GC的日志，然后通过工具查看日志中的吞吐量，GC的次数是否过多，平均停顿时间、最大停顿时间是否过长等等；通过GC日志要初步定位问题，比如说如果GC次数过多是否是因为堆内存确实过小、是否存在大对象不能被回收、是否存在死循环、是否因为停顿时间设置过小而导致等等；
2）通过上一步分析出问题可能的原因，然后再去查看dump文件中的堆内存的使用详情，通过这一步再一次定位问题；如果是跟CPU有关的问题就要通过jstack查看栈信息。
3）根据上面两步定位到的问题，然后尝试调整相关参数，再观察调整之后的结果
4）重复第三步

# 综合

## CPU使用率过高应该如何排查？

答：1.使用top 定位到占用CPU高的进程PIDtop 指令通过ps aux | grep PID命令
      2.获取线程信息，并找到占用CPU高的线程ps -mp 进程ID -o THREAD,tid,time | sort -rn 
      3.将需要的线程ID转换为16进制格式printf "%x\n" tid
      4.打印线程的堆栈信息jstack pid |grep tid -A 30

