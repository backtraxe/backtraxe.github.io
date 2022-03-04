# Java Collections Framework 教程


Collection 是用于表示和操作集合的统一体系结构。

<!--more-->

## 1.简介

所有 Collection 都包含以下内容：

- **接口（Interfaces）**：这些是表示集合的抽象数据类型。接口允许集合独立于其表示的细节进行操作。在面向对象语言中，接口通常形成一个层次结构。
- **实现（Implementations）**：这些是集合接口的具体实现。本质上，它们是可重用的数据结构。
- **算法（Algorithms）**：这些方法对实现集合接口的对象执行有用的计算，如搜索和排序。这些算法被认为是多态的：也就是说，相同的方法可以用于相应集合接口的许多不同实现。本质上，算法是可重用的功能。

{{< mermaid >}}classDiagram
    Collection <|-- Set
    Collection <|-- List
    Collection <|-- Queue
    Collection <|-- Deque
    Set <|-- SortedSet
    Map <|-- SortedMap
{{< /mermaid >}}

上图描述了核心 Collection 接口：

- **Collection**：父类，提供统一接口。
- **Set**：集合；元素无序；元素唯一。
- **List**：数组；元素有序；索引访问。
- **Queue**：队列；FIFO（除了优先级队列）。
- **Deque**：双向队列。
- **Map**：键值对；键无序；键唯一；一一映射。
- **SortedSet**：有序集合；默认升序；元素唯一。
- **SortedMap**：有序键值对；默认升序；键唯一；一一映射。

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

## 3.Set

### 3.1 特性

- 继承 `Collection`，可使用其所有方法
- 元素唯一

### 3.2 实现

- `HashSet`：哈希表实现；性能最佳；元素无序
- `TreeSet`：红黑树实现；性能最差；元素有序（默认升序）
- `LinkedHashSet`：哈希表+链表实现；性能第二；元素按添加顺序排序

## 4.List

### ArrayList

```java
import java.util.ArrayList;
import java.util.List;
```

```java
// 定义
List<Integer> list = new ArrayList<>(); // 数组实现
// 遍历1
for (int i = 0; i < list.size(); i++) {
    int x = list.get(i);
}
// 遍历2
import java.util.Iterator;
for (Iterator<Integer> it = list.iterator(); it.hasNext();) {
    int x = it.next();
}
// 遍历3
for (int x : list) {
    //
}
```

### LinkedList

```java
import java.util.LinkedList;
import java.util.List;
```

```java
// 定义
List<Integer> b = new LinkedList<>(); // 链表实现
List<Integer> c = List.of(1, 2, 3);
```

### 4.3 方法

- `E get(int index)`：返回指定索引的元素
- `E set(int index, E element)`：修改指定索引的元素，返回原来的元素
- `boolean add(E element)`：添加到尾部
- `E remove(Object element)`：只删除第一个匹配到的元素，返回删除的元素
- `int indexOf(E element)`：返回第一个匹配到的元素的索引，失败返回 -1
- `int lastIndexOf(E element)`：返回最后一个匹配到的元素的索引，失败返回 -1
- `boolean containsAll(Collection<?> c)`：先将容器 c 转换为集合，再判断；只关注元素的值，不关注数量
- `boolean addAll(Collection<? extends E> c)`：添加到尾部
- `boolean removeAll(Collection<?> c)`：删除所有与容器 c 中具有相同值的元素；先将容器 c 转换为集合，再删除所有匹配项
- `boolean retainAll(Collection<?> c)`：删除所有与容器 c 中具有不同值的元素；先将容器 c 转换为集合，再删除所有匹配项
- `ListIterator<E> listIterator()`：返回列表迭代器，可双向迭代
- `ListIterator<E> listIterator(int index)`：返回指定索引的列表迭代器，可双向迭代

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
Queue<Integer> q1 = new PriorityQueue<>(new Comparator<Integer>() {
    public int compare(Integer a, Integer b) {
        return b - a;
        // 或者
        // return b.compareTo(a);
    }
});
// 添加
q.offer(1);
// 遍历
while (!q.isEmpty()) {
    int x = q.peek();
    q.poll();
}
// 清空
q.clear();
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
import java.util.Comparable;

class A implements Comparable<A> {
    @override
    public int compareTo(A a) {
        // ...
    }
}
```

```java
import java.util.Comparator;

class A implements Comparator<A> {
    @override
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

## 6.Deque

## Map

### HashMap

- 无序键值对，键不重复。

```java
import java.util.HashMap;
import java.util.Map;
```

```java
// 定义
Map<String, Integer> map = new HashMap<>();
// 添加
map.put("Alice", 80);
// 查询值
int score = map.get("Alice");
// 查询键
boolean isExist = map.containsKey("Bob");
// 遍历1
for (String key : map.keySet()) {
    int value = map.get(key);
}
// 遍历2
for (Map.Entry<String, Integer> ent : map.entrySet()) {
    String key = ent.getKey();
    int value = ent.getValue();
}
```

### TreeMap

- 有序键值对，键不重复。
- 放入的 Key 必须实现`Comparable`接口。

```java
import java.util.Map;
import java.util.TreeMap;
```

```java
// 类实现接口
class Stu implements Comparable<Stu> {
    String name;
    int score;

    public int compareTo(Stu s) {
        if (score != s.score) {
            return s.score - score;
        } else {
            return name.compareTo(s.name);
        }
    }
}
// 类未实现接口，手动实现
Map<Stu, Integer> map = new TreeMap<>(new Comparator<Stu>() {
    public int compare(Stu a, Stu b) {
        if (a.score != b.score) {
            return b.score - a.score;
        } else {
            return a.name.compareTo(b.name);
        }
    }
});
```

## 8.SortedSet

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

## ## 重写 hashCode 方法

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

