# java集合

> 2023.11.5

* 集合

1. 可以动态的保存任意多个对象
2. 提供一系列的操作对象方法
3. 使用集合添加，删除新元素，简洁

## 集合的框架体系图

![image-20231105112651586](https://boimage.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20231105112651586.png)

![image-20231105112717885](https://boimage.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20231105112717885.png)

> 1. Iterator 对象称为迭代器，主要用于遍历 Collection 集合中的元素
> 2. 所有实现了 Collection接口的的集合类都有一个 iterator() 方法， 用以返回一个实现了 Iterator 接口的对象，既可以返回一个迭代器
> 3. Iterator 结构图
> 4. Iterator 仅用于遍历集合，Iterator 本身并不存放对象

* 迭代器的原理

```java
Iterator iterator = coll.iterator(); // 得到一个集合迭代器
// hasNext() : 判断是否有下一个元素
while(iterator.hasNext()){
    // next() 作用：下移 2：将下移以后集合位置上的元素返回
    System.out.println(iterator.next());
}

/*
	在调用 iterator.next() 的时候必须先调用 iterator.hasNext() 进行检测，若不调用，
	且下一条记录无效，直接调用 it.next() 会抛出 NoSuchElementException 异常
*/

// 显示所有的快捷键的快捷键  ctrl + j
```

# List

## List接口和常用方法

List接口是 Collection 接口的子类

1. List集合中的元素有序（即添加顺序和取出顺序一致），且可以重复
2. 每个元素都有其对应的顺序索引，即支持索引
3. List容器中的元素都对应一个整型的序号记载其在容器中的位置，可以根据序号取出容器中的元素
4. JDK API 中 List 接口的实现类  ArrayList  LinkedList  Vector

## ArrayList 底层结构源码分析

1. ArrayList 中 维护了一个 Object 类型的数组 elementData [debug 看源码]
2. 创建 ArrayList  对象时，如果使用的是无参构造器，则初始elemantData 容量 为 0 ，第一次参加则扩容  elementData 为 10， 如需要再次扩容，则扩容 elementData 为 1.5 倍
3. 如果使用的是指定大小的构造器，则初始 elementData 的容量为指定大小，如果需要扩容，则直接扩容 elementData 的大小为 1.5 倍

## LinkedList 的底层结构源码分析

1. LinkedList 底层实现了双向链表和双端队列的特点
2. 可以添加任意元素（元素可以重复），包括 null
3. 线程不安全，没有实现同步

* LinkedList的底层操作机制
  1. LinkedList 底层维护了了一个双向链表
  2.  LinkedList 中维护了两个属性 first 和 last 分别指向首节点和尾结点
  3. 每个节点 （Node对象）里面维护了 prev next item  三个属性，其中通过prev指向前一个，通过 next 指向后一个，最终实现了双向链表
  4. 所以LinkedList 的元素的添加和删除，不是通过数组完成的，相对来说效率较高
  5. 模拟一个简单的双向链表 （走代码） 

## ArrayList 和 LinkedList  的比较

|            | 底层结构 | 增删的效率           | 改查的效率 |
| ---------- | -------- | -------------------- | ---------- |
| ArrayList  | 可变数组 | 较低   数组扩容      | 较高       |
| LinkedList | 双向链表 | 较高，通过链表的追加 | 较低       |

* 如何选择 ArrayList 和 LinkedList:
  1. 如果我们改查的操作多 选择 ArrayList
  2. 如果我们增删的操作多 选择 LinkedList
  3. 一般来说，在程序中 80% - 90% 都是查询，因此大部分情况下选择 ArrayList
  4. 在一个项目中，根据业务灵活选择，也可能这样，一个模块使用的是 ArrayList,另一个模块使用的 LinkedList,也就是所 根据业务来选择

# Set

## set接口常用的方法

* Set接口的介绍

1. 无序（添加和取出的顺序不一致），没有索引
2. 不允许重复元素，所以最多包含一个 null
3. jdk API 中 set接口的实现类有 ![image-20231116141936283](https://boimage.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20231116141936283.png)

* Set 接口得常用方法

  跟List接口一样，Set也是collection 的 子接口 ，因此常用方法和 Collection 接口一样

* Set接口的遍历方式

​		同Collection的遍历方式一样，因为 Set 接口是 collection 的子接口

1. 可以使用迭代器
2. 增强 for
3. 不能使用索引的方式来获取

* HashSet底层机制说明

> 1. HashSet 底层是 HashMap
> 2. 添加一个元素时，先得到一个 hash 值 会转成 -> 索引值
> 3. 找到存储数据表table，看这个索引位置是否已经存放元素
> 4. 如果没有直接加入
> 5. 如果有调用 equals 比较，如果相同就放弃添加，如果不同则添加到最后
> 6. 在java中，如果有一条链表的元素个数达到 TREEIFY_THRESHOLD (默认是 8 )，并且table 的大小 >= MIN_TREEIFY_CAPACITY(默认值是 64)，就会进行树化 （红黑树）

> 1. 先获取元素的哈希值（hashCode 方法）
>
> 2. 对哈希值进行运算，得出一个索引值即为要存放在哈希表中的位置号
>
> 3. 如果该位置上没有其他元素，则直接存放
>
>    如果该位置上已经有其他元素，则需要进行 equals 判断，如果相等，则不在添加，如果不相等，则以链表的方式添加

## 分析 hashSet 扩容和转 红黑树 的机制

1. hashSet 底层是 HashMap ，第一添加时， table 数组扩容到 16，临界值（threshold） 是 16*加载因子（loadFactor）是 0.75 = 12
2. 如果 table 水族使用到了临界值 12，就会扩容到 16* 2 = 32. 新的临界值 就是 32*0.75 = 24  题词类推
3. 在 java8 中 如果一条链表的元素个数到达 TREEIFY_THRESHOLD(默认是 8)， 并且table 的大小  >= MIN_TRREEIFY_CAPACITY(默认是 64)， 就会进行树化（红黑树）， 否则仍然采用数组扩容机制

​	

## Set 接口实现子类 - LinkedHashSet

> 1. LinkedHashSet 是 HashSet 的子类
> 2. LinkedHashSet 底层是一个 LinkedHashMap， 底层维护了一个数组 + 双向链表
> 3. LinkedHashSet 根据元素的 hashCode 值来决定元素存储的位置，同时使用链表维护元素的次序（图），这使得元素看起来是以插入顺序保存的
> 4. LinkedHashSet 不允许重复添加元素

# Map接口

## Map接口和常用方法[很实用]

> 注意： 这里讲的是 JDK 8 的 Map  接口的特点

1. Map 与 Collection 并列存在，用于保存具有映射关系的数据： Key - Value
2. Map 中的 key 和 value 可以是引用数据类型， 会封装待 HashMap$Node 对象中
3. Map 中的 key 不允许重复， 原因和 HashSet 一样
4. Map 中的 Value 可以重复
5. Map 的 key 可以为 null， value  也就可以为 null，注意 key  为 null ，只能有一个，value 为 null 可以有多个
6. 常用的 String 类作为 Map 的 key
7. key 和 value 之间存在单向一对一的关系，即通过指定的 key 总能找到对应的 value