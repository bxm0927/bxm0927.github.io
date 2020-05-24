---
title: Java 类集框架详细解读
date: 2018-07-11 13:48:05
tags: [Java, Collection, List, Set, Map, 集合类, 类集框架]
categories: [Java]
---


## 类集框架概述

类集框架又叫做集合框架或集合类，是 Java 提供的一套性能优良、使用方便的接口和类，位于`java.util`包中。类集框架本质上是对基本的数据结构（线性表、树等）和算法（查找、排序等）的封装。

<!-- more -->

由于基本数据类型不能保存一系列的数据，对其进行扩展便形成了数组；又由于数组长度不可更改，缺乏灵活性，对数组进一步扩展便形成了功能更强大的“类集框架”。数组和类集框架都是一种“容器”，不同的是：

- 数组的长度时固定的，集合类的长度时可变的；
- 数组用来存放基本数据类型，集合类用来存储对象的引用，不能存储基本数据类型，存储的基本数据类型会进行自动类型转换。

```java
// 集合类初体验
public static void main(String[] args) {
    List<Integer> list = new ArrayList<>();
    list.add(1);
    list.add(2);

    System.out.println(list.size()); // 2
    System.out.println(list); // [1, 2]
    list.forEach(item -> System.out.print(item + " ")); // 1 2
}
```




## 类集框架体系结构

类集框架最上层定义了两个接口，Collection 和 Map，Collection 叫做集合接口，每次只保存一个对象；Map 叫做映射接口，每次保存一对对象：

```
public interface Collection<E> extends Iterable<E> { ... }
public interface Map<K,V> { ... }
```

在类集框架的类中，有一些是上层接口，有一些是抽象类（如：AbstractCollection、AbstractList、AbstractSet、AbstractMap等），提供了接口的部分实现，还有一些是具体实现类，这些类可以直接拿来使用。

```
public class ArrayList<E> extends AbstractList<E> implements List<E>, RandomAccess, Cloneable, java.io.Serializable { ... }
public class HashSet<E> extends AbstractSet<E> implements Set<E>, Cloneable, java.io.Serializable { ... }
public class HashMap<K,V> extends AbstractMap<K,V> implements Map<K,V>, Cloneable, Serializable { ... }
```


