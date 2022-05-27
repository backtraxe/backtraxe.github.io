# Java Collections Framework 教程


<!--more-->

## 1.数组

### 打印数组

```java
import java.utils.Arrays;
int[] arr1;
System.out.println(Arrays.toString());
int[][] arr2;
System.out.println(Arrays.deepToString());
```

### 类型转换

```java
// int[] 转 List<Integer>
// 不可添加、删除、修改
int[] arr;
List<Integer> list = List.of(arr);
```

```java
// int[] 转 Integer[]
int[] arr;
Integer[] arr2 = IntStream.of(arr).boxed()
```

```java
// Integer[] 转 List<Integer>
// 不可添加、删除、修改
Integer[] arr;
List<Integer> list = List.of(arr);
```

```java
// List<Integer> 转 Integer[]
List<Integer> list;
int[] arr2 = list.stream().mapToInt().toArray(Integer::new);
```

## 2.Collection

### 2.1 方法

- `int size()`：返回容器大小
- `boolean isEmpty()`：容器是否为空
- `boolean contains(Object element)`：是否包含指定元素
- `boolean add(E element)`：添加元素；未添加成功返回 false
- `boolean remove(Object element)`：删除指定元素；未删除成功返回 false
- `Iterator<E> iterator()`：返回迭代器
- `boolean containsAll(Collection<?> c)`：$A \supset B$；当前容器是否包含了容器 c 中的所有元素
- `boolean addAll(Collection<? extends E> c)`：$A \cup B$；添加容器 c 中的所有元素到当前容器
- `boolean removeAll(Collection<?> c)`：$A-B$；从当前容器中删除同时包含于容器 c 中的所有元素
- `boolean retainAll(Collection<?> c)`：$A \cap B$；从当前容器中删除不包含于容器 c 中的所有元素
- `void clear()`：清空容器
- `Object[] toArray()`：容器转 Object 数组
- `<T> T[] toArray(T[] a)`：容器转数组
- ``

### 2.2 遍历

**（1）聚合操作（Stream + Lambda）**：只能访问，不能修改。

```java
myColl.stream()
    .filter(e -> e.getAttribute() == "Attribute")
    .forEach(e -> System.out.println(e.toString()));

myColl.forEach(System.out::println);
```

**（2）for-each**：只能访问，不能修改。

```java
for (Object o : myColl) {
    System.out.println(o);
}
```

**（3）Iterator**：可修改。

```java
// 定义
public interface Iterator<E> {
    boolean hasNext(); // 是否存在下个元素
    E next();          // 迭代器后移，同时返回下个元素
    void remove();     // 删除当前元素
}
```

```java
for (Iterator<E> it = myColl.iterator(); it.hasNext();) {
    System.out.println(it.next());
}
```

```java
for (Iterator<E> it = myColl.iterator(); it.hasNext();) {
    E e = it.next();
    if (judge(e)) {
        it.remove();
    }
}
```

**（4）for**：可修改。

```java
for (int i = 0; i < myColl.size(); i++) {
    System.out.println(myColl.get(i));
}
```

## List

```java
import java.util.List;
```

### ArrayList

```java
import java.util.ArrayList;
```

#### API

**构造函数**

- `ArrayList()`
- `ArrayList(int initialCapacity)`
- `ArrayList(Collection<? extends E> c)`

**增**

- `void	add(int index, E element)`
- `boolean add(E e)`
- `boolean addAll(int index, Collection<? extends E> c)`
- `boolean addAll(Collection<? extends E> c)`

**删**

- `E remove(int index)`
- `boolean remove(Object o)`：删除第一个出现的
- `boolean removeAll(Collection<?> c)`
- `void	clear()`

```java
for (int i = 0; i < list.size(); i++) {
    if (list.get(i) == target) {
        list.remove(i);
        i--; // 集合删除元素后，后面元素整体前移一位。
    }
}

for (Iterator<Integer> it = list.iterator(); it.hasNext();) {
    int val = it.next();
    if (val == target) {
        it.remove();
    }
}
```

**改**

- `E set(int index, E element)`

**查**

- `E get(int index)`
- `boolean contains(Object o)`

**大小**

- `int size()`
- `boolean isEmpty()`

#### 初始化

