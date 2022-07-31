# 竞赛编程


<!--more-->

## C++

## Java

### I/O

读取数据

```java
Scanner in = new Scanner(System.in);
while (in.hasNext()) {
    String s = in.next();
    int i = in.nextInt();
    long l = in.nextLong();
    double d = in.nextDouble();
    char c = in.nextChar();
    byte b = in.nextByte();
}
```

打印数据

```java
System.out.println();
System.out.print();
System.out.printf("%.2f", Math.PI); // 3.14
```

重定向

```bash
// 从 input.txt 读取输入并输出到 output.txt 中
java Main < input.txt > output.txt

# 将 Main1 的输出作为 Main2 的输入
java Main1 | java Main2
```

命令行参数

```java
class Main {
    public static void main(String[] args) {
        // java Main 1 two
        String a = args[0]; // "1"
        String b = args[1]; // "two"
    }
}
```

### 字符串

```java
// 字符串拼接
s1 + s2
// 长度
int length()
// 下标为 i 的字符
int charAt(int i)
// p 第一次出现的下标，未找到返回 -1
int indexOf(String p)
// 从下标 i 开始，p 第一次出现的下标，未找到返回 -1
int indexOf(String p, int i)
// 子串 [i,j - 1]
String substring(int i, int j)
// 分割字符串，支持正则表达式
String[] split(String delim)
// 比较字符串，小于返回 -1，相等返回 0，大于返回 1
int compareTo(String t)
// 比较字符串
boolean equals(String t)
```

### 数组

```java
// 一维数组
int[] arr1 = new int[3];
int[] arr2 = { 1, 2, 3 };

// 二维数组
int[][] arr3 = new int[2][2];
int[][] arr4 = { { 1, 2 }, { 3, 4 } };

// 对象数组
User[] arr5 = new User[3];
for (int i = 0; i < 3; i++)
    arr5[i] = new User();
```

### 变长数组

```java
List<Integer> list = new ArrayList<>();
```

### 链表

```java
// 双向链表
List<Integer> list = new LinkedList<>();
```

### 栈

```java
// 数组实现
Deque<Integer> stack1 = new ArrayDeque<>();
// 链表实现
Deque<Integer> stack2 = new LinkedList<>();
```

API 1:

- `boolean offerLast(E e)`：向栈顶添加元素。
- `E pollLast()`：从栈顶删除元素并返回该元素，若栈为空返回 null。
- `E peekLast()`：返回栈顶元素，若栈为空返回 null。

API 2:

- `void push(E e)`：向栈顶添加元素。
- `E pop()`：从栈顶删除元素并返回该元素，若栈为空抛出`java.util.NoSuchElementException`。
- `E peek()`：返回栈顶元素，若栈为空返回 null。

### 队列

#### 普通队列

```java
// 数组实现
Queue<Integer> queue1 = new ArrayDeque<>();
// 链表实现
Queue<Integer> queue2 = new LinkedList<>();
```

- `boolean offer(E e)`：添加元素到队尾。
- `E poll()`：从队头删除元素并返回该元素，若队列为空返回 null。
- `E peek()`：返回队头元素，若队列为空返回 null。
- `int size()`：返回队列中元素数量。
- `boolean isEmpty()`：返回队列是否为空。

```java
// 遍历，不支持增强 for 循环和迭代器。
while (!queue.isEmpty()) {
    int e = queue.poll();
}
```

#### 双端队列

```java
// 数组实现
Deque<Integer> queue1 = new ArrayDeque<>();
// 链表实现
Deque<Integer> queue2 = new LinkedList<>();
```

- `boolean offerFirst(E e)`：队首添加元素。
- `boolean offerLast(E e)`：队尾添加元素。
- `E pollFirst()`：删除队首元素并返回，若队列为空返回 null。
- `E pollLast()`：删除队尾元素并返回，若队列为空返回 null。
- `E peekFirst()`：返回队首元素，若队列为空返回 null。
- `E peekLast()`：返回队尾元素，若队列为空返回 null。

