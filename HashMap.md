# HashMap

HashMap是无序的，Map的有序实现类：TreeMap、LinkedHashMap

JDK7或更早的版本中，HashMap是通过 **数组+链表** 实现的；但在JDK8以后，HashMap是通过 **数组+链表或红黑树** 实现的

在JDK8中，底层的数据结构依然是数组，数组中的元素，如果元素个数不超过8个，那么存储的是链表结构，超过8个以后，将会被转换为红黑树存储结构。链表是不利于查找/寻址的，在最差的情况下，也就是所有key的hash值都一样的时候，时间复杂度会退化到O(n)；红黑树是自平衡的二叉查找树，是对链表可能会很长时查找的优化，即便在最差的情况下，其时间复杂度也只有O(log n)

HashMap是线程不安全的，线程安全的有：Hashtable，ConcurrentHashMap



# HashMap和Hashtable

两者的实现原理几乎完全一样，只有两个差别

- HashMap允许key和value为null，Hashtable则不允许key和value为null
- HashMap线程不安全，Hashtable是线程安全的

不过Hashtable实现线程安全的策略简单粗暴，直接对get、put方法加synchronized，代价太大，在高并发情况下性能太差



# 关于ConcurrentHashMap

线程安全的HashMap，是为了应对HashMap在并发环境下的线程不安全而诞生的

但是，ConcurrentHashMap 在 JDK7 和 JDK8 中的实现有很大的不同，具体的不同点可见下表

|          | JDK7                                                         | JDK8                                                |
| -------- | ------------------------------------------------------------ | --------------------------------------------------- |
| 数据结构 | 数组+Segment（分段锁）                                       | 数组+链表/红黑树（结构非常类似HashMap）             |
| 原理     | 使用分段锁技术，将数据分段，对分段数据加锁，Segment继承自ReentrantLock | Node节点存储数据，CAS+synchronized保证线程安全      |
| 锁粒度   | 对需要进行数据操作的Segment加锁                              | 对每个数组元素Node加锁                              |
| 优缺点   | 缺点：Segment的结构带来的副作用是hash过程比普通的HashMap要长 | 优点：由遍历链表的时间复杂度O(n)变为红黑树的O(logn) |

