# Java Collection

## List

### ArrayList

- 底层数组实现
- 查找访问速度快，数组下标查询，增删效率低
- 线程不安全

	- Collections 的 synchronizedList(new ArrayList()) 实现list线程安全
	- 为list的add方法加锁

		- synchronized(list.get()) {
list.get().add(model);
}

	- 使用CopyOnWriteArrayList

		- List<Object> list1 = new CopyOnWriteArrayList<Object>();

	- 使用ThreadLocal

		- 每new一个线程，就需要new一个list，开销比较大

			- ThreadLocal<List<Object>> threadList = new ThreadLocal<List<Object>>() {
        @Override
         protected List<Object> initialValue() {
              return new ArrayList<Object>();
         }
 };

- 扩容

	- 初始化数组长度为0
	- add的时候，出发长度检查，数组为null时给一个defaul 10的长度
	- 长度不够用时，按照1.5倍扩容

		- oldCapacity+(oldCapacity >> 1)

### LinkedList

- 双向链表
- 适合插入、删除频繁得情况，内部维护了链表长度；查询慢，由于要遍历和对比
- 线程不安全

### Vector

- 底层数组实现
- 线程安全

### Stack

## Set

### HashSet

- 存放的元素的hash值。HashSet通过hashCode值来确定元素在内存中的位置，一个hashCode的位置上可以存放多个元素。

### TreeSet

- 使用二叉树的原理对add元素排序
- 自定义类必须要实现Comparable接口，override compareTo方法进行排序
- 根据compare方法返回值进行排序

### LinkedHashSet

- HashSet+LinkedHashMap
- 底层使用LinkedHashMap保存数据
- 调用父类HashSet的操作方法

### 存储无序，值不能重复的元素

## Map

### HashMap

- 1.7

	- 数组+链表
	- 头插

- 1.8

	- 数组+链表+红黑树
	- 尾插

- 扩容机制

	- capacity数组容量，为2的n次方
	- LoadFactory 默认0.75
	- 扩容阈值 threshold 等于 capacity*loadFactory
	- 创建一个空数组，重新Hash

		- Hash公式和数组长度有关

- 线程不安全

	- Collections.synchronizedMap() 方法实现线程安全

		- 实现原理类似与HashTable，在Map的get和put方法里面加排斥锁

	- ConcurrentHashMap

- 2得幂次

	- 方便位运算
	- 均匀分布

- 重写equals，必须重写hashcode
- Entry

	- final K Key
	- V value
	- Entry next
	- int hash

### HashTable

- 遗留类
- 继承自 Dictionary类
- 线程安全

	- 在put和get方法上加synchronized锁定整个对象

- HashTable由于是遗留类，不建议使用。可以使用HashMap和ConcurrentHashMap进行对应场景的替换

### TreeMap

- 实现SortedMap，默认按照Key的升序保存
- 在使用TreeMap的时候，Key必须实现Comparable 接口或者在构造TreeMap的时候传入自定义的Comparator

### LinkedHashMap

- 记录了插入顺序
- 遍历时按照FIFO的方式
- 也可以构造时指定按照访问次序排序

### ConcurrentHashMap

- 线程安全
- 1.7

	- 数组+链表
	- segment 分段锁

		- 继承了ReentrantLock
		- 尝试获取锁，存在并发竞争，自旋，阻塞

	- get高效， volatile修饰，不需要加锁
	- volatile修饰节点指针
	- HashEntry

- 1.8

	- 数组+链表+红黑树
	- CAS+synchronized

		- CAS失败，自旋保证成功

			- 再失败就sync保证

	- node

- ConcurrencyLevel 并行级别，并发数 默认16，不可扩容

*XMind - Evaluation Version*