### 哈希表

#### 无序哈希表

##### 初始化

```java
Map<String, Integer> map1 = new HashMap<>();

Map<String,Integer> map2 = new HashMap<>() {{
    put("one", 1);
    put("two", 2);
}};
```

##### 添加

- `V put​(K key, V value)`
    - key 存在：覆盖旧值，返回旧值。
    - key 不存在：添加元素，返回 null。

```java
map.put("one", 1);    // null {one=1}
map.put("one", 2);    // 1    {one=2}
map.put("one", null); // 2    {one=null}
map.put("two", null); // null {one=null, two=null}
map.put(null, 1);     // null {null=1, one=null, two=null}
map.put(null, 2);     // 1    {null=2, one=null, two=null}
```

- `V compute​(K key, BiFunction<? super K, ​? super V,​? extends V> remappingFunction)`
    - key 存在：若新值为 null 则删除元素，返回 null；若新值不为 null 则覆盖旧值，返回新值。
    - key 不存在：若新值为 null 则返回 null；若新值不为 null 则添加元素，返回新值。

```java
map.compute("one", (k, v) -> 1);    // 1    {one=1}
map.compute("two", (k, v) -> 2);    // 2    {one=1, two=2}
map.compute("two", (k, v) -> null); // null {one=1}
```

- `V putIfAbsent​(K key, V value)`
    - key 存在：返回旧值。
    - key 不存在：添加元素，返回 null。

```java
map.putIfAbsent("one", 10); // 1    {one=1}
map.putIfAbsent("two", 2);  // null {one=1, two=2}
```

- `V computeIfAbsent​(K key, Function<? super K,​? extends V> mappingFunction)`
    - key 存在：返回旧值。
    - key 不存在：若新值为 null 则返回 null；若新值不为 null 则添加元素，返回新值。

```java
map.computeIfAbsent("one", k -> 10);     // 1    {one=1, two=2}
map.computeIfAbsent("three", k -> null); // null {one=1, two=2}
map.computeIfAbsent("three", k -> 3);    // 3    {one=1, three=3, two=2}
```

- `V computeIfPresent​(K key, BiFunction<? super K,​? super V,​? extends V> remappingFunction)`
    - key 存在：覆盖旧值.
    - key 不存在：返回 null。

```java
map.computeIfPresent​("one", (k, v) -> 10);
map.computeIfPresent​("two", (k, v) -> null);
map.computeIfPresent​("two", (k, v) -> 2);
map.computeIfPresent​("four", (k, v) -> null);
```

- `void putAll​(Map<? extends K, ​? extends V> m)`：添加另一个 Map 中的所有元素。

##### 删除

- `V remove​(Object key)`：删除元素。key 存在则返回旧值；key 不存在则返回 null。
- `void clear()`：清空 Map。
- `boolean remove​(Object key, Object value)`：删除指定键值对。

##### 查询

- `V get​(Object key)`：key 存在返回对应的值；key 不存在返回 null。
- `V getOrDefault​(Object key, V defaultValue)`：key 存在返回对应的值；key 不存在返回 defaultValue。
- `boolean containsKey​(Object key)`：是否存在指定 key。
- `boolean containsValue​(Object value)`：是否存在指定 value。
- `int size()`：大小。
- `boolean isEmpty()`：是否为空。

##### 遍历

- `Set<K> keySet()`：返回所有 key 的集合（非副本）。该集合支持删除元素，不支持添加元素。
- `Set<Map.Entry<K,​V>> entrySet()`：返回所有键值对的集合（非副本）。该集合支持删除元素，不支持添加元素。
- `Collection<V> values()`：返回所有 value 的集合（非副本）。该集合支持删除元素，不支持添加元素。
- `void forEach​(BiConsumer<? super K,​? super V> action)`：增强 for 循环。