```java
// 初始化1
List<Integer> list1 = new ArrayList<>();
list1.add(1);
// 初始化2，推荐
List<Integer> list2 = new ArrayList<>() {{
    add(1);
}};
// 初始化3，不能修改
List<Integer> list3 = Arrays.asList(1, 2, 3);
// 初始化4，不能修改
List<Integer> list4 = List.of(1, 2, 3);
// 初始化5，推荐
List<Integer> list5 = Stream.of(1, 2, 3).collect(Collectors.toList());
// 初始化6，推荐
List<Integer> list6 = new ArrayList<>(List.of(1, 2, 3));
```

#### 去重

```java
List<Integer> list1 = new ArrayList<>(List.of(1, 2, 2, 3, 2));
// 去重1
LinkedHashSet<Integer> deDuplicator = new LinkedHashSet<>(list1);
List<Integer> list2 = new ArrayList<>(deDuplicator);
// 去重2
List<Integer> list3 = list1.stream().distinct().collect(Collectors.toList());
// 去重3
List<Integer> list4 = new ArrayList<>();
HashSet<Integer> hashset = new HashSet<>();
for (int x : list1) {
    if (hashset.add(x)) {
        list4.add(x);
    }
}
```

#### 源码

{{<admonition tip "源码" false>}}
```java
public class ArrayList<E> extends AbstractList<E> implements List<E>, RandomAccess, Cloneable, java.io.Serializable {

    // AbstractList<E>
    protected transient int modCount = 0;

    // 默认容量
    private static final int DEFAULT_CAPACITY = 10;

    private static final Object[] EMPTY_ELEMENTDATA = {};

    private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};

    transient Object[] elementData;

    private int size;

    public ArrayList(int initialCapacity) {
        if (initialCapacity > 0) {
            this.elementData = new Object[initialCapacity];
        } else if (initialCapacity == 0) {
            this.elementData = EMPTY_ELEMENTDATA;
        } else {
            throw new IllegalArgumentException("Illegal Capacity: " + initialCapacity);
        }
    }

    public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }

    public boolean add(E e) {
        modCount++;
        add(e, elementData, size);
        return true;
    }

    private void add(E e, Object[] elementData, int s) {
        if (s == elementData.length)
            elementData = grow();
        elementData[s] = e;
        size = s + 1;
    }

    private Object[] grow() {
        return grow(size + 1);
    }

    private Object[] grow(int minCapacity) {
        return elementData = Arrays.copyOf(elementData, newCapacity(minCapacity));
    }

    private int newCapacity(int minCapacity) {
        int oldCapacity = elementData.length;
        // 扩容为原大小的 1.5 倍
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        if (newCapacity - minCapacity <= 0) {
            // 如果初始化时未指定大小
            if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA)
                // 默认大小为 10
                return Math.max(DEFAULT_CAPACITY, minCapacity);
            if (minCapacity < 0)
                throw new OutOfMemoryError();
            return minCapacity;
        }
        return (newCapacity - MAX_ARRAY_SIZE <= 0) ? newCapacity : hugeCapacity(minCapacity);
    }

    private static int hugeCapacity(int minCapacity) {
        if (minCapacity < 0) // overflow
            throw new OutOfMemoryError();
        return (minCapacity > MAX_ARRAY_SIZE) ? Integer.MAX_VALUE : MAX_ARRAY_SIZE;
    }

    public <T> T[] toArray(T[] a) {
        if (a.length < size)
            return (T[]) Arrays.copyOf(elementData, size, a.getClass());
        System.arraycopy(elementData, 0, a, 0, size);
        if (a.length > size)
            a[size] = null;
        return a;
    }
}
```
{{< /admonition >}}

### LinkedList

```java
import java.util.LinkedList;
```

#### API

**构造方法**

```java

```

**增**

```java
// 尾部添加
boolean add​(E e)        // 一般 List 使用
boolean offer​(E e)      // 一般 Queue 使用
void addLast​(E e)       // 一般 LinkedList 使用
boolean offerLast​(E e)  // 一般 Deque 使用

// 首部添加
void addFirst​(E e)      // 一般 LinkedList 使用
boolean offerFirst​(E e) // 一般 Deque 使用
void push​(E e)          // 一般 栈 使用

// 指定位置添加
void add​(int index, E element)

// 添加集合
boolean addAll​(Collection<? extends E> c)
boolean addAll​(int index, Collection<? extends E> c)
```

