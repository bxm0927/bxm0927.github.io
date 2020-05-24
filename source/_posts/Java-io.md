---
title: Java 文件 I/O 流详细解读
date: 2018-07-11 21:31:05
tags: [Java, IO]
categories: [Java]
---


## I/O 概述

利用 Java 的 I/O（input/output，输入和输出）技术可以将数据保存到文本文件、二进制文件甚至是压缩包之中，以达到永久性保存数据的要求。I/O 通常使用“流”的方式进行数据传输，一个流必须有源和目的地，他们通常是磁盘文件，但也可以是键盘、鼠标、显示器、内存甚至是网络等设备。

<!-- more -->


Java 语言定义了许多类专门负责各种形式的输入和输出，这些类被放在`java.io`包中，他们根据数据形态可分为：流式部分和非流式部分。

Java I/O 机制提供了一套简单的标准化 API，以方便从不同的数据源读取和写入字符数据或字节数据。

![](http://oph264zoo.bkt.clouddn.com/18-7-11/63818593.jpg)




## 非流式部分


### File 类

文件自身操作类，是 `java.io` 包中唯一一个与文件本身有关的操作类。File 类用于处理文件和文件系统，如创建、删除、重命名文件等操作，以路径名的形式代表一个文件。

>

File 类的构造方法：

```
File(String filePath)                   // 创建指定文件路径的 File 对象
File(String filePath, String fileName)  // 创建指定文件路径和指定文件名的 File 对象
File(File dirObj, String fileName)
```

```
File f1 = new File("/");
File f2 = new File("/", "a.txt");
File f3 = new File(f1, "a.txt");
```


注意：

- File 类不支持文件内容的读写操作。
- 路径（filePath）分为相对路径和绝对路径，相对路径是相对于项目根目录而言的。


#### File 类常用方法

> 文件路径分隔符：`/`、`\\`或`File.separator`

方法 | 描述
---|---
length() | 文件的长度（字节 Byte）
getName() | 返回文件名
getParent() | 返回父目录名
exists() | 判断文件（或目录）是否存在
canRead() | 判断文件是否可读
canWrite() | 判断文件是否可写
isFile() | 是否是一个文件
delete() | 删除文件
getPath() | 返回相对路径
getAbsolutePath() | 返回绝对路径
getCanonicalPath() | 返回路径名的规范格式？？
isDirectory() |是否是目录
isAbsolute() | 是否是绝对路径名
isHidden() | 是否是隐藏文件
lastModified() | 最后修改时间
list()、list(filter) | 返回当前目录下的文件和目录的列表（String[]）
listFiles() | 返回直接子目录(文件)的抽象
createNewFile() | 创建文件
mkdir() | 创建单级目录
mkdirs() | 创建多级目录
renameTo() | 重命名

```
File f1 = new File("e:" + File.separator + "test.txt");

if (f1.exists()) {
    f1.delete();
    System.out.println("文件删除成功");
} else {
    try {
        f1.createNewFile();
        System.out.println("文件创建成功");
    } catch (IOException e) {
        e.printStackTrace();
    }
}

System.out.println(f1.getName()); // test.txt
System.out.println(f1.getPath()); // e:\test.txt
System.out.println(f1.getAbsolutePath()); // e:\test.txt
System.out.println(f1.getParent()); // e:\
System.out.println(f1.canRead()); // true
System.out.println(f1.canWrite()); // true
System.out.println(f1.isDirectory()); // false
System.out.println(f1.isFile()); // true
System.out.println(f1.isAbsolute()); // true
System.out.println(f1.lastModified()); // 1519556856932
System.out.println(f1.length() + "Byte"); // 0Byte
```


### RandomAccessFile 类

文件随机访问类，即可以跳转到文件的任意位置处进行读写操作。RandomAccessFile 对象有一个位置指示器，指向当前读写处的位置。该类仅限于操作文件，不能访问其他 IO 设备，如网络、内存映象等等。



RandomAccessFile 类的构造方法：

```
RandomAccessFile(File file, String mode)     // 创建一个随机访问文件流，文件属性由 File 参数指定
RandomAccessFile(String name, String mode)   // 创建一个随机访问文件流，文件名由 name 参数指定
```

mode 决定了随机访问文件流的操作模式：

value | meaning
---|---
r | read，以只读方式打开，调用该对象的任何写方法都会导致 IOException 异常
rw | read-write，以读、写方式打开，若文件不存在，则创建之
rws | read-write-synchronized，以读、写方式打开，且文件内容同步更新
rwd | 类似 rws

#### RandomAccessFile 类常用方法


方法 | 描述
---|---
close() | 关闭此随机访问文件流
length() | 返回此文件的长度
read() | 从这个文件读取一个字节的数据
write(byte[] b) | 写 b.length 字节到文件中
getFilePointer() | 返回此文件中的当前偏移量（位置指示器）
seek(long pos) | 设置文件指针偏移量（位置指示器）
skipBytes(int n) | 跳过指定字节

```
import java.io.File;
import java.io.RandomAccessFile;

// 向文件中写入三个员工的信息，并读出
public class Test {
    public static void main(String[] args) throws Exception {
        Employee person1 = new Employee("zhangsan", 18);
        Employee person2 = new Employee("lisi", 20);

        File file = new File("C:/Users/bxm09/Desktop/test.txt");
        if (file.exists()) {
            file.delete();
        }

        RandomAccessFile f1 = new RandomAccessFile("C:/Users/bxm09/Desktop/test.txt", "rw");
        f1.write(person1.name.getBytes());
        f1.writeInt(person1.age);
        f1.write(person2.name.getBytes());
        f1.writeInt(person2.age);
        f1.close();

        RandomAccessFile f2 = new RandomAccessFile("C:/Users/bxm09/Desktop/test.txt", "r");
        System.out.println("第 2 个员工的信息：");
        f2.skipBytes(12); // 跳过指定字节
        String p1Name = "";
        for (int i = 0; i < 8; i++) {
            p1Name += (char) f2.readByte();
        }
        System.out.println("name：" + p1Name);
        System.out.println("age：" + f2.readInt());

        System.out.println("第 1 个员工的信息：");
        f2.seek(0); // 设置文件指针偏移量
        String p2Name = "";
        for (int i = 0; i < 8; i++) {
            p2Name += (char) f2.readByte();
        }
        System.out.println("name：" + p2Name);
        System.out.println("age：" + f2.readInt());
        f2.close();
    }
}

// 员工类
class Employee {
    String name; // 员工姓名
    int age; // 员工年龄
    final static int LENGTH = 8; // 规定 name 为 8 个字符，方便读取。8 个字符 = 8个汉字 = 8个数字 = 8个字母

    public Employee(String name, int age) {
        if (name.length() > LENGTH) {
            name = name.substring(0, LENGTH); // 超八位则截取
        } else {
            while (name.length() < LENGTH) {
                name += "\u0000"; // 不足八位用空格代替
            }
        }

        this.name = name;
        this.age = age;
    }
}
```





---






## 流式部分


在 Java 中，所有的 I/O 机制都是基于数据“流”方式完成的，而操作流的对象都封装到了 `java.io` 包中。


I/O 流的分类：

- 按操作数据的内容形态划分：字节流（InputStream、OutputStream）和字符流（Reader、Writer）
- 按流的功能流向划分：输入流（InputStream、Reader）和输出流（OutputStream、Writer）


```
public abstract class InputStream implements Closeable { ... }
public abstract class OutputStream implements Closeable, Flushable { ... }
public abstract class Reader implements Readable, Closeable { ... }
public abstract class Writer implements Appendable, Closeable, Flushable { ... }
```



> 字节流类和字符流类的 API 大致相同

> 流属于资源，使用后要关闭之（记得擦屁股 - - !）

![](http://oph264zoo.bkt.clouddn.com/18-3-5/67568977.jpg)



### 字符流

字符流又叫做文本流，是用于操作字符数据的 I/O 流，==通常用来处理字符或字符串数据（汉字、多国语言等）==。

- Reader 字符输入流
- Writer 字符输出流


每个字节存放一个 ASCII 码，代表一个字符（而对于 Unicode 编码来说，每两个字节表示一个字符）。


```
File file = new File("C:/Users/bxm09/Desktop/test.txt");

Writer out = new FileWriter(file);
out.write("hello world!");
out.close();

Reader in = new FileReader(file);
char[] accept = new char[1024]; // 此数组用于存放读取到的数据
int len = in.read(accept);
System.out.println(new String(accept, 0, len)); // 将字符数组转化为字符串
in.close();
```


### 字节流

字节流又叫做二进制流，是用于操作字节数据的 I/O 流，==通常用来处理二进制数据（多媒体数据、网络传输数据等）==。

- InputStream 字节输入流
- OutputStream 字节输出流

字节流在输入输出时，与内存中的存储形式相同，以单个字节（Byte）为读写单位。用字节流来处理数据可以节省内存空间和转化时间，但一个字节并不代表一个字符，所以不能直接输出字符形式。


一般在操作文件流时，不管是字节流还是字符流，都可以按照如下的流程进行：

1. 使用 File 类找到一个要操作的文件对象。
2. 使用 File 类的对象去实例化字节流类或字符流类的子类
3. 使用字节/字符的读写操作
4. 关闭 IO 流资源


```java
File file = new File("C:/Users/bxm09/Desktop/test.txt");

// 写文件
OutputStream out = new FileOutputStream(file);
byte[] b = "hello world!".getBytes();
out.write(b);
out.close();

// 读文件
InputStream in = new FileInputStream(file);
byte[] accept = new byte[1024]; // 此数组用于存放读取到的数据
int len = in.read(accept);
System.out.println(new String(accept, 0, len)); // 将字节数组转化为字符串
in.close();
```





### 文件流

使用文件流可以将数据永久保存在文件中。

- FileInputStream 字节文件输入流
- FileOutputStream 字节文件输出流
- FileReader 字符文件输入流
- FileWriter 字符文件输出流

```
// 文件流初体验
// 使用 FileInputStream 向文件写入数据，然后使用 FileOutputStream 读取文件中的数据

File file = new File("hello.txt");

// 写文件
try {
    OutputStream out = new FileOutputStream(file);
    out.write("HELLO WORLD".getBytes());

    out.flush(); // 记得擦屁股
    out.close();
} catch (IOException e) {
    e.printStackTrace();
}

// 读文件
try {
    InputStream in = new FileInputStream(file);
    byte[] data = new byte[1024]; // 用于存储读取到的数据
    int len = in.read(data);
    System.out.println(new String(data, 0, len));

    in.close();
} catch (IOException e) {
    e.printStackTrace();
}
```




### 缓冲流


为了达到较高的转换效率，建议不要直接使用 InputStream、OutputStream 来进行读写，而应尽量使用缓冲流类来对数据进行缓存处理。

- BufferedReader 字符缓冲输入流
- BufferedWriter 字符缓冲输出流
- InputStreamReader 字节缓冲输入流
- OutputStreamWriter 字节缓冲输出流

![](http://oph264zoo.bkt.clouddn.com/18-7-11/43278259.jpg)



```
// 缓冲流初体验，性能对比，复制一首歌曲
public static void main(String[] args) throws IOException {
    InputStream in = new FileInputStream("爱我中华.mp3"); // 指定需要复制的文件路径
    OutputStream out = new FileOutputStream("爱我中华2.mp3"); // 指定文件复制后的输出路径

    // copyOfByte(in, out); // 耗时18599毫秒
    // copyOfByteArray(in, out); // 耗时40毫秒
    copyOfBuffer(in, out); // 耗时14毫秒
}

// 1. 使用字节读写
private static void copyOfByte(InputStream in, OutputStream out) throws IOException {
    long startTime = System.currentTimeMillis();

    int data = 0;
    while (data != -1) {
        data = in.read(); // read 方法返回 -1 表示要读的文件已经到了尽头
        out.write(data);
    }
    in.close();
    out.flush();
    out.close();

    long endTime = System.currentTimeMillis();
    System.out.println("耗时" + (endTime - startTime) + "毫秒");
}

// 2. 使用字节数组读写
private static void copyOfByteArray(InputStream in, OutputStream out) throws IOException {
    long startTime = System.currentTimeMillis();

    byte[] bytes = new byte[1024];
    int data = 0;
    while (data != -1) {
        data = in.read(bytes);
        out.write(bytes);
    }
    in.close();
    out.flush();
    out.close();

    long endTime = System.currentTimeMillis();
    System.out.println("耗时" + (endTime - startTime) + "毫秒");
}

// 3. 使用缓冲流来读写
private static void copyOfBuffer(InputStream in, OutputStream out) throws IOException {
    long startTime = System.currentTimeMillis();

    InputStream buffIn = new BufferedInputStream(in);
    OutputStream buffOut = new BufferedOutputStream(out);
    byte[] bytes = new byte[1024];
    int data;
    while ((data = buffIn.read(bytes)) != -1) {
        buffOut.write(bytes, 0, data);
    }
    in.close();
    out.flush();
    out.close();

    long endTime = System.currentTimeMillis();
    System.out.println("耗时" + (endTime - startTime) + "毫秒");
}
```




### 转换流

字符流对字节流进行了包装，使其可以接受字符串的输入，字符流在输入时在其底层将字符转化为字节，读取时又将字节转化为字符。如果要将字节显示为字符，就需要用到字节与字符的转换。

- InputStreamReader：字节输入流转为字符输入流
- OutputStreamWriter：字节输出流转为字符输出流


```
// 读取来自控制台的输入
BufferedReader buffIn = new BufferedReader(new InputStreamReader(System.in));
BufferedWriter buffOut = new BufferedWriter(new OutputStreamWriter(System.out));

String readLine = buffIn.readLine(); // 读取键盘上输入的一整行字符
```

> JDK 5 后的版本我们也可以使用 Java Scanner 类来获取控制台的输入。



### 管道流

管道：将一个程序的输出当做另一个程序的输入。

管道流主要用于连接两个线程间的通讯。

管道流分为两组：

- 字节管道操作流：PipedInputStream、PipedOutputStream
- 字符管道操作流：PipedReader、PipedWriter


一个 PipedInputStream 对象必须和一个 PipedOutputStream 对象进行连接，才能产生一个通信管道。

```
import java.io.IOException;
import java.io.PipedInputStream;
import java.io.PipedOutputStream;

// 管道流
public class Test {
    public static void main(String[] args) throws Exception {
        Sender sender = new Sender();
        Receiver receiver = new Receiver();

        PipedOutputStream out = sender.getPipedOutputStream(); // 返回各自的管道流
        PipedInputStream in = receiver.getPipedInputStream();

        out.connect(in); // 管道连接

        new Thread(sender).start(); // 启动线程
        new Thread(receiver).start();
    }
}

// 消息发送方
class Sender implements Runnable {
    PipedOutputStream out = new PipedOutputStream();

    public PipedOutputStream getPipedOutputStream() {
        return out;
    }

    @Override
    public void run() {
        String str = "hello world!";

        try {
            out.write(str.getBytes());
            out.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

// 消息接受方
class Receiver implements Runnable {
    PipedInputStream in = new PipedInputStream();

    public PipedInputStream getPipedInputStream() {
        return in;
    }

    @Override
    public void run() {
        byte[] accept = new byte[1024];

        try {
            int len = in.read(accept);
            System.out.println("Receiver：" + new String(accept, 0, len));
            in.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```




### 打印流

打印流是一个输出信息最方便的流类，==如果由程序向一个终端输出数据时，一定要使用打印流==。

打印流特点：

1. 不抛出 IO 异常
2. 自动刷新

打印流分为两组：

- 字节打印流：PrintStream
- 字符打印流：PrintWriter

PrintStream 类提供了一系列重载的 print 和 println 方法，用来将基本数据类型的格式转换为字符串进行输出，对于一个非基本数据类型的对象，print 和 println 方法会默认调用对象的 toString 方法。


PrintStream 继承了 OutputStream类，并且实现了方法 write()，但是 write() 方法不经常使用，因为 print() 和 println() 方法用起来更为方便。


> 打印流使用的是“装饰设计模式”，即：将一个不是很完善的功能，添加一些代码后将其完善起来。

> `System.out.prinln` 语句中的 `System.out` 就是 PrintStream 类的一个实例化对象。

```
// 向屏幕输出信息
PrintWriter out = new PrintWriter(System.out);
out.println("hello");
out.close();

// 向文件输出信息
File f = new File("C:/Users/bxm09/Desktop/test.txt");
PrintWriter out = new PrintWriter(new FileWriter(f));
out.println("hello");
out.close();
```

#### 打印流的格式化输出

类似C语言的 printf 函数，即用一些占位标记来暂时代替输出。

占位标记 | 含义
---|---
%s | 字符串
%S | 字符串（大写形式）
%c | 字符
%d | 整数（十进制）
%o | 整数（八进制）
%x | 整数（十六进制）
%X | 整数（十六进制大写形式）
%f | 小数
%.nf | 小数（限制小数位数）
%e | 科学计数法
%E | 科学计数法（大写形式）
%n | 换行
%b | 布尔值

```
System.out.printf("字符串：%s，小数：%.2f", "hello", 20.123); // 字符串：hello，小数：20.12
```




### 内存操作流

在内存中读写数据，而不是在文件中。此时，内存作为源和目的地。

内存操作流分为两组：

- 字节内存操作流：ByteArrayInputStream、ByteArrayOutputStream
- 字符内存操作流：CharArrayReade、CharArrayWriter





### 合并流（序列流）

SequenceInputStream 类可以将多个输入流按顺序连接起来，也可以实现多个文件的合并操作。

```
// 两个文件的合并为一个文件
File f1 = new File("e:/1.txt");
File f2 = new File("e:/2.txt");
File f3 = new File("e:/3.txt");

FileInputStream in1 = new FileInputStream(f1); // 文件输入流
FileInputStream in2 = new FileInputStream(f2); // 文件输入流

SequenceInputStream s = new SequenceInputStream(in1, in2); // 将两个输入流合并为一个输入流

FileOutputStream out = new FileOutputStream(f3); // 文件输出流

int len;
while ((len = s.read()) != -1) {
    out.write(len);
}

in1.close();
in2.close();
out.close();
```


### 对象流（对象序列化）

对象序列化是指将内存中保存的对象转化为二进制数据流的形式的一种操作。通过对象序列化，可以方便的地实现对象的存储和读取。

对象反序列化就是使用 ObjectInputStream 类将序列化的对象读取出来。

在 Java 中，并不是所有类的对象都可以被序列化，会报`NotSerializableException`异常，只有实现了 `java.io.Serializable` 接口的类的对象可以被序列化，Serializable 接口中没有任何成员，是一个标识接口。


有两个类用于对象序列化操作：

- 对象输入流：ObjectInputStream
- 对象输出流：ObjectOutputStream

#### transient 关键字

阻止对象序列化

在默认情况下，当一个类的对象序列化时，会将这个类的所有属性都保存下来，如果不希望类中的某个属性被序列化，就需要加上 transient 关键字。


```
public class Test {
    public static void main(String[] args) throws Exception {
        File file = new File("./test");
        serializ(file);
        deserializ(file);
    }

    // 对象序列化：存储
    private static void serializ(File file) throws Exception {
        OutputStream out = new FileOutputStream(file);
        ObjectOutputStream cout = new ObjectOutputStream(out);
        cout.writeObject(new Person("白小明")); // 写对象
        cout.close();
    }

    // 对象反序列化：读取
    private static void deserializ(File file) throws Exception {
        InputStream in = new FileInputStream(file);
        ObjectInputStream cin = new ObjectInputStream(in);
        Person p = (Person) cin.readObject(); // 读对象
        System.out.println(p.getName()); // null
        cin.close();
    }
}

@SuppressWarnings("serial")
class Person implements Serializable {
    private transient String name; // 该属性不想被序列化

    public Person(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