![](http://oph264zoo.bkt.clouddn.com/18-2-28/99945912.jpg)



## Collection

Collection 是最基本的集合接口，一个 Collection 代表一组 Object，即 Collection 的元素。Collection 接口很少在开发中直接去使用，且没有直接实现它的标准类，通常都会用它的子接口 List 和 Set。

Collection 接口中定义的常用抽象方法：

方法 | 描述
---|---
`int size()` | 类集中元素的个数
`boolean add(Object obj)` | 将 obj 添加到类集中
`boolean addAll(Collection c)` | 将 c 中的所有元素添加到类集中
`boolean remove(Object obj)` | 从类集中删除 obj
`boolean removeAll(Collection c)` | 从类集中删除 c 中的所有元素
`boolean retainAll(Collection c)` | 从类集中删除除了包含在 c 中的所有的元素
`void clear()` |  清空类集中的元素
`boolean contains(Object obj)` | obj 是否属于该类集
`boolean containsAll(Collection c)` | c 中所有元素是否都属于该类集
`boolean isEmpty()` | 类集是否为空
`Object[] toArray()` | 将类集中的元素以数组的形式返回
`Object[] toArray(Object array[])` | 将类集中指定类型的元素以数组的形式返回
`Iterator iterator()` | 调用类集的迭代器



### List

List 接口是实现了 Collection 接口的子接口，是数据结构中的线性表的体现，List 接口下还有一些实现它的标准类：ArrayList、LinkedList，其中：

- ArrayList 是顺序表的体现
- LinkedList 是链表的体现


```
public interface List<E> extends Collection<E> { ... }
public class ArrayList<E> extends AbstractList<E> implements List<E>, RandomAccess, Cloneable, java.io.Serializable { ... }
public class LinkedList<E> extends AbstractSequentialList<E> implements List<E>, Deque<E>, Cloneable, java.io.Serializable { ... }
```

List 集合的特点是：

- List 集合中的元素允许重复
- List 集合中存储的元素在逻辑上是有序的，因此能够通过索引来访问 List 中的元素，能够精确的控制每个元素插入的位置


List 接口对 Collection 接口进行了扩充，增加了一些方法：

方法| 描述
---|---
`Object set(int index, Object obj)` | 对列表中指定索引处的对象进行赋值
`Object get(int index)` | 查找列表中指定索引处的对象（根据索引找对象）
`int indexOf(Object obj)` | 查找列表中 obj 第一次出现的索引，没有则返回 -1（根据对象找索引）
`int lastIndexOf(Object obj)` | 查找列表中 obj 最后一次出现的索引，没有则返回 -1（根据对象找索引）
`void add(int index, Object obj)` | 将 obj 插入到列表的 index 索引处（列表动态扩容）
`boolean addAll(int index, Collection c)` | 将 c 中的所有元素插入到列表的 index 索引处
`Object remove(int index)` | 删除列表中指定索引处的对象（列表动态压缩）
`List subList(int start, int end)` | 返回一个子列表
`ListIterator listIterator()` | 调用列表开始的迭代器
`ListIterator listIterator(int index)` | 调用列表指定索引处开始的迭代器



#### ArrayList

> 可以这么说，Java 中最常用的两个类就是 String 和 ArrayList 了

ArrayList 是 List 接口的实现类，是数据结构中顺序表的体现，相当于一个动态数组（长度可变的数组）。ArrayList 的底层是`Object[]`，String 底层是 `char[]` 数组。


除了上层 List 给予的特性外，ArrayList 集合类还具有以下的特性：

- 允许有 null 元素
- 不同步，非线程安全
- 访问快（查找、遍历），时间复杂度是O(1)；更新慢（插入、删除），时间复杂度为O(n)。当 ArrayLIst 里有大量数据时，这时候去频繁插入或删除元素，会触发底层数组频繁拷贝，效率很低，还会造成内存空间的浪费。


ArrayList 类的构造方法：

```
ArrayList();                    // 初始容量为 10
ArrayList(int initialCapacity); // 指定初始容量为 initialCapacity 大小
ArrayList(Collection c);        // 由 c 中的元素初始化
```

> 注意：虽然初始化数组的长度是10，但是size是0，size是实际长度，并不是数组的容量


#### LinkedList

LinkedList 是 List 接口的实现类，是数据结构中链表（双向循环链表）的体现。链表在物理存储上通常是非连续的，数据元素的逻辑顺序通过链表中的指针引用来实现。

除了上层 List 给予的特性外，LinkedList 集合类还具有以下的特性：

- 允许有 null 元素
- 不同步，非线程安全
- 访问慢（查找、遍历），更新快（插入、删除），因为插入和删除时不会触发底层数组的复制


LinkedList 类的构造方法：

```
LinkedList();               // 初始容量为 10
LinkedList(Collection c);   // 由 c 中的元素初始化
```

```
// LinkedList 初体验
LinkedList<Integer> linkedList = new LinkedList<Integer>();
System.out.println(linkedList); // []

linkedList.addFirst(1); // 这里会自动装箱 int -> Integer
linkedList.addFirst(2);
linkedList.addLast(3);
linkedList.addLast(4);
System.out.println(linkedList); // [2, 1, 3, 4]

linkedList.removeFirst();
linkedList.removeLast();
System.out.println(linkedList); // [1, 3]

linkedList.set(1, 9527);
System.out.println(linkedList.get(1)); // 9527
```


### Set


Set 接口也是 Collection 接口的子接口，它并没有对 Collection 接口进行了扩充，而是完整的继承了 Collection 接口。Set 接口下也有一些实现它的标准类：HashSet、TreeSet、SortedSet。

```
public interface Set<E> extends Collection<E> { ... }
public class HashSet<E> extends AbstractSet<E> implements Set<E>, Cloneable, java.io.Serializable { ... }
public class TreeSet<E> extends AbstractSet<E> implements NavigableSet<E>, Cloneable, java.io.Serializable { ... }
```

Set 集合的特点是：

- Set 集合中的元素不可重复
- Set 集合中的元素是无序的的，因此不能通过索引来访问 Set 中的元素，但是删除和插入效率较高，因为插入和删除不会引起元素位置改变



#### HashSet

HashSet 类使用哈希表实现了 Set 接口。

除了上层 Set 给予的特性外，HashSet 集合类还具有以下的特性：

- 允许有 null 元素，但最多只能一个
- 不同步，非线程安全
- 不保证元素的迭代顺序，同 HashMap


> 所谓无序，就是 Java 语言没有规定 HashSet 按什么顺序遍历。你应该知道，有好多种 Java 虚拟机，有的运行在Windows上，有的运行在 Linux上。即使在同一个平台上，也会有好几种虚拟机。每种虚拟机对 HashSet 的实现都是不一样的，所以每种虚拟机对 HashSet 的遍历顺序可能都不太一样。但对同一种虚拟机来说，你的遍历输出都是一样的。 Java 是跨平台的，你写的程序可能会在不同的平台上运行，这些平台上的虚拟机都是不一样的。如果你选用了 HashSet，就要明白，在不同的平台上，遍历顺序可能会不一样。如果你对遍历顺序有要求，就要考虑使用有序的，或排序的容器。


HashSet 类的构造方法：

```
HashSet(); // 初始化一个哈希表
HashSet(Collection c); // 由 c 中的元素初始化
HashSet(int capacity); // 初始容量为 capacity
HashSet(int capacity, float fillRatio); // 初始容量为 capacity，填充比 fillRatio，默认 0.75
```



```
// HashSet 初体验
HashSet<String> hs = new HashSet<String>();
hs.add("F");
hs.add("C");
hs.add("B");
hs.add("E");
hs.add("D");
hs.add("A");

System.out.println(hs.size()); // 6
System.out.println(hs.add("C")); // false
hs.remove("C");
System.out.println(hs.add("C")); // true
System.out.println(hs); // [A, B, C, D, E, F]
```

#### TreeSet

TreeSet 类使用树（二叉树）实现 了Set 接口。

除了上层 Set 给予的特性外，TreeSet 集合类还具有以下的特性：

- 对象默认按升序存储，检索很快
- 适用于存储大量需要检索的信息


TreeSet 类的构造方法：

```
TreeSet(); // 初始化一个空的 TreeSet
TreeSet(Collection c); // 由 c 中的元素初始化
TreeSet(Comparator comp); // 通过比较器（Comparator）自定义比较规则
TreeSet(SortedSet ss);
```

TreeSet 类扩充的方法：

- first()：返回第一个元素
- last()：返回最后一个元素



```
// TreeSet 初体验
TreeSet<String> ts = new TreeSet<>();
ts.add("1");
ts.add("3");
ts.add("2");
ts.add("C");
ts.add("a");
ts.add("B");

System.out.println(ts.first()); // 1
System.out.println(ts); // [1, 2, 3, B, C, a]
```

#### SortedSet

SortedSet 类也实现了 Set 接口，保存有序的集合。



### Queue

Queue 接口是数据结构中队列的实现，与 List、Set 同一级别，都是继承了 Collection 接口。LinkedList 类实现了 Queue 接口。

队列有两个基本操作：在队列尾部加入一个元素（入队），和从队列头部移除一个元素（出队）

方法 | 描述
---|---
`boolean add(E e)` | 增加一个元索，如果队列已满，则抛出一个IIIegaISlabEepeplian异常
`boolean offer(E e)` | 添加一个元素并返回true，如果队列已满，则返回false
`E remove()` | 移除并返回队列头部的元素，如果队列为空，则抛出一个NoSuchElementException异常
`E poll()` | 移除并返问队列头部的元素，如果队列为空，则返回null
`E element()` | 返回队列头部的元素，如果队列为空，则抛出一个NoSuchElementException异常
`E peek()` | 返回队列头部的元素，如果队列为空，则返回null



---






## Map

Map 叫做映射接口，它存储的是一系列形如`<K, V>`的“键值对”元素，提供 key 到 value 的映射，通过 key 就能找到对应的 value，key 必须是唯一的，value 可以相同。

Map 接口下也有一些实现它的标准类：HashMap、TreeMap、SortedMap 等

```
public interface Map<K,V> { ... }
public class HashMap<K,V> extends AbstractMap<K,V> implements Map<K,V>, Cloneable, Serializable  { ... }
public class TreeMap<K,V> extends AbstractMap<K,V> implements NavigableMap<K,V>, Cloneable, java.io.Serializable { ... }
```


![](http://oph264zoo.bkt.clouddn.com/17-10-6/95753617.jpg)


Map 接口中常用的方法：

方法 | 描述
---|---
`int size()` | 返回“键值对”的个数
`Object get(Object k)` | 通过 key 获取 value
`Object put(Object k, Object v)` | 添加一对对象到映射中，同时改写原来 key 所对应 value（如果存在的话）
`Object putAll(Map m)` | 将映射 m 添加到调用的映射中
`void remove(Object k)` | 删除关键字为 k 的那一对对象
`void clear()` | 清空映射
`boolean containsKey()` | 映射中是否存在该 key
`boolean containsValue()` | 映射中是否存在该 value
`boolean isTmpty()` | 映射是否为空
`Set<Map.Entry<K, V>> entrySet()` | 将 Map 变为 Collection 返回
`Set<k> keySet()` | 得到键的“类集视图”
`Collection values()` | 得到值的“类集视图”


> 需要注意的是，Map 并不属于 Collection，他们没有任何亲缘关系。但可以通过一些方法获得映射的类集视图：entrySet()、keySet() 和 values()，“类集视图”是将映射集成到类集框架内的手段。


### HashMap

HashMap 类使用哈希表实现 Map 接口

HashMap 集合类的特点：

- 允许有 null 元素，但 key 最多有一个 null，value 不限制
- 不同步，非线程安全
- 更新快（插入、删除元素）
- 不保证元素的迭代顺序，同 HashSet

HashMap 类的构造方法：

```
HashMap(); // 初始化一个散列映射
HashMap(Map m); // 由 m 中的元素初始化
HashMap(int capacity); // 初始容量为 capacity
HashMap(int capacity, float fillRatio); // 初始容量为 capacity，填充比 fillRatio，默认 0.75
```

```
HashMap<String, Double> hm = new HashMap<>();
System.out.println(hm); // {}

hm.put("苹果", 19.8);
hm.put("橘子", 15.0);
hm.put("香蕉", 13.5);
hm.put("香蕉", 14.5); // 修改元素
System.out.println(hm); // {苹果=19.8, 香蕉=14.5, 橘子=15.0}

// 将 Map 转换为 Set
Set<Entry<String, Double>> set = hm.entrySet();
Iterator<Entry<String, Double>> it = set.iterator();
while (it.hasNext()) {
    Map.Entry<String, Double> entry = (Map.Entry<String, Double>) it.next();

    System.out.println(entry); // 苹果=19.8 香蕉=14.5 橘子=15.0
    System.out.println(entry.getKey());
    System.out.println(entry.getValue());
}
```


### TreeMap

TreeMap 类使用树（二叉树）来实现 Map 接口。

TreeMap 集合类的特点：

- key 和 value 都不允许有 null 元素
- 元素按升序排序
- 更新和访问的效率不如 HashMap

TreeMap 类的构造方法：

```
TreeMap(); // 初始化一个空的树映射
TreeMap(Comparator comp); // 通过比较器（Comparator）自定义比较规则
TreeMap(Map m); // 由 m 中的元素初始化
TreeMap(sortedMap sm); // 由 sm 中的元素初始化
```

```
TreeMap<String, Double> tm = new TreeMap<String, Double>();
System.out.println(tm); // {}

tm.put("苹果", 19.8);
tm.put("橘子", 15.0);
tm.put("香蕉", 13.5);

Set<String> set = tm.keySet();
Iterator<String> it = set.iterator();
while (it.hasNext()) {
    String string = (String) it.next();
    System.out.println(string);
}

Collection<Double> coll = tm.values();
Iterator<Double> it2 = coll.iterator();
while (it2.hasNext()) {
    Double double1 = (Double) it2.next();
    System.out.println(double1);
}
```


### SortedMap

SortedMap 类也实现了 Map 接口，使 Key 保持在升序排列。




### Map.Entry 接口

`java.util.Map.Entry` 接口描述了在一个 Map 中的一个元素（键/值对），其中有两个重要的方法：

- `getKey()`
- `getValue()`




## Collection 的输出和遍历

### 迭代器 Iterator

Iterator 是标准的类集输出方式。

迭代器（Iterator）是一种设计模式，Java 把它封装成了一个接口，利用这个接口的迭代方法`iterator()`可以实现类集的遍历。

每一个 Collection 接口的标准实现类都提供一个迭代方法`iterator()`，调用这个方法返回一个该类集的迭代器（类似 C 语言中的指针），用来遍历该类集的每一个元素。

Iterator 接口定义如下：

```
public interface Iterator<E> {
    public boolean hasNext();
    public E next();
    public void remove();
}
```

- hasNext：判断是否有下一个元素
- next：取得下一个元素
- remove：删除当前元素（需要先通过`next`取得元素）

使用迭代方法的步骤：

1. 调用该类集的`iterator()`方法获取该类集的迭代器
2. `hasNext()`为真就进行迭代
3. 使用`next()`取得元素

```
List<String> list = new ArrayList<>();
list.add("hello");
list.add("Java");

Iterator<String> iterator = list.iterator();
while (iterator.hasNext()) {
    String elem = iterator.next();
    System.out.println(elem);
}
```

### 双向迭代器 ListIterator

双向迭代器 `ListIterator` 扩展了迭代器 `Iterator`，增加了一些方法，允许双向遍历类集，并且可以修改元素

> 注意：完成反向遍历前，需要先进行正向遍历，让迭代器（指针）指向类集尾部

方法 | 描述
---|---
`void add(Object obj)` | 将 obj 插入到列表
`void set(Object obj)` | 将 obj 赋给当前元素
`void remove()` | 删除当前元素
`boolean hasNext()` | 是否有下一个元素
`boolean hasPrevious()` | 是否有上一个元素
`boolean next()` | 取得下一个元素
`boolean previous()` | 取得上一个元素
`boolean nextIndex()` | 取得下一个元素的索引
`boolean previousIndex()` | 取得上一个元素的索引

```
ArrayList<String> al = new ArrayList<String>();
al.add("a");
al.add("b");
al.add("c");
System.out.println(al);

ListIterator<String> listIt = al.listIterator(); // 返回该类集的双向迭代器 listIt

// 正向遍历
while (listIt.hasNext()) {
    String string = (String) listIt.next();
    listIt.set(string + "-set"); // 修改元素
}

// 反向遍历
while (listIt.hasPrevious()) {
    String string = (String) listIt.previous();
    System.out.println(string); // c-set b-set a-set
}
```


### 枚举迭代 Enumeration

Enumeration 属于最古老的输出接口之一，==尽管没有被废弃，但是已经被迭代器 `Iterator` 所取代。==

若要取得 Enumeration 的对象，只能依靠 Vector 类。

Enumeration 接口定义如下：

```
public interface Enumeration<E> {
    public boolean hasMoreElements();
    public E nextElement();
}
```

```
Vector<String> al = new Vector<String>();
al.add("a");
al.add("b");
al.add("c");

Enumeration<String> en = al.elements();
while (en.hasMoreElements()) {
    String string = (String) en.nextElement();
    System.out.println(string);
}
```

### for、foreach、forEach 输出

foreach 不仅可以用来遍历数组，也可以用来遍历集合类。

```
List<String> list = new ArrayList<>();
list.add("hello");
list.add("Java");

// 普通 foreach
for (String elem : list) {
    System.out.println(elem);
}

// Java 8 Lambda 表达式
list.forEach(item -> {
    System.out.println(item);
});
```



## Map 的输出和遍历

> 遍历技巧：如果需要在遍历过程中增加元素，可以新建一个临时map存放新增的元素，等遍历完毕，再把临时map放到原来的map中

遍历集合类有多种方法，从最早的 Iterator，到 Java 5 支持的 foreach ，再到 Java 8 的 Lambda 表达式，让我们一起来看下具体的用法以及各自的优缺点：


### entrySet()

可以同时拿到key和value，这一种也是最常用的遍历方法，==推荐使用==。

```
Map<String, String> map = new HashMap<>();
map.put("name", "白小明");
map.put("sex", "男");

for (Map.Entry<String, String> string : map.entrySet()) {
    System.out.println(string.getKey());
    System.out.println(string.getValue());
}
```

### keySet()、values()

如果只需要map的key或者value，用map的keySet或values方法无疑是最方便的。

```
Map<String, String> map = new HashMap<>();
map.put("name", "白小明");
map.put("sex", "男");

for (String string : map.keySet()) {
    System.out.println(string);
}

for (String string : map.values()) {
    System.out.println(string);
}
```



### keySet()、get(key)

可以同时拿到key和value，但性能不及上一种，不推荐使用

```
Map<String, String> map = new HashMap<>();
map.put("name", "白小明");
map.put("sex", "男");

for (String string : map.keySet()) {
    System.out.println(string);
    System.out.println(map.get(string));
}
```

### Iterator

Map 接口中没有迭代器`iterator()`方法，需要先用 `entrySet()` 等转成集合才行。

使用 Iterator 的 remove 方法删除元素非常便捷，如果需要在遍历过程中删除元素推荐使用Iterator。

```java
Map<String, String> map = new HashMap<>();
map.put("name", "白小明");
map.put("sex", "男");

Set<Map.Entry<String, String>> set = map.entrySet();
Iterator<Map.Entry<String, String>> it = set.iterator();
while (it.hasNext()) {
  Map.Entry<String, String> entry = (Map.Entry<String, String>) it.next();

  System.out.println(entry.getKey() + " = " + entry.getValue());
  // it.remove(); // 删除元素
}
```


### Java 8  Lambda 表达式

Java 8 提供了 Lambda 表达式支持，语法看起来更简洁，可以同时拿到key和value，不过，经测试，性能低于 entrySet

```
Map<String, String> map = new HashMap<>();
map.put("name", "白小明");
map.put("sex", "男");

map.forEach((key, value) -> {
  System.out.println(key + " = " + value);
});
```


## 关于 Map 遍历的一个简单的性能测试

用100万条数据，做了一个简单性能测试，该测试结果仅供参考

```
private static int sum;

public static void main(String[] args) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < 1000000; i++) {
        map.put(i, 1);
    }

    entrySet(map); // 29 31 31
    lambda(map); // 204 202 238
}

private static void entrySet(Map<Integer, Integer> map) {
    long start = System.currentTimeMillis();

    for (Map.Entry<Integer, Integer> integer : map.entrySet()) {
        sum += integer.getKey() + integer.getValue();
    }

    long end = System.currentTimeMillis();

    System.out.println(end - start);
}

private static void lambda(Map<Integer, Integer> map) {
    long start = System.currentTimeMillis();

    map.forEach((key, value) -> {
        sum += key + value;
    });

    long end = System.currentTimeMillis();

    System.out.println(end - start);
}
```