**删**

```java
// 尾部删除
E removeLast()  // 一般 LinkedList 使用
E pollLast()    // 一般 Deque 使用

// 首部删除
E remove()      // 一般 List 使用
E poll()        // 一般 Queue 使用
E removeFirst() // 一般 LinkedList 使用
E pollFirst()   // 一般 Deque 使用
E pop()         // 一般 栈 使用

// 指定位置删除
E remove​(int index)

// 指定元素删除
boolean remove​(Object o)
boolean removeFirstOccurrence​(Object o)
boolean removeLastOccurrence​(Object o)

// 清空
void clear()
```

**改**

```java
E set​(int index, E element)
```

**查**

```java
// 尾部
E getLast()   // 一般 List 使用
E peekLast()  // 一般 List 使用

// 指定位置
E get​(int index)

// 首部
E getFirst()  // 一般 List 使用
E peekFirst() // 一般 List 使用
E peek()      // 一般 Queue 使用

boolean contains​(Object o)
int indexOf​(Object o)
int lastIndexOf​(Object o)
```

**其他**

```java
ListIterator<E> listIterator​(int index)
Iterator<E> descendingIterator()
```

### 4.4 遍历

**ListIterator**：可修改，双向迭代。

```java
// 列表迭代器定义
public interface ListIterator<E> extends Iterator<E> {
    boolean hasNext();
    E next();
    int nextIndex(); // 返回下个元素下标

    boolean hasPrevious();
    E previous();
    int previousIndex();

    void remove();
    void set(E e); // 修改当前迭代器指向元素
    void add(E e); // 添加到当前迭代器指向元素之前（next 之前，previous 之后），能被
}
```

> 迭代器总是位于元素之间，它并不指向某个元素，而是指向元素之间的间隔。
> <br>
> 所以，`listIterator(int index)`可以接受的参数范围为`[0, myList.size()]`
> <br>
> `listIterator()`返回的迭代器初始位于第一个元素之前。
> <br>
> `listIterator()`返回的迭代器初始位于第一个元素之前。

```java
// 正向
for (ListIterator<E> it = myList.listIterator(); it.hasNext();) {
    E e = it.next();
}

// 逆向
for (ListIterator<E> it = myList.listIterator(myList.size()); it.hasPrevious();) {
    E e = it.previous();
}
```

### List 和数组转换

#### List 转数组

```java
List<Integer> list = List.of(1, 2, 3);
// 方法1
Integer[] array1 = list.toArray(new Integer[0]);
// 方法2
Integer[] array2 = list.toArray(Integer[]::new);
// 不能转为 int[]
```

#### 数组转 List

```java
// int[] 不能转换
Integer[] array = { 1, 2, 3 };
List<Integer> list = List.of(array);
// 转换后的 list 只读，不能增删或修改
```

## Queue

```java
// 定义
Queue<Integer> queue = new LinkedList<>();
// 向队尾添加元素
queue.offer(1);
// 删除队首元素并返回
int first1 = queue.poll();
// 取队首元素
int first2 = queue.peek();
```

### Deque

```java
// 定义
Deque<Integer> deque = new LinkedList<>();
// 队尾添加
deque.offerLast(1);
// 队首添加
deque.offerFirst(1);
// 队尾删除并返回
int last1 = deque.pollLast();
// 队首删除并返回
int first1 = deque.pollFirst();
// 取队尾元素
int last2 = deque.peekLast();
// 取队首元素
int first2 = deque.peekFirst();
```

### PriorityQueue

优先队列，使用堆实现。

#### 基本用法

```java
import java.util.Comparator;
import java.util.PriorityQueue;
import java.util.Queue;

// 默认最小堆，元素值小的优先
Queue<Integer> q = new PriorityQueue<>();
// 自定义最大堆，元素值大的优先
Queue<Integer> q1 = new PriorityQueue<>((a, b) -> b - a);
q.offer(1);
int top = q.peek();
while (!q.isEmpty()) {
    int x = q.poll();
}
q.clear();
q.size();
```

#### 自定义类

