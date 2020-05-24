---
title: Java 异常处理详细解读
date: 2018-07-10 10:35:58
tags: [Java, Exception, 异常处理]
categories: [Java]
---

## 异常概述

异常（Exception）指的是所有可能造成计算机无法正常处理的情况。发生异常时，将阻止程序的运行，若不妥善处理异常，可能造成计算机死机。经过异常处理，可以保证程序的正常运行，我们把针对不同的异常做妥善的处理的方式叫做“异常处理机制”。

异常处理的目的：加强程序的健壮性、安全性。

<!-- more -->

Java 中，所有的异常都被封装到相应的类中，同时，用户也可以自定义异常类和自定义抛出异常。

- 抛出异常（throw）：是指将异常提交给运行时系统的过程。
- 捕获异常（catch）：是指运行时系统找到发生异常的方法的过程。

![](http://oph264zoo.bkt.clouddn.com/18-7-10/17076115.jpg)


## 异常类 `Throwable`

异常类是指由程序抛出的异常对象所属的类。

异常可以分为两大类：`java.lang.Exception`类和`java.lang.Error`类。这两个类均继承自 `java.lang.Throwable` 类

- Error 类通常指的是 JVM 出错和线程死锁，用户无法在程序中处理（无法捕获）
- Exception 类通常指的是可以处理的异常，分为编译时异常和运行时异常（RuntimeException），编译时异常是 Exception 下除了运行时异常以外的所有异常
    - 对于编译时异常，不解决编译就不会通过
    - 对于运行时异常，可以选择性地来进行处理，如果不处理则出现异常时将交给 JVM 默认处理

![](http://oph264zoo.bkt.clouddn.com/18-7-10/42397762.jpg)



### 获取异常信息的方法


![](http://oph264zoo.bkt.clouddn.com/18-7-10/87466879.jpg)



### Java 中常见的异常类

异常 | 描述
---|---
NullPointerException | 空指向异常（空指针异常）。指的是使用了一个未初始化的对象（未开辟内存空间的对象）。
ArithmeticException | 算术异常。如：除以零
ArrayIndexOutOfBoundsException | 数组下标越界异常。数组的索引超过了上界或下界
FileNotFoundException | 文件未找到异常
OutOfMemoryExceptin | 内存溢出异常（内存不足异常）。可用内存不足以分配给一个对象时抛出
NoSuchElementExceptin | 调用了类集中不存在的元素
ClassCastExceptin | 对象与类集中的元素不兼容
UnsupportedOperationExceptin | 修改一个不允许修改的对象




## 异常处理 `try-catch-finally`

> 死了都要 try，不 try 到 catch 不痛快

![](http://oph264zoo.bkt.clouddn.com/17-10-15/60958506.jpg)


当发生异常时，通常有两种处理方法：

1. 交给 Java 默认的异常处理机制做处理。这种情况 Java 会输出异常信息，然后中断程序的运行
2. 自己编写 `try-catch-finally` 来捕获异常。这种情况可以灵活的控制程序，而且程序不会中断运行。



异常处理的语法：

```
try {
    // 要检查的语句
} catch (异常类 e) {
    // 异常发生时的处理语句
    e.printStackTrace(); // 打印异常信息
} finally {
    // 一定会运行到的语句
}
```

- try 语句块不可以独立存在，必须与 catch 或 finally 同存，finally 语句块可以省略
- catch 区块可以有多个，此时异常类型必须子类在前，父类在后
- 当 try 区块捕获到异常时，不执行接下来的语句，立即进入 catch 区块


![](http://oph264zoo.bkt.clouddn.com/17-10-3/75135977.jpg)


### finally 语句块不会被执行的情况

1. 在 finally 语句块中发生了异常
2. 在前面的代码中使用了 System.exit() 退出程序
3. 程序所在的线程死亡
4. CPU 关闭


### 异常处理的几种形式

#### try...catch

![](http://oph264zoo.bkt.clouddn.com/18-7-10/67665230.jpg)

#### try...catch...finally

![](http://oph264zoo.bkt.clouddn.com/18-7-10/14424309.jpg)

#### try...finally

![](http://oph264zoo.bkt.clouddn.com/18-7-10/32735647.jpg)


## 向上抛出异常 `throws`

方法头中使用 `throws` 关键字可以表明这个方法可能存在的异常类型，此方法不处理异常，而是将该异常提交给调用这个方法的方法。

![](http://oph264zoo.bkt.clouddn.com/18-7-10/75648718.jpg)

```
public class Test {
    public static void main(String[] args) {
        int[] arr = new int[5];
        try {
            setZero(arr, 10);
        } catch (Exception e) {
            e.printStackTrace();
        }
        System.out.println("end");
    }

    public static void setZero(int[] arr, int index) throws ArrayIndexOutOfBoundsException {
        arr[index] = 0;
    }
}

```


## 自行抛出异常 `throw`

`throws`用于方法声明中，声明一个方法会抛出哪些异常，而`throw`是方法体中实际执行抛出异常的动作。



```
throw 异常类型的实例
```

```
try {
    throw new ArrayIndexOutOfBoundsException("我是异常信息");
} catch (Exception e) {
    e.printStackTrace();
}
```

## 自定义异常类

当系统提供的异常类不足以满足业务需求时，我们可以自定义异常类

```
class 自定义异常类 extends 异常类型 {

}
```

```
public class Test {
    public static void main(String[] args) {
        try {
            throw new MyException("我是自定义异常类");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

class MyException extends Exception {
    private String msg;

    public MyException(String msg) {
        this.msg = msg;
    }

    @Override
    public String toString() {
        return "MyException [msg=" + msg + "]";
    }
}
```



## 异常处理的经验之谈

- 不要忽略异常
- 只针对异常的情况才使用异常
- 优先使用标准异常，而不是直接在方法上 throws Exception 或 Throwable
- 每个方法抛出的异常都应该有文档
- 处理运行时异常时，采用逻辑规避同时辅助try-catch处理
- 在多重catch块后面，可以加上一个`catch(Exception)`来处理被遗漏的异常
- 对于不确定的代码，也可以加上`try-catch`，处理潜在的异常
- 尽量去处理异常，切忌只是简单的调用`printStackTrace()`去打印输出
- 尽量添加`finally`去释放占用的资源
- 具体如何处理异常，要根据不同的业务需求和异常类型去决定

