# 一、Lambda表达式

> 主要从以下几个方面梳理：
>
> 1.  需求分析，Lambda的使用背景
> 2. Lambda的语法规则
> 3. 了解@FunctionalInterface注解
> 4. Lambda表达的实现原理
> 5. Lambda的省略写法
> 6. Lambda和匿名内部类的对比

- Lambda表达式： 简化内部类的使用
- Xjad反编译工具



匿名内部类在编译的时候会产生一个class文件

Lambda表达式在程序运行的时候会形成一个类

	1. 在类中新增一个方法，这个方法的方法体就是Lambda表达式中的代码
 	2. 还会形成一个匿名内部类，实现接口，重写抽象方法
 	3. 在接口中重写方法会调用新生成的方法。

Lambda表达式的省略写法

- 小括号内的参数类型可以省略
- 如果小括号内有且仅有一个参数，则小括号可以省略
- 如果大括号内有且仅有一个语句，可以同时省略大括号，return关键字以及语句分号



lambda表达式的使用前提

- 方法的参数或局部变量类型必须为接口才能使用lambda
- 接口中有且仅有一个抽象方法 @FunctionalInterface



Lambda和匿名内部类的对比

- 所需类型不一样
  - 匿名内部类的类型可以是类，抽象类，接口
  - lambda表达式需要的类型必须是接口
- 抽象方法的数量不一样
  - 匿名内部类所需的接口中的抽象方法的数量是不限制的
  - Lambda表达式所需的接口中只能有一个抽象方法
- 实现原理不一样
  - 匿名内部类：是在编译后形成一个class
  - Lambda表达式：程序运行的时候动态生成class

# 二、接口中新增的方法

## 1.JDK8中接口的新增

在JDK8中针对接口有做增强，在JDK8之前，接口中包含的属性

```java
interface 接口名{
   静态常量；
   抽象方法；
}
```

JDK8之后对接口做了增强，接口中可以有默认方法和静态方法

```java
interface 接口名{
  静态常量；
  抽象方法;
  默认方法;
  静态方法;
}
```

## 2.默认方法

### 2.1为什么要增加默认方法

在JDK8以前接口中只能有抽象方法和静态常量，会存在以下的问题：

在接口中新增抽象方法，所有实现类都需要重写这个方法，不利于接口的扩展

### 2.2 接口默认方法的格式

```java
interface 接口名{
  修饰符 default 返回值类型 方法名{
    方法体
  }
}
```



```java
interface A{
  default String test(){
    return "test"
  }
}
```



### 2.3接口中的默认方法的使用

1. 实现类中直接调用默认方法
2. 实现类中可以重写默认方法

## 3.静态方法

作用：JDK中为接口新增了静态方法，作用也是为了接口的扩展

## 3.1语法规则

```java
interface 接口名{
  修饰符 static 返回类型 方法名{
    
  }
}
```



```java
interface A{
  static String test(){
    return "test static method"
  }
}
```

## 3.2接口中静态方法的使用

接口中的静态方法在实现类中是不能被重写的，调用的话只能通过接口类型来实现  `接口名.静态方法名()`



## 4. 两者的区别

1. 默认方法通过实例调用，静态方法通过接口名调用
2. 默认方法可以被继承，实现类可以直接调用接口默认方法，也可以重写接口默认方法
3. 静态方法不能被继承，实现类不能重写接口的静态方法，只能使用接口名调用

# 三、函数式接口

## 1. 函数式接口的由来

我们使用lambda表达式的前提是需要有函数式接口，而lambda表达式使用时不关心接口名，抽象方法名。只关心抽象方法的参数列表和返回值类型。因此为了让我们使用lambda表达式更加的方便，在JDK中提供了大量常用的函数式接口

```java
public class Demo1Fun {

  public static void main(String[] args) {
    // 示例求和
    int[] arr = new int[]{1,2,3,4};
    int sum = fun1(arr, (arr1 -> {
      int s = 0;
      for (int i : arr1) {
        s += arr1[i];
      }
      return s;
    }));
  }

  public static int fun1(int[] arr ,Operator operator){
    return operator.sum(arr);
  }

}

@FunctionalInterface
interface Operator{
  int sum(int[] arr);
}
```

## 2. 函数式接口介绍

在JDK中帮我们提供的有函数式接口，在java.util.function包中

### 2.1 Supplier

无参有返回值的接口, 主要用来生产数据，提供数据的。

对Lambda表达式需要提供一个返回数据的类型。

```java
@FunctionalInterface
public interface Supplier<T> {

    /**
     * Gets a result.
     *
     * @return a result
     */
    T get();
}
```

使用：

```java
public class SupplierTest {

  public static void main(String[] args) {
    int arr[] = {1,2,35,23,10};
    // 打印数组中的最大值
    printMax(() -> {
      Arrays.sort(arr);
      return arr[arr.length - 1];
    });
  }

  private static void printMax(Supplier<Integer> supplier){
    Integer max = supplier.get();
    System.out.println("max = " + max);
  }
}
```



### 2.2 Consumer

有参无返回值的接口，主要是用来消费数据的，使用的时候需要指定一个泛型来定义参数类型。

```java
@FunctionalInterface
public interface Consumer<T> {

    /**
     * Performs this operation on the given argument.
     *
     * @param t the input argument
     */
    void accept(T t);
}
```

使用：将输入的数据统一转换成小写输出



### 2.3 Function

有参有返回值的接口

### 2.4 Predicate

有参数，返回值为boolean的接口



#### Stream分组

Collectors.groupingby

#### Stream聚合

Collectors.toList .....

并行流的线程安全问题

1. 可以加同步锁
2. 使用线程安全的容器
3. 将线程不安全的容器包装成线程安全的容器