```java
// 自定义类
class Stu {
    String name;
    int score;
}
```

##### 模板

```java
class A implements Comparable<A> {
    @Override
    public int compareTo(A a) {
        // ...
    }
}
```

```java
import java.util.Comparator;

class A implements Comparator<A> {
    @Override
    public int compare(A a, A b) {
        // ...
    }
}
```

##### 具体实现

```java
import java.util.Comparator;
import java.util.PriorityQueue;
import java.util.Queue;
```

**方法1**

```java
// 定义1
Queue<Stu> q1 = new PriorityQueue<>(new StuComp<Stu>());
// 显式定义排序类
class StuComp implements Comparator<Stu> {
    @Override
    public int compare(Stu s1, Stu s2) {
        if (s1.score != s2.score) {
            // 成绩高的优先
            return s2.score - s1.score;
        } else {
            // 再按名字字母排序
            return s1.name.compareTo(s2.name);
        }
    }
}
```

**方法2**

```java
// 定义2: 匿名类隐式定义排序类
Queue<Stu> q1 = new PriorityQueue<>(new Comparator<Stu>() {
    @Override
    public int compare(Stu s1, Stu s2) {
        if (s1.score != s2.score) {
            // 成绩高的优先
            return s2.score - s1.score;
        } else {
            // 再按名字字母排序
            return s1.name.compareTo(s2.name);
        }
    }
});
```

**方法3**

```java
// 定义3: 修改类定义
class Stu implements Comparator<Stu> {
    String name;
    int score;

    @Override
    public int compare(Stu s1, Stu s2) {
        if (s1.score != s2.score) {
            // 成绩高的优先
            return s2.score - s1.score;
        } else {
            // 再按名字字母排序
            return s1.name.compareTo(s2.name);
        }
    }
}
```

## Set

### HashSet

- 元素唯一，不可重复
- 元素乱序
- 数组+哈希函数实现，性能最优

```java
import java.util.HashSet;

HashSet<Integer> set = new HashSet<>();
set.add(1);
set.contains(1);
set.remove(1);
set.clear();
set.size();
set.isEmpty();
```

### TreeSet

- 元素唯一，不可重复
- 元素有序（默认升序）
- 红黑树实现，性能最差

```java
import java.util.TreeSet;

TreeSet<Integer> set = new TreeSet<>();
TreeSet<Integer> set2 = new TreeSet<>();
set2.addAll(Set.of(1, 3, 5));
```

#### 增

```java
set.add(1);       // [1]
set.addAll(set2); // [1, 3, 5]
```

#### 查

```java
set.contains(1);             // true
set.first();                 // 1
set.last();                  // 5
```

```java
// 第一个大于等于指定值的元素，不存在返回 null
Integer a1 = set.ceiling(0);  // 1
Integer a2 = set.ceiling(1);  // 1
Integer a3 = set.ceiling(4);  // 5
Integer a4 = set.ceiling(7);  // null
// 第一个大于指定值的元素，不存在返回 null
Integer a5 = set.higher(0);  // 1
Integer a6 = set.higher(1);  // 3
Integer a7 = set.higher(4);  // 5
Integer a8 = set.higher(7);  // null
```

```java
// 第一个小于等于指定值的元素，不存在返回 null
Integer a9 = set.floor(0);    // null
Integer a10 = set.floor(1);   // 1
Integer a11 = set.floor(4);   // 3
Integer a12 = set.floor(7);   // 5
// 第一个小于指定值的元素，不存在返回 null
Integer a13 = set.lower(0);   // null
Integer a14 = set.lower(1);   // null
Integer a15 = set.lower(4);   // 3
Integer a16 = set.lower(7);   // 5
```

#### 删

```java
set.pollFirst(); // 1
set.pollLast();  // 5
set.remove(3);   // true
set.remove(4);   // false
set.clear();
```

```java
set.size();
set.isEmpty();
```

#### 子集