```java
// 方法1：keySet 增强 for 循环
for (String key : map.keySet()) {
    int val = map.get(key);
}

// 方法2：entrySet 增强 for 循环
// for (var entry : map.entrySet())
for (Map.Entry<String, Integer> entry : map.entrySet()) {
    String key = entry.getKey();
    int val = entry.getValue();
}

// 方法3：keySet 迭代器
Iterator<String> it1 = map.keySet().iterator();
while (it1.hasNext()) {
    String key = iterator.next();
    int val = map.get(key);
}

// 方法4：entrySet 迭代器
Iterator<Map.Entry<String, Integer>> it2 = map.entrySet().iterator();
while (it2.hasNext()) {
    Map.Entry<String, Integer> entry = it2.next();
    int key = entry.getKey();
    int val = entry.getValue();
}
```

速度：
- entrySet > keySet
- 迭代器 > 增强 for 循环

##### 排序

```java
List<Map.Entry<String, Integer>> list = new ArrayList<>(map.entrySet());
Collections.sort(list);
// 降序 lambda
Collections.sort(list, (a, b) -> b.getKey().compareTo(a.getKey()));
// 降序 Comparator
Collections.sort(list, new Comparator<Map.Entry<String, Integer>>() {
    @Override
    public int compare(Map.Entry<String, Integer> a, Map.Entry<String, Integer> b) {
        return b.getKey().compareTo(a.getKey());
    }
});
```

#### 有序哈希表

##### 初始化

- `TreeMap​(Comparator<? super K> comparator)`：自定义排序规则。

```java
// 降序
Map<String, Integer> map = new TreeMap<>((a, b) -> b.compareTo(a));
```

##### 查询

- `K floorKey(K key)`：返回最后一个小于等于 key 的键，若无返回 null。
- `K ceilingKey(K key)`：返回第一个大于等于 key 的键，若无返回 null。
- `K lowerKey(K key)`：返回最后一个小于 key 的键，若无返回 null。
- `K higherKey(K key)`：返回第一个大于 key 的键，若无返回 null。
- `K firstKey()`：返回第一个键，若空抛出`java.util.NoSuchElementException`。
- `K lastKey()`：返回最后一个键，若空抛出`java.util.NoSuchElementException`。
- `Map.Entry<K,​V> floorEntry(K key)`：返回最后一个小于等于 key 的键值对，若无返回 null。
- `Map.Entry<K,​V> ceilingEntry(K key)`：返回第一个大于等于 key 的键值对，若无返回 null。
- `Map.Entry<K,​V> higherEntry(K key)`：返回最后一个小于 key 的键值对，若无返回 null。
- `Map.Entry<K,​V> lowerEntry(K key)`：返回第一个大于 key 的键值对，若无返回 null。
- `Map.Entry<K,​V> firstEntry()`：返回第一个键值对，若空返回 null。
- `Map.Entry<K,​V> lastEntry()`：返回最后一个键值对，若空返回 null。

```java
// {five=5, four=4, one=1, three=3, two=2}
floorKey("one");   // one
floorKey("six");   // one
floorKey("aaa");   // null
floorKey("zzz");   // two
ceilingKey("one"); // one
ceilingKey("six"); // three
ceilingKey("aaa"); // five
ceilingKey("zzz"); // null
lowerKey("one");   // four
lowerKey("six");   // one
lowerKey("aaa");   // null
lowerKey("zzz");   // two
higherKey("one");  // three
higherKey("six");  // three
higherKey("aaa");  // five
higherKey("zzz");  // null
```

##### 遍历

- `NavigableSet<K> descendingKeySet()`
- `NavigableMap<K,​V> descendingMap()`

```java

```

### 集合

#### 无序集合

##### 初始化

##### 添加

##### 删除

##### 查询

##### 遍历

##### 排序

#### 有序集合

##### 初始化

