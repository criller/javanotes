- [Java反射机制详解](#java反射机制详解)
  - [1.反射基础](#1反射基础)
    - [Class类](#class类)
    - [类加载流程](#类加载流程)
  - [2.反射的使用](#2反射的使用)
    - [2.1 反射机制相关类](#21-反射机制相关类)
    - [2.2 Class对象的获取](#22-class对象的获取)
    - [2.3 获取Class对象的摘要信息](#23-获取class对象的摘要信息)
    - [2.4 获取类的属性信息](#24-获取类的属性信息)
    - [2.5 获取类的方法信息](#25-获取类的方法信息)
    - [2.6  对象实例创建的几种方式](#26--对象实例创建的几种方式)
    - [2.7 获取对象的属性值](#27-获取对象的属性值)
    - [2.8 动态调用对象的方法](#28-动态调用对象的方法)
  - [3.反射机制的执行流程](#3反射机制的执行流程)
  - [4.反射机制的使用实战](#4反射机制的使用实战)
  - [5.参考文章](#5参考文章)
# Java反射机制详解

> JAVA反射机制是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意一个方法和属性；这种动态获取的信息以及动态调用对象的方法的功能称为Java语言的反射机制。Java反射机制在框架设计中极为广泛，需要深入理解。

## 1.反射基础

RTTI（Run-Time Type Identification）运行时类型识别。在《Thinking in Java》一书第十四章中有提到，其作用是在运行时识别一个对象的类型和类的信息。主要有两种方式：一种是“传统的”RTTI，它假定我们在编译时已经知道了所有的类型；另一种是“反射”机制，它允许我们在运行时发现和使用类的信息。

![image-20210927191700934](assets/image-20210927191700934.png)

反射就是把Java类中的各种成分映射成一个个的Java对象

例如：一个类有：成员变量、方法、构造方法、包等等信息，利用反射技术可以对一个类进行解剖，把个个组成部分映射成一个个对象。

> 所以，我们首先需要理解Class类以及类的加载机制；然后基于此我们如何通过反射获取Class类以及类中的成员变量、成员方法和构造方法等。

### Class类

源代码通过编译器编译为字节码，再通过类加载子系统加载到JVM中，生成对应的Class对象。所有的数据类型，包括基本数据类型和关键字void同样表现为Class对象。这也说明了在Java中一切皆为对象。

<img src="assets/image-20210927193856296.png" alt="image-20210927193856296" style="zoom:150%;" />

所以，从中我们可以得到一下几点信息：

- class字节码加载到JVM中，会生成对应的Class对象
- Class类也是类的一种，与Class关键字是不一样的。
- 手动编写的类被编译后会产生一个Class对象，其表示的是创建类的类型信息
- 每个通过关键字class标识的类，在内存中有且只有一个与之对应的Class对象来描述其类型信息，无论创建多少个实例对象，其依据的都是用的同一个Class对象。（这里说明每个类的Class对象也是单例的）
- Class类只存私有构造函数，因此对应Class对象只能有JVM创建和加载
- Class类的对象作用是允许时提供或获取某个对象的类型信息，这点对于反射技术很重要。

### 类加载流程

回顾一下类加载流程，源代码经过编译成class字节码后，类加载子系统通过 **加载 -> 链接 -> 初始化 -> 使用 -> 卸载**

![image-20210927194408670](assets/image-20210927194408670.png)

对上图类加载流程和内存结构的简化版

![image-20210927194620622](assets/image-20210927194620622.png)

## 2.反射的使用

![反射类图](assets/image-20210927191847619.png)

> 回顾一下反射机制是什么？在运行状态中，对任意一个类，可以获取它的类信息、属性信息和方法信息；对任意一个对象，能够调用它的方法和属性

那该章节有以下几个问题

- 怎么获取类对象？
- 怎么获取类相关信息？
- 怎么获取类的属性信息？
- 怎么获取对象的属性值？
- 怎么获取类的方法信息？
- 怎么动态调用对象的方法？

### 2.1 反射机制相关类

```java
java.lang.Class; //类               
java.lang.reflect.Constructor;//构造方法 
java.lang.reflect.Field; //类的成员变量       
java.lang.reflect.Method;//类的方法
java.lang.reflect.Modifier;//访问权限
```

### 2.2 Class对象的获取

获取class对象的三种方式

- 根据类名：类名.class
- 根据对象：对象.getClass()
- 根据全限定类名：Class.forName(全限定类名)

```java
	/**
     * 示例：创建Class对象的3种方式
     */
    @Test
    public void test() throws Throwable{
        // 方式一  类.class
        Class personClazz = Person.class;

        // 方式二  实例.getClass()
        Person person = new Person();
        Class personClazz1 = person.getClass();

        // 方式三  Class.forName("类的全路径")
        Class personClazz2 = Class.forName("com.muse.reflect.Person");

        System.out.println(personClazz == personClazz1);

        System.out.println(personClazz == personClazz2);
    }
```

### 2.3 获取Class对象的摘要信息

获取class摘要信息的常用方法

| 方法名            | 描述                                                         |
| ----------------- | ------------------------------------------------------------ |
| forName()         | 加载Class对象                                                |
| getName()         | 取全限定的类名(包括包名)，即类的完整名字。                   |
| getSimpleName()   | 获取类名(不包括包名)                                         |
| isInterface()     | 判断是否是一个接口                                           |
| isArray           | 是否是集合类                                                 |
| isAnnotation      | 是否是注解类                                                 |
| getModifiers      | 获取class访问权限                                            |
| getInterfaces()   | 返回Class对象数组，表示Class对象所引用的类所实现的所有接口。 |
| getSupercalss()   | 返回Class对象，表示Class对象所引用的类所继承的直接基类。应用该方法可在运行时发现一个对象完整的继承结构。 |
| newInstance()     | 返回一个Oject对象，是实现“虚拟构造器”的一种途径。使用该方法创建的类，必须带有无参的构造器。 |
| getFields()       | 获得某个类的所有的公共（public）的字段，包括继承自父类的所有公共字段。 类似的还有getMethods和getConstructors。 |
| getDeclaredFields | 获取某个类的自己声明的字段，包括public、private和protected，默认但是不包括父类声明的任何字段。 |

### 2.4 获取类的属性信息

### 2.5 获取类的方法信息

### 2.6  对象实例创建的几种方式

### 2.7 获取对象的属性值

### 2.8 动态调用对象的方法

```java
 public void createInstance() throws Exception {
        // 首先，获取class对象，有三种调用方式，这里随机选取一种
        Class<Person> personClass = Person.class;
        // 第一种方式， 该方式只能调用无参构造器
        Person person1 = personClass.newInstance();
        // 第二种方式， 通过class对象，创建无参构造器
        Constructor<Person> constructor = personClass.getConstructor();
        Person person2 = constructor.newInstance();
        // 第三种方式， 通过class对象，创建有参构造器
        Constructor<Person> constructor2 = personClass.getConstructor(Integer.class);
        Person person3 = constructor2.newInstance(10);
}
```



某种情况下，反射通过私有构造方法创建对象，破坏单例模式。

```java
public class SingletonPerson {

    private static final SingletonPerson instance = new SingletonPerson();

    private SingletonPerson(){
    }
    public static SingletonPerson getInstance(){
        return instance;
    }
}
```

```java
    @Test
    public void test2() throws Throwable {

        /** 补充内容：反射通过私有构造方法创建对象，破坏单例模式 */
        Class singletonPersonClazz = SingletonPerson.class;
        // Constructor constructor3 = singletonPersonClazz.getConstructor();
        Constructor constructor3 = singletonPersonClazz.getDeclaredConstructor();
        constructor3.setAccessible(true);
        SingletonPerson singletonPerson = (SingletonPerson) constructor3.newInstance();
        SingletonPerson singletonPerson1 = SingletonPerson.getInstance();
        SingletonPerson singletonPerson2 = SingletonPerson.getInstance();
        System.out.println("singletonPerson==singletonPerson1 is " + (singletonPerson == singletonPerson1));
        System.out.println("singletonPerson==singletonPerson2 is " + (singletonPerson == singletonPerson2));
        System.out.println("singletonPerson1==singletonPerson2 is " + (singletonPerson1 == singletonPerson2));
    }
```



## 3.反射机制的执行流程

## 4.反射机制的使用实战

## 5.参考文章

- https://pdai.tech/md/java/basic/java-basic-x-reflection.html#%E5%8F%8D%E5%B0%84%E5%9F%BA%E7%A1%80
- https://www.cnblogs.com/whoislcj/p/6038511.html