```java
TreeSet<Integer> set = new TreeSet<>();
set.addAll(Set.of(1, 2, 3));                       // [1, 2, 3]
// 范围子集
Set<Integer> set1 = set.subSet(1, 2);              // [1]
Set<Integer> set2 = set.subSet(1, false, 2, true); // [2]
// 首部子集
Set<Integer> set3 = set.tailSet(2);                // [1]
Set<Integer> set4 = set.tailSet(2, true);          // [1, 2]
// 尾部子集
Set<Integer> set3 = set.tailSet(2);                // [2, 3]
Set<Integer> set4 = set.tailSet(2, false);         // [3]
// 逆序
Set<Integer> set7 = set.descendingSet();           // [3, 2, 1]
```

#### 遍历

```java
// foreach
for (int x : set) {
    //
}
// foreach 逆序
for (int x : set.descendingSet()) {
    //
}
// iterator
for (Iterator<Integer> it = set.iterator(); it.hasNext();) {
    it.next();
}
// iterator 逆序
for (Iterator<Integer> it = set.descendingIterator(); it.hasNext();) {
    it.next();
}
```

## Map

<img src="/Java-Collections-Framework-教程/Map家族.svg" alt="Map家族">

### HashMap

- 无序键值对，键不重复。

```java
import java.util.HashMap;
import java.util.Map;
```

```java
// 定义
Map<String, Integer> map = new HashMap<>();
Map<String, Integer> otherMap = new HashMap<>();
```

#### 增/改

```java
// 添加元素，始终覆盖原值
map.put("Alice", 80);
// 存在返回原值，否则添加该元素，返回 null
map.putIfAbsent("Alice", 60); // 80
// 添加所有元素，始终覆盖原值
map.putAll(otherMap);
// 自增 map[key]++
map.put("Alice", map.getOrDefault("Alice", 0) + 1); // 81
```

#### 查

```java
// 根据键查询值，不存在返回 null
map.get("Alice"); // 81
// 存在返回原值，否则返回指定值
map.getOrDefault("Bob", 0);
// 存在返回原值，否则添加该元素，并返回新值
map.computeIfAbsent("Alice", key -> 90); // 81
map.computeIfAbsent("Bob", key -> 90);   // 90
// 修改已存在的值，并返回新值
map.computeIfPresent("Alice", (key, value) -> value + 10); // 91
// 查询键
map.containsKey("Bob"); // true
```

#### 删

```java
// 删除
map.remove("Alice");
// 清空
map.clear();
```

```java
// 是否为空
map.isEmpty();
// 大小
map.size();
```

#### 遍历

```java
// 遍历1
for (String key : map.keySet()) {
    int value = map.get(key);
}
// 遍历2
for (Map.Entry<String, Integer> ent : map.entrySet()) {
    String key = ent.getKey();
    int value = ent.getValue();
}
// 只遍历 value
for (int value : map.values()) {
}
```

### TreeMap

- 有序键值对，键不重复。
- 放入的 Key 必须实现`Comparable`接口。

```java
import java.util.Map;
import java.util.TreeMap;
```

#### 类实现 Comparable 接口

```java
class Stu implements Comparable<Stu> {
    String name;
    int score;

    @Override
    public int compareTo(Stu s) {
        // x.compareTo(y) < 0 等价于 x < y
        if (score != s.score) {
            return s.score - score;
        } else {
            return name.compareTo(s.name);
        }
    }
}

Map<Stu, Integer> map = new TreeMap<>();
```

#### 自定义 Comparator 比较器

```java
import java.util.Comparator;

class Stu {
    String name;
    int score;

    String getName() {
        return name;
    }

    int getScore() {
        return score;
    }
}
```

```java
// 实现1 匿名类
Map<Stu, Integer> map1 = new TreeMap<>(new Comparator<Stu>() {
    @Override
    public int compare(Stu a, Stu b) {
        // compare(x, y) < 0 等价于 x < y
        if (a.score != b.score) {
            return b.score - a.score;
        } else {
            return a.name.compareTo(b.name);
        }
    }
});
```

```java
// 实现2 lambda 表达式
Comparator<Stu> cmp = (Stu a, Stu b) -> a.score != b.score ? b.score - a.score : a.name.compareTo(b.name);
Map<Stu, Integer> map2 = new TreeMap<>(cmp);
```

```java
// 实现3 lambda 表达式
Map<Stu, Integer> map3 = new TreeMap<>((Stu a, Stu b) -> a.score != b.score ? b.score - a.score : a.name.compareTo(b.name));
```