- `TreeSet​(Collection<? extends E> c)`：使用其他容器进行初始化。
- `TreeSet​(Comparator<? super E> comparator)`：自定义排序规则。

```java
TreeSet<Integer> set1 = new TreeSet<>(List.of(2, 1, 1, 3, 2)); // [1, 2, 3]
TreeSet<Integer> set2 = new TreeSet<>((a, b) -> b - a);        // 降序
```

##### 查询

- `E floor(E e)`：返回最后一个小于等于 e 的元素，若无返回 null。
- `E ceiling(E e)`：返回第一个大于等于 e 的元素，若无返回 null。
- `E lower(E e)`：返回最后一个小于 e 的元素，若无返回 null。
- `E higher(E e)`：返回第一个大于 e 的元素，若无返回 null。
- `E first()`：返回第一个元素，若无抛出`java.util.NoSuchElementException`。
- `E last()`：返回最后一个元素，若无抛出`java.util.NoSuchElementException`。
- `E pollFirst()`：删除并返回第一个元素，若无返回 null。
- `E pollLast()`：删除并返回最后一个元素，若无返回 null。

使用同 [TreeMap](#查询-1)。

##### 遍历

- `Iterator<E> descendingIterator()`
- `NavigableSet<E> descendingSet()`

使用同 [TreeMap](#遍历-1)。

### 优先队列

### 常量

```java
Integer.MAX_VALUE == 2147483647;
Integer.MIN_VALUE == -2147483648;
Double.POSITIVE_INFINITY;
Double.NEGATIVE_INFINITY;
```

### 类型转换

```java
// String 转 int，不能转换抛出 java.lang.NumberFormatException
int i = Integer.parseInt("123");

// int 转 二进制字符串
String s = Integer.toBinaryString(123);
```

## 算法模板

### 数据规模

### getMin 功能的栈

1. 两个栈：一个存元素，一个存最小值。最小值可以同步存，也可以不同步。
2. 一个栈：捆绑元素和最小值。

```java
class MinStack {
    Deque<Integer> stackData = new ArrayDeque<>();
    Deque<Integer> stackMin = new ArrayDeque<>();

    public void push(int e) {
        stackData.push(e);
        if (stackMin.isEmpty()) stackMin.push(e);
        else stackMin.push(Math.min(e, getMin()));
    }

    public int pop() {
        if (stackData.isEmpty()) throw new Exception("栈中没有元素");
        stackMin.pop();
        return stackData.pop();
    }

    public int peek() {
        if (stackData.isEmpty()) throw new Exception("栈中没有元素");
        return stackData.peek();
    }

    public int getMin() {
        if (stackData.isEmpty()) throw new Exception("栈中没有元素");
        return stackMin.peek();
    }
}
```

### 两个栈组成队列

```java
class TwoStackQueue {
    Deque<Integer> stackIn = new ArrayDeque<>();
    Deque<Integer> stackOut = new ArrayDeque<>();

    public void offer(int e) {
        stackIn.push(e);
    }

    public int poll() {
        check();
        return stackOut.pop();
    }

    public int peek() {
        check();
        return stackOut.peek();
    }

    private void check() {
        if (stackOut.isEmpty() && stackIn.isEmpty())
            throw new Exception("队列中没有元素");
        if (stackOut.isEmpty())
            while (!stackIn.isEmpty())
                stackOut.push(stackIn.pop());
    }
}
```

### 用递归函数和栈逆序一个栈

```java
static void reverseStack(Deque<Integer> stack) {
    if (stack.isEmpty()) return;
    int bottom = getAndRemoveBottom(stack);
    reverseStack(stack);
    stack.push(bottom);
}

static int getAndRemoveBottom(Deque<Integer> stack) {
    int top = stack.poll();
    if (stack.isEmpty()) return top;
    else {
        int bottom = getAndRemoveBottom(stack);
        stack.push(top);
        return bottom;
    }
}
```

### 猫狗队列

```java

```
