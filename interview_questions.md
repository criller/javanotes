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