```java
// 实现4 Comparator 类方法
Map<Stu, Integer> map4 = new TreeMap<>(Comparator.comparing(Stu::getScore).reversed().thenComparing(Stu::getName));
```

## Arrays

```java
import java.util.Arrays;
```

### 二分查找

查找指定值，返回下标，若未找到则返回应该插入的下标的相反数减一。需要**数组升序且元素无重复**。

- `static <T> int binarySearch(T[] a, T key)`
- `static <T> int binarySearch(T[] a, int fromIndex, int toIndex, T key)`
- `static <T> int binarySearch​(T[] a, T key, Comparator<? super T> c)`
- `static <T> int binarySearch​(T[] a, int fromIndex, int toIndex, T key, Comparator<? super T> c)`

```java
int[] arr = {1, 3, 5};
Arrays.binarySearch(arr, -1); // -1
Arrays.binarySearch(arr, 0);  // -1
Arrays.binarySearch(arr, 1);  // 0
Arrays.binarySearch(arr, 2);  // -2
Arrays.binarySearch(arr, -4); // -1
Arrays.binarySearch(arr, 8);  // -4
```

### 比较

a 小于 b 返回 -1，等于返回 0，大于返回 1。

- `static <T> int compare(T[] a, T[] b)`
- `static <T> int compare​(T[] a, int aFromIndex, int aToIndex, T[] b, int bFromIndex, int bToIndex)`
- `static <T> int compare​(T[] a, T[] b, Comparator<? super T> cmp)`
- `static <T> int compare​(T[] a, int aFromIndex, int aToIndex, T[] b, int bFromIndex, int bToIndex, Comparator<? super T> cmp)`

相等 true，不相等 false。

- `static <T> boolean equals​(T[] a, T[] b)`
- `static <T> boolean equals​(T[] a, T[] b, Comparator<? super T> cmp)`
- `static <T> boolean equals​(T[] a, int aFromIndex, int aToIndex, T[] b, int bFromIndex, int bToIndex)`
- `static <T> boolean equals​(T[] a, int aFromIndex, int aToIndex, T[] b, int bFromIndex, int bToIndex, Comparator<? super T> cmp)`

比较多维数组

- `static boolean deepEquals​(Object[] a1, Object[] a2)`

```java
int[][] a = { { 1, 2 }, { 3, 4 } };
int[][] b = { { 1, 2 }, { 3, 4 } };
System.out.println(a == b);                  // false
System.out.println(a.equals(b));             // false
System.out.println(Arrays.deepEquals(a, b)); // true
```

返回第一个不相等的下标，若全相等，返回 -1。

- `static <T> int mismatch​(T[] a, T[] b)`
- `static <T> int mismatch​(T[] a, int aFromIndex, int aToIndex, T[] b, int bFromIndex, int bToIndex)`
- `static <T> int mismatch​(T[] a, T[] b, Comparator<? super T> cmp)`
- `static <T> int mismatch​(T[] a, int aFromIndex, int aToIndex, T[] b, int bFromIndex, int bToIndex, Comparator<? super T> cmp)`

### 复制

新数组长度小于原数组则截断，大于则多余元素为 null/0。

- `static <T> T[] copyOf​(T[] original, int newLength)`
- `static <T,​U> T[] copyOf​(U[] original, int newLength, Class<? extends T[]> newType)`
- `static <T> T[] copyOfRange​(T[] original, int from, int to)`
- `static <T,​U> T[] copyOfRange​(U[] original, int from, int to, Class<? extends T[]> newType)`

### 赋值

- `static <T> void fill(T[] a, T val)`
- `static <T> void fill(T[] a, int fromIndex, int toIndex, T val)`

### 前缀操作

- `static <T> void parallelPrefix​(T[] array, BinaryOperator<T> op)`
- `static <T> void parallelPrefix​(T[] array, int fromIndex, int toIndex, BinaryOperator<T> op)`

```java
int[] a = { 1, 2, 3 };
// 前缀和
Arrays.parallelPrefix(a, (x, y) -> x + y); // [1, 3, 6]
```

### 排序

- `static <T> void sort​(T[] a)`
- `static <T> void sort​(T[] a, Comparator<? super T> c)`
- `static <T> void sort​(T[] a, int fromIndex, int toIndex)`
- `static <T> void sort​(T[] a, int fromIndex, int toIndex, Comparator<? super T> c)`

