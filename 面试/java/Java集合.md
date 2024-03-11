#####  List Set Queue Map四者区别？
* List：存储的元素是有序的、可重复的
* Set: 存储的元素是不可重复的
* Queue：先进先出的数据结构(区别于栈)，存储的元素是有序的、可重复的
* Map：使用键-值对存储，key是无序、不可重复，value是无序、可重复的


#####  ArrayList和Array(数组)的区别？
 * ArrayList 内部基于动态数组实现，比 Array（静态数组） 使用起来更加灵活。
 * ArrayList会根据实际存储的元素动态地扩容或缩容，而 Array 被创建之后就不能改变它的长度了。
 * ArrayList 允许你使用泛型来确保类型安全，Array 则不可以。
 * ArrayList 中只能存储对象。对于基本类型数据，需要使用其对应的包装类（如 Integer、Double 等）。Array 可以直接存储基本类型数据，也可以存储对象。
 * ArrayList 支持插入、删除、遍历等常见操作，并且提供了丰富的 API 操作方法，比如 add()、remove()等。Array 只是一个固定长度的数组，只能按照下标访问其中的元素，不具备动态添加、删除元素的能力。
 * ArrayList创建时不需要指定大小，而Array创建时必须指定大小。
 
 
 #####  集合常见方法时间复杂度
 数组 Array
 访问：O(1)
 查找(未排序)：O(n)
 查找(已排序)：O(log n)
 插入/删除：不支持
 
 数组列表 ArrayList
 添加： O(1)
 删除：O(n)
 查询：O(n)
 求长：O(1)
 
 链接列表 LinkedList
 插入：O(n)
 删除：O(n)
 查找：O(n)
 
 
栈 Stack
压栈：O(1)
出栈：O(1)
栈顶：O(1)
查找：O(n)

队列 Queue/Deque/Circular Queue
插入：O(1)
移除：O(1)
求长：O(1)


#####  比较 HashSet、LinkedHashSet 和 TreeSet 三者的异同
  
  HashSet、LinkedHashSet 和 TreeSet 都是 Set 接口的实现类，都能保证元素唯一，并且都不是线程安全的。
  HashSet、LinkedHashSet 和 TreeSet 的主要区别在于底层数据结构不同。HashSet 的底层数据结构是哈希表（基于 HashMap 实现）。LinkedHashSet 的底层数据结构是链表和哈希表，元素的插入和取出顺序满足 FIFO。TreeSet 底层数据结构是红黑树，元素是有序的，排序的方式有自然排序和定制排序。
  底层数据结构不同又导致这三者的应用场景不同。HashSet 用于不需要保证元素插入和取出顺序的场景，LinkedHashSet 用于保证元素的插入和取出顺序满足 FIFO 的场景，TreeSet 用于支持对元素自定义排序规则的场景。


##### ArrayDeque和ArrayList都是Java中的集合类，但它们在应用场景上有一些不同之处。

ArrayDeque 是一个基于数组实现的双端队列，可以在队列的两端高效地进行插入和删除操作。ArrayDeque适用于以下场景：

队列和栈操作：ArrayDeque可以作为先进先出（FIFO）队列和后进先出（LIFO）栈来使用。它提供了add()、remove()、push()和pop()等方法来实现队列和栈的基本功能。

双端队列：ArrayDeque支持在队列的两端进行插入和删除操作，可以使用offerFirst()、pollFirst()、offerLast()和pollLast()等方法来实现双端队列的功能。

高性能要求：由于ArrayDeque是基于数组实现的，不涉及指针的操作，因此在插入和删除操作上具有较高的性能。相比于LinkedList，ArrayDeque在大部分场景下性能更好。

需要注意的是，ArrayDeque不是线程安全的，如果在多线程环境下使用，需要进行额外的同步控制。

ArrayList 是一个动态数组，可以自动扩展容量，适用于以下场景：

随机访问：ArrayList通过索引可以快速访问元素，支持随机访问操作，即通过索引直接访问某个位置的元素。这使得ArrayList适用于需要频繁访问指定位置元素的场景。

列表操作：ArrayList实现了List接口，提供了一系列方法来进行列表操作，如添加、删除、修改和查找元素等。它具有较好的灵活性和易用性。

动态扩展：ArrayList的容量可以根据需要自动扩展，因此在大部分情况下不需要担心容量问题。可以使用add()方法在末尾添加元素，也可以使用remove()方法删除指定位置的元素。

需要注意的是，ArrayList在频繁插入和删除元素时性能较差，因为插入和删除操作可能涉及到元素的移动。另外，ArrayList也不是线程安全的，如果在多线程环境下使用，需要进行额外的同步控制。

综上所述，如果需要在队列的两端进行高效的插入和删除操作，或者需要实现双端队列或栈的功能，可以选择ArrayDeque。如果需要随机访问元素，或者进行列表操作并且元素的插入和删除操作较少，可以选择ArrayList。

##### ConcurrentHashMap用到了哪些锁

ConcurrentHashMap是Java中的线程安全的哈希表实现。它在内部使用了一种称为"分段锁"（Segment Locking）的机制来提供并发性能。

具体来说，ConcurrentHashMap使用了多个锁（Segment），每个锁保护哈希表中的一部分数据，这样不同的线程可以同时访问不同的部分，从而提高并发性能。每个Segment类似于一个小的哈希表，其中包含了若干个键值对。

在Java 8之前的版本中，ConcurrentHashMap的分段锁使用了ReentrantLock（可重入锁）来实现。每个Segment都有一个关联的ReentrantLock对象，用于保护该Segment的操作。

从Java 8开始，ConcurrentHashMap的实现发生了变化。它引入了一种新的锁实现方式，即Striped64。Striped64通过一种更高效的方式来解决并发问题，并提供更好的扩展性和性能。

总结起来，ConcurrentHashMap使用了分段锁机制来实现并发控制，早期版本使用的是ReentrantLock，而Java 8及以后版本使用的是Striped64锁。这些锁用于保护ConcurrentHashMap内部的各个分段，确保线程安全的同时提供较高的并发性能。


##### ConcurrentHashMap的get方法里有做并发处理吗?

ConcurrentHashMap 的 get 方法之所以支持并发读取操作，并不是因为在 get 方法内部做了显式的并发处理，而是通过分段锁的设计，在更细粒度的层面上实现了并发控制。
这种设计使得在读多写少的场景下，ConcurrentHashMap 能够提供较好的性能表现，同时保证数据的一致性和线程安全性。


##### 数组元素的地址计算与数组的存储方式有关嘛？

在大多数情况下，数组元素的地址计算与数组的存储方式是相关的。具体而言，如果是连续存储方式（如C语言中的数组），那么数组元素的地址计算与数组的存储方式是相关的，因为元素在内存中是按照连续的地址存储的。

但如果是非连续存储方式（如链表等），则可能存在一些例外情况，数组元素的地址计算与数组的存储方式就不再完全相关了。


##### 二位数组的创建

定义二维数组时，不能省略第二维的大小，这是由编译器原理限制的。

#### 数组中的常见方法

push()方法可以在数组的末属添加一个或多个元素
shift()方法把数组中的第一个元素删除
unshift()方法可以在数组的前端添加一个或多个元素
pop()方法把数组中的最后一个元素删除