### 流

- `static <T> Stream<T> stream​(T[] array)`
- `static <T> Stream<T> stream​(T[] array, int startInclusive, int endExclusive)`

### 转为字符串

- `static <T> String toString(T[] a)`
- `static String deepToString​(Object[] a)`：可用于打印多维数组。

```java
int[][] a = { { 1, 2 }, { 3, 4 } };
int[][] b = { { 1, 2 }, { 3, 4 } };
System.out.println(a);                       // [[I@36aa7bc2
System.out.println(Arrays.toString(a));      // [[I@76ccd017, [I@182decdb]
System.out.println(Arrays.toString(a[0]));   // [1, 2]
System.out.println(Arrays.deepToString(a));  // [[1, 2], [3, 4]]
```

## Collections

```java
import java.util.Collections;
```

```java
// 创建空集合
// 返回的集合是不可变集合，无法向其中添加或删除元素。
List<String> list1 = Collections.emptyList();
List<String> list2 = List.of();
Map<String, Integer> map1 = Collections.emptyMap();
Map<String, Integer> map2 = Map.of();
Set<String> set1 = Collections.emptySet();
Set<String> set2 = Set.of();
```

```java
// 创建单元素集合
// 返回的集合是不可变集合，无法向其中添加或删除元素。
List<String> list1 = Collections.singletonList("one");
List<String> list2 = List.of("one");
Map<String, Integer> map1 = Collections.singletonMap("one", 1);
Map<String, Integer> map2 = Map.of("one", 1);
Set<String> set1 = Collections.singleton("one");
Set<String> set2 = Set.of("one");
```

```java
// 排序
// 必须传入可变 List
Collections.sort(list);

// 洗牌
// 必须传入可变 List
Collections.shuffle(list);
```

## 10.代码片段

```java
// 数组转 List
// 1.重新分配空间
List<Integer> myList = new ArrayList<>(Arrays.asList(1, 2, 3));
// 2.使用同一片空间
List<Integer> myList = Arrays.asList(1, 2, 3);
// 禁止调整大小，若 add 或 remove
// 则抛出 UnsupportedOperationException 异常
```

```java
// 转换为字符串
String str = myColl.stream()
    .map(Object::toString)
    .collect(Collectors.joining(", "));
```

```java
// 求和
int sum = myColl.stream()
    .collect(Collectors.summingInt(E::getValue));
```

```java
// 删除容器中所有指定元素
myColl.removeAll(Collections.singleton(e));
myColl.removeAll(Collections.singleton(mull));
```

```java
// 容器转数组，要求容器中元素类型统一
String[] arrStr = myColl.toArray(new String[0]);
Integer[] arrInt = myColl.toArray(new Integer[0]);
```

```java
// 1.容器去重
Collection<E> noDups = new HashSet<>(myColl);
Collection<E> noDups = myColl.stream().collect(Collectors.toSet());
// 2.容器去重，且保持原来顺序
Collection<E> noDups = new LinkedHashSet<>(myColl);

public static <E> Set<E> removeDups(Collection<E> myColl) {
    return new LinkedHashSet<E>(myColl);
}
```

```java
// 取出属性值，创建集合
Set<String> set = people.stream()
    .map(Person::getName)
    .collect(Collectors.toCollection(TreeSet::new));
```

```java
// 交换元素
public static <E> void swap(List<E> a, int i, int j) {
    E tmp = a.get(i);
    a.set(i, a.get(j));
    a.set(j, tmp);
}
```

```java
// 洗牌算法，打乱顺序
public static void shuffle(List<E> list, Random rnd) {
    for (int i = list.size(); i > 1; i--) {
        swap(list, i - 1, rnd.nextInt(i));
    }
}
```

### char[] 转 Character[]

```java
char[] charArray = { 'a', 'b', 'c' };
Character[] charObjectArray = ArrayUtils.toObject(charArray);
```

## 11.聚合操作

## 重写 equals 方法

- 对引用类型用`Objects.equals()`比较（避免判断`null`），对基本类型直接用`==`比较。

```java
import java.util.Objects;

public boolean equals(Object o) {
    if (o instanceof E) {
        E e = (E) o;
        //
    }
    return false;
}
```

## 重写 hashCode 方法

- 如果两个对象不相等，则两个对象的`hashCode()`尽量不要相等。

```java
import java.util.Objects;

class Stu {
    String name;
    int score, age;

    public int hashCode() {
        return Objects.hash(name, score, age);
    }
}
```

参考：

1. [Aggregate Operations - Java Tutorials](https://docs.oracle.com/javase/tutorial/collections/streams/index.html)

## Streams

Java 8 中引入。

```java
import java.util.stream.Collectors;
import java.util.stream.Stream;
```

```java
// 获取流方法1
int[] array = { 1, 2, 3 };
Stream<Integer> s1 = Stream.of(array);
// 获取流方法2
List<Integer> list = List.of(1, 2, 3);
Stream<Integer> s2 = list.stream();
// 获取流方法3
Stream<Integer> s3 = Stream.of(1, 2, 3);
```

```java
// forEach 终端操作
List<Integer> list = List.of(1, 2, 3);
list.stream().forEach(e -> System.out.println(e));
list.stream().forEach(System.out::println);
// map 中间操作
List<String> list = list.stream().map(e -> e.toString()).collect(Collectors.toList());
// collect 终端操作
List<Integer> list = Stream.of(1, 2, 3).collect(Collectors.toList());
String str = list.stream().map(e -> e.toString()).collect(Collectors.joining(", "));
Set<Integer> set = list.stream().collect(Collectors.toSet());
// filter 中间操作
List<Integer> list = Stream.of(1, 2, 3).filter(e -> e < 2).collect(Collectors.toList());
// findFirst
Integer x = Stream.of(1, 2, 3).filter(e -> e % 2 == 1).findFirst().orElse(null);
// toArray
Integer[] array = list.stream().toArray(Integer[]::new);
// flatMap 中间操作
List<List<String>> namesNested = Arrays.asList(
        Arrays.asList("Jeff", "Bezos"),
        Arrays.asList("Bill", "Gates"),
        Arrays.asList("Mark", "Zuckerberg"));
List<String> namesFlatStream = namesNested.stream()
        .flatMap(Collection::stream)
        .collect(Collectors.toList());
// count 终端操作
long a = list.stream().count();
//
Stream<Integer> infiniteStream = Stream.iterate(2, i -> i * 2);
List<Integer> collect = infiniteStream
    .skip(3)  // 跳过前 3 个元素
    .limit(5) // 长度为 5
    .collect(Collectors.toList());
// sorted
list.stream().sorted((a, b) -> b - a);
// min max
Integer x = list.stream().min((a, b) -> a - b).orElse(null);
Integer x = list.stream().max((a, b) -> a - b).orElse(null);
// distinct
List<Integer> newList = list.stream().distinct().collect(Collectors.toList());
// allMatch anyMatch noneMatch
boolean allEven = list.stream().allMatch(i -> i % 2 == 0);
boolean oneEven = list.stream().anyMatch(i -> i % 2 == 0);
boolean noneMultipleOfThree = list.stream().noneMatch(i -> i % 3 == 0);
```

```java
// IntStream
list.stream().mapToInt(e -> e + 1);
IntStream.of(1, 2, 3);
// range
IntStream.range(1, 4);
// sum
int sum = list.stream().mapToInt(e -> e).sum();
// averge
double avg = list.stream().mapToInt(e -> e).average().orElse(0);
// reduce
int sum = list.stream().mapToInt(e -> e).reduce(0, Integer::sum);
int product = list.stream().mapToInt(e -> e).reduce(1, (a, b) -> a * b);
```

```java
DoubleSummaryStatistics stats = list.stream().mapToDouble(e -> (double) e).summaryStatistics();
DoubleSummaryStatistics stats = list.stream().collect(Collectors.summarizingDouble(e -> (double) e));
stats.getCount();
stats.getSum();
stats.getAverage();
stats.getMin();
stats.getMax();
```

```java
Map<Boolean, List<Integer>> isEven = list.stream().collect(Collectors.partitioningBy(i -> i % 2 == 0));
```

```java
Stream.generate(Math::random).limit(5).forEach(System.out::println);
```
