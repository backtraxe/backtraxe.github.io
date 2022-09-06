# Java 基础


<!--more-->

## 容器

### ArrayList

#### 扩容机制

1. `ArrayList()`使用长度为 0 的数组，第一次扩容时至少为 10。
2. `ArrayList(int initialCapacity)`使用指定容量大小的数组。
3. `ArrayList(Collection<? extends E> c)`使用 c 的大小作为数组容量。
4. `add(E e)`首次扩容为 10（无参初始化），之后每次存满时扩容为当前容量的 1.5 倍，向下取整。
    - 使用无参构造方法初始化时，第一次扩容为`10`，之后每次扩容为`size + (size >> 1)`。
    - 使用有参构造方法初始化时，第一次扩容为`1`，之后每次扩容为`Math.max(size + (size >> 1), size + 1)`。
5. `addAll(Collection<? extends E> c)`：剩余空间不够时，扩容为当前容量的 1.5 倍和加上 c 的元素后的元素数量的更大值。
    - 使用无参构造方法初始化时，第一次扩容为`Math.max(10, c.size())`，之后每次扩容为`Math.max(length + (length >> 1), size + c.size())`。
    - 使用有参构造方法初始化时，第一次扩容为`size + c.size()`，之后每次扩容为`Math.max(length + (length >> 1), size + c.size())`。

```java
protected transient int modCount = 0;                                 // 修改次数，继承自 AbstractList

private static final int DEFAULT_CAPACITY = 10;                       // 默认容量
private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;      // 最大容量
private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {}; // 空数组，无参构造器使用
private static final Object[] EMPTY_ELEMENTDATA = {};                 // 空数组，其他情况使用
transient Object[] elementData;                                       // 存储元素
private int size;                                                     // 已存储元素数量

public ArrayList() { // 共享空数组
    this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
}

public ArrayList(int initialCapacity) {
    if (initialCapacity > 0) { // 使用指定容量创建数组
        this.elementData = new Object[initialCapacity];
    } else if (initialCapacity == 0) { // 共享空数组
        this.elementData = EMPTY_ELEMENTDATA;
    } else {
        throw new IllegalArgumentException("Illegal Capacity: "+ initialCapacity);
    }
}

public ArrayList(Collection<? extends E> c) {
    Object[] a = c.toArray();
    size = a.length;
    if (size != 0) {
        if (c.getClass() == ArrayList.class) elementData = a;
        else elementData = Arrays.copyOf(a, size, Object[].class);
    } else // 共享空数组
        elementData = EMPTY_ELEMENTDATA;
}

public boolean add(E e) {
    modCount++;
    add(e, elementData, size);
    return true;
}

public boolean addAll(Collection<? extends E> c) {
    Object[] a = c.toArray();
    modCount++;
    int numNew = a.length;
    if (numNew == 0) return false;
    Object[] elementData = this.elementData;
    final int s = size;
    if (numNew > elementData.length - s) // 剩余容量不够，扩容到至少 size + c.size()
        elementData = grow(s + numNew);
    System.arraycopy(a, 0, elementData, s, numNew);
    size = s + numNew;
    return true;
}

private void add(E e, Object[] elementData, int s) {
    // 在索引 s 位置上添加元素
    if (s == elementData.length) // 空间不够，扩容
        elementData = grow();
    elementData[s] = e;
    size = s + 1;
}

private Object[] grow() {
    return grow(size + 1); // 扩容到至少 size + 1
}

private Object[] grow(int minCapacity) { // 指定容量扩容
    return elementData = Arrays.copyOf(elementData, newCapacity(minCapacity));
}

private int newCapacity(int minCapacity) { // 返回扩容后的容量
    int oldCapacity = elementData.length;
    int newCapacity = oldCapacity + (oldCapacity >> 1);       // 新容量为原来容量的 1.5 倍，向下取整
    if (newCapacity - minCapacity <= 0) {                     // 当指定容量大于新容量时，使用指定容量
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) // 若为无参构造器构造，则第一次扩容至少为 10
            return Math.max(DEFAULT_CAPACITY, minCapacity);
        if (minCapacity < 0) throw new OutOfMemoryError();
        return minCapacity;
    }
    return (newCapacity - MAX_ARRAY_SIZE <= 0) ? newCapacity : hugeCapacity(minCapacity);
}

private static int hugeCapacity(int minCapacity) {
    if (minCapacity < 0) throw new OutOfMemoryError();
    return (minCapacity > MAX_ARRAY_SIZE) ? Integer.MAX_VALUE : MAX_ARRAY_SIZE;
}
```

### ArrayList 和 LinkedList 比较

- `ArrayList`
    1. 基于数组，需要分配连续空间。
    2. 随机访问快，可根据索引访问。实现了`RandomAccess`接口，该接口仅用于说明容器支持随机访问。
    3. 尾部插入/删除性能高；越靠近首部，插入/删除性能越低，因为需要移动元素。
    4. 因为时连续空间，所以可以利用缓存，实现高效访问。
- `LinkedList`
    1. 基于双向链表，元素不需要连续存储。
    2. 随机访问慢，不能根据索引访问。
    3. 头部/尾部的插入/删除性能高；越靠近中间，插入/删除性能越低，需要遍历找到对应元素。
    4. 结点除了存储元素，还需要存储前/后指针，占用内存多。

```java
public interface RandomAccess {}

// ArrayList
public class ArrayList<E> extends AbstractList<E> implements RandomAccess {
    transient Object[] elementData;
    private int size;

    public E get(int index) {
        Objects.checkIndex(index, size);
        return elementData(index);
    }

    E elementData(int index) {
        return (E) elementData[index];
    }

    public E remove(int index) {
        Objects.checkIndex(index, size);
        final Object[] es = elementData;
        E oldValue = (E) es[index];
        fastRemove(es, index);
        return oldValue;
    }

    private void fastRemove(Object[] es, int i) {
        modCount++;
        final int newSize = size - 1;
        if (newSize > i) // 移动元素
            System.arraycopy(es, i + 1, es, i, newSize - i);
        size = newSize;
        es[size] = null;
    }
}

// LinkedList
public class LinkedList<E> extends AbstractSequentialList<E> {
    transient int size = 0;
    transient Node<E> first;
    transient Node<E> last;

    private static class Node<E> {
        E item;
        Node<E> next;
        Node<E> prev;

        Node(Node<E> prev, E element, Node<E> next) {
            this.item = element;
            this.next = next;
            this.prev = prev;
        }
    }

    public E getFirst() {
        final Node<E> f = first;
        if (f == null) throw new NoSuchElementException();
        return f.item;
    }

    public E getLast() {
        final Node<E> l = last;
        if (l == null) throw new NoSuchElementException();
        return l.item;
    }

    public E get(int index) {
        checkElementIndex(index);
        return node(index).item;
    }

    public void addFirst(E e) {
        linkFirst(e);
    }

    public void addLast(E e) {
        linkLast(e);
    }

    public void add(int index, E element) {
        checkPositionIndex(index);
        if (index == size) linkLast(element);
        else linkBefore(element, node(index));
    }

    public E set(int index, E element) {
        checkElementIndex(index);
        Node<E> x = node(index);
        E oldVal = x.item;
        x.item = element;
        return oldVal;
    }

    public E removeFirst() {
        final Node<E> f = first;
        if (f == null) throw new NoSuchElementException();
        return unlinkFirst(f);
    }

    public E removeLast() {
        final Node<E> l = last;
        if (l == null) throw new NoSuchElementException();
        return unlinkLast(l);
    }

    public E remove(int index) {
        checkElementIndex(index);
        return unlink(node(index));
    }

    private void checkElementIndex(int index) {
        if (!isElementIndex(index)) throw new IndexOutOfBoundsException(outOfBoundsMsg(index));
    }

    Node<E> node(int index) {
        if (index < (size >> 1)) { // 查询前半部分
            Node<E> x = first;
            for (int i = 0; i < index; i++) // 从头开始遍历
                x = x.next;
            return x;
        } else { // 查询后半部分
            Node<E> x = last;
            for (int i = size - 1; i > index; i--) // 从尾开始遍历
                x = x.prev;
            return x;
        }
    }
}
```

### HashMap

```java
public class HashMap<K,V> extends AbstractMap<K,V> {
    static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16
    static final float DEFAULT_LOAD_FACTOR = 0.75f;
    static final int TREEIFY_THRESHOLD = 8;
    static final int UNTREEIFY_THRESHOLD = 6;
    static final int MIN_TREEIFY_CAPACITY = 64;

    transient Node<K,V>[] table;
    transient int size;
    transient int modCount;
    int threshold;
    final float loadFactor;

    static class Node<K,V> implements Map.Entry<K,V> {
        final int hash;
        final K key;
        V value;
        Node<K,V> next;

        Node(int hash, K key, V value, Node<K,V> next) {
            this.hash = hash;
            this.key = key;
            this.value = value;
            this.next = next;
        }

        public final K getKey()        { return key; }
        public final V getValue()      { return value; }
        public final String toString() { return key + "=" + value; }

        public final V setValue(V newValue) {
            V oldValue = value;
            value = newValue;
            return oldValue;
        }

        public final boolean equals(Object o) {
            if (o == this) return true;
            if (o instanceof Map.Entry) {
                Map.Entry<?, ?> e = (Map.Entry<?, ?>) o;
                if (Objects.equals(key, e.getKey()) && Objects.equals(value, e.getValue()))
                    return true;
            }
            return false;
        }
    }

    public HashMap() {
        this.loadFactor = DEFAULT_LOAD_FACTOR;
    }

    public HashMap(int initialCapacity) {
        this(initialCapacity, DEFAULT_LOAD_FACTOR);
    }

    public HashMap(int initialCapacity, float loadFactor) {
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal initial capacity: " + initialCapacity);
        if (initialCapacity > MAXIMUM_CAPACITY) initialCapacity = MAXIMUM_CAPACITY;
        if (loadFactor <= 0 || Float.isNaN(loadFactor))
            throw new IllegalArgumentException("Illegal load factor: " + loadFactor);
        this.loadFactor = loadFactor;
        this.threshold = tableSizeFor(initialCapacity);
    }

    static final int tableSizeFor(int cap) {
        // 扩容为 2 的次方
        int n = -1 >>> Integer.numberOfLeadingZeros(cap - 1);
        return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
    }

    public V put(K key, V value) {
        return putVal(hash(key), key, value, false, true);
    }

    static final int hash(Object key) {
        if (key == null) return 0;
        int h = key.hashCode();
        return h ^ (h >>> 16); // 增加了结果的随机性，因为高位一般不参与计算下标（数组不够大）
    }

    final V putVal(int hash, K key, V value, boolean onlyIfAbsent, boolean evict) {
        Node<K, V>[] tab = table;
        Node<K, V> p;
        int n = tab.length;
        int i;
        if (tab == null || n == 0) {
            tab = resize();
            n = tab.length;
        }
        i = (n - 1) & hash; // 计算下标，等价于 hash % n，因为 n 是 2 的次方，所以可以用 & 代替 %
        p = tab[i];
        if (p == null) // 没有元素，直接添加
            tab[i] = newNode(hash, key, value, null);
        else { // 存在元素，遍历查找是否 key 已存在
            Node<K, V> e;
            if (p.hash == hash && (p.key == key || (key != null && key.equals(p.key))))
                e = p; // 已找到
            else if (p instanceof TreeNode) // 查找红黑树
                e = ((TreeNode<K, V>) p).putTreeVal(this, tab, hash, key, value);
            else { // 查找链表
                for (int binCount = 0; ; ++binCount) {
                    e = p.next;
                    if (e == null) { // 查找完发现 key 不存在，则添加结点到链表末尾
                        p.next = newNode(hash, key, value, null);
                        if (binCount >= TREEIFY_THRESHOLD - 1)
                            treeifyBin(tab, hash); // 链表长度超过 8，可升级为红黑树
                        break;
                    }
                    if (e.hash == hash && (e.key == key || (key != null && key.equals(e.key))))
                        break; // 已找到
                    p = e;
                }
            }
            if (e != null) {
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null) e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
        ++modCount;
        if (++size > threshold) resize();
        afterNodeInsertion(evict);
        return null;
    }



}
```

### fail-fast 和 fail-safe

- fail-fast：遍历过程中若发现容器进行了修改，抛出`ConcurrentModificationException`。如`ArrayList`、`Vector`、`LinkedList`。
    - Java 容器类中使用`modCount`表示容器的修改次数，若遍历过程中`modCount`发生了改变，则说明容器发生了修改，抛出异常。
- fail-safe：正常完成遍历，对容器发生的修改不可见。如`CopyOnWriteArrayList`。
    - `CopyOnWriteArrayList`容器进行修改时会复制一份新数组，不对原数组上进行修改，然后替换原数组。

```java
// ArrayList
public class ArrayList<E> extends AbstractList<E> {
    protected transient int modCount = 0; // 继承自 AbstractList

    transient Object[] elementData;
    private int size;

    public Iterator<E> iterator() { // 迭代器
        return new Itr();
    }

    private class Itr implements Iterator<E> {
        int cursor;
        int lastRet = -1;
        int expectedModCount = modCount;

        public boolean hasNext() {
            return cursor != size;
        }

        public E next() {
            checkForComodification();
            int i = cursor;
            if (i >= size) throw new NoSuchElementException();
            Object[] elementData = ArrayList.this.elementData;
            if (i >= elementData.length) throw new ConcurrentModificationException();
            cursor = i + 1;
            return (E) elementData[lastRet = i];
        }

        final void checkForComodification() {
            if (modCount != expectedModCount) throw new ConcurrentModificationException();
        }
    }
}

// Vector
public class Vector<E> extends AbstractList<E> {
    protected transient int modCount = 0; // 继承自 AbstractList

    protected Object[] elementData;
    protected int elementCount;

    public synchronized Iterator<E> iterator() {
        return new Itr();
    }

    private class Itr implements Iterator<E> {
        int cursor;
        int lastRet = -1;
        int expectedModCount = modCount;

        public boolean hasNext() {
            return cursor != elementCount;
        }

        public E next() {
            synchronized (Vector.this) {
                checkForComodification();
                int i = cursor;
                if (i >= elementCount) throw new NoSuchElementException();
                cursor = i + 1;
                return elementData(lastRet = i);
            }
        }

        public void remove() {
            if (lastRet == -1) throw new IllegalStateException();
            synchronized (Vector.this) {
                checkForComodification();
                Vector.this.remove(lastRet);
                expectedModCount = modCount;
            }
            cursor = lastRet;
            lastRet = -1;
        }

        final void checkForComodification() {
            if (modCount != expectedModCount) throw new ConcurrentModificationException();
        }
    }

// CopyOnWriteArrayList
public class CopyOnWriteArrayList<E> {
    private transient volatile Object[] array;

    public boolean add(E e) {
        synchronized (lock) {
            Object[] es = getArray();
            int len = es.length;
            es = Arrays.copyOf(es, len + 1); // 创建新数组
            es[len] = e;                     // 在新数组中进行修改
            setArray(es);                    // 指向新数组
            return true;
        }
    }

    final Object[] getArray() {
        return array;
    }

    final void setArray(Object[] a) {
        array = a;
    }
}
```

## 面向对象

### 封装

#### 四种权限修饰符

- `public`
    - 修饰范围：外部类、内部类、方法、属性。
    - 继承特性：可继承。
    - 作用范围：可被任意类访问。
    - 其他特性：一个 Java 文件只能有一个`public`外部类，且文件名与类名相同。
- `protected`
    - 修饰范围：内部类、方法、属性。
    - 继承特性：可继承。
    - 作用范围：可被同一个包下的类和子类访问。
- ` `
    - 修饰范围：外部类、内部类、方法、属性、局部变量。
    - 继承特性：可被同一个包下的子类继承。
    - 作用范围：可被同一个包下的类访问。
- `private`
    - 修饰范围：内部类、方法、属性。
    - 继承特性：不可继承。
    - 作用范围：只能本类访问。

> `protected`和`private`只能修饰内部类，不能修饰外部类。

#### 三种面向对象修饰符

- `abstract`
    - 修饰范围：外部类、内部类、方法（抽象类、接口）。
    - 其他特性：
        - 抽象方法无方法体。
        - 抽象类的子类必须重写父类所有抽象方法后才能创建对象，否则也成为一个抽象类。
- `static`
    - 修饰范围：内部类、方法、属性。
    - 其他特性：
        - 通过类名访问。
        - 所有实例共享。
        - 静态方法中不能访问非静态方法和非静态属性。
        - 静态方法中不能访问`this`和`super`。
- `final`
    - 修饰范围：外部类、内部类、方法（非构造方法）、属性、局部变量。
    - 其他特性：
        - 修饰的类不可被继承，类中的方法也被`final`修饰。
        - 修饰的方法不可被重写。
        - 修饰的属性必须赋值（直接赋值、构造块赋值、构造方法赋值，三选一），且不可修改。
        - 修饰的局部变量一旦赋值便不可修改。
        - 若修饰的是引用类型的变量，则初始化后不可指向另一对象，但对象内容仍可发生改变。

> `abstract`与`private`、`static`和`final`均不可搭配。
>
> `private`方法会隐式地被指定为`final`方法。

### 继承

### 多态

## 编码

- ASCII：共 127 个字符，每个字符占 1B。
- ISO-8859-1：共 256 个字符，每个字符占 1B。
- GB2312
- GBK
- Unicode（Java 默认）：每个字符占 2B，每个中文字符占 2B。
- UTF-8：每个字符占 1-4B，每个中文字符占 3B。
- UTF-16：每个字符占 2B 或 4B。
- UTF-32：每个字符占 4B。

## 基本数据类型

1. `boolean`：单独使用占 4B（当作`int`），数组使用占 1B（当作`byte`）。包装类为`Boolean`。只能取`true`或`false`。
1. `byte`：占 1B。包装类为`Byte`。
1. `short`：占 2B。包装类为`Short`。
1. `char`：占 2B。包装类为`Character`。
1. `int`：占 4B。包装类为`Integer`。默认整数类型。
1. `float`：占 4B。包装类为`Float`。如`3.14f`。
1. `long`：占 8B。包装类为`Long`。如`1L`。
1. `double`：占 8B。包装类为`Double`。默认浮点数类型。

> `byte`、`short`、`char`与整数发生运算时会被向上转型为`int`。
>
> `float`与浮点数发生运算时会被向上转型为`double`。

## 常量池



## 反射

程序在运行时可以获取任意类的所有信息，可以访问任意对象的所有方法和属性。

### Class 类

每个 Java 类运行时都在 JVM 里表现为一个 Class 对象，包括数组、`boolean`、`byte`、`char`、`short`、`int`、`float`、`long`、`double`和`void`。

- 每个类被编译后会产生一个 Class 对象（唯一），包含该类的类型信息，保存在同名的 .class 文件中。
- 每个类对应的 Class 对象只有一个（唯一），无论创建多少个实例对象，其依据的都是用一个 Class 对象。
- Class 类的构造方法私有，因此 Class 类的实例不能手动创建，只能由 JVM 创建和加载。
- Class 对象的作用是在运行时获取某个对象的类型信息。

### 获取 Class 对象的三种方式

```java
// 1.通过类
Class<?> clazz1 = Person.class;

// 2.通过实例
Class<?> clazz2 = new Person().getClass();

// 3.通过全限定类名
try {
    Class<?> clazz3 = Class.forName("src.Person");
} catch (ClassNotFoundException e) {
    e.printStackTrace();
}

class Person {}
```

### 反射操作对象

- `Person`

```java
package src;

class Person {
    private String name;
    private int age;

    Person() {
    }

    private Person(Person p) {
    }

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public void setName(String name) {
        setNameAndAge(name, age);
    }

    public void setAge(int age) {
        setNameAndAge(name, age);
    }

    private void setNameAndAge(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person [name=" + name + ", age=" + age + "]";
    }
}
```

- 简单操作

```java
Class<?> clz = Person.class;

String s1 = clz.getSimpleName();    // 类名
String s2 = clz.getName();          // 全限定类名
String s3 = clz.getCanonicalName(); // 包名+类名

boolean b = clz.isInterface(); // 是否接口

Class<?>[] c1 = clz.getInterfaces(); // 返回实现的所有接口的 Class 对象
Class<?> c2 = clz.getSuperclass();   // 返回实父类的 Class 对象

try {
    Person p1 = (Person) clz.newInstance(); // 创建对象（必须有无参构造方法），从 JDK 9 开始弃用
} catch (Exception e) {
    e.printStackTrace();
}
```

> 一般情况下，`getName()`和`getCanonicalName()`返回值相同，但对于内部类、数组，两者返回值不同。只有`getName()`返回的才是全限定名。

- `Constructor`：获取构造方法。

```java
import java.lang.reflect.Constructor;

Class<?> clz = Person.class;

Constructor<?>[] c1 = clz.getDeclaredConstructors(); // 返回所有构造方法
Constructor<?>[] c2 = clz.getConstructors();         // 返回包括父类的所有 public 构造方法

try {
    Constructor<?> c3 = clz.getDeclaredConstructor(String.class, int.class); // 返回指定类型的构造方法
    Constructor<?> c4 = clz.getConstructor(String.class, int.class);         // 返回指定类型的构造方法

    Person person = (Person) c3.newInstance("Alice", 18);                    // 通过构造方法传入参数创建对象
    Class<?>[] c5 = c3.getParameterTypes();                                  // 获取参数类型
    String s1 = c3.getName();                                                // src.Person
    String s2 = c3.toGenericString();                                        // public src.Person(java.lang.String,int)
} catch (Exception e) {
    e.printStackTrace();
}
```

- `Method`：获取方法。

```java
import java.lang.reflect.Method;

Class<?> clz = Person.class;
Person person = new Person("Alice", 18);

Method[] m1 = clz.getDeclaredMethods(); // 返回所有方法
Method[] m2 = clz.getMethods();         // 返回包括父类的所有 public 方法

try {
    Method m3 = clz.getDeclaredMethod("setName", String.class); // 返回指定类型的方法
    Method m4 = clz.getMethod("toString");                      // 返回指定类型的方法
    m4.setAccessible(true);                                     // 设置可访问

    m3.invoke(person, "Mike");                                  // 对指定对象调用该方法
    String s1 = (String) m4.invoke(person);                     // 对指定对象调用该方法，并获取返回值

    Class<?> c1 = m3.getReturnType();                           // 返回返回值类型
    Class<?>[] c2 = m3.getParameterTypes();                     // 返回方法的参数类型
    boolean	b1 = m3.isVarArgs();                                // 判断是否带有可变参数

    String s2 = m3.getName();                                   // 返回方法名
    String s3 = m3.toGenericString();                           // 返回方法名，带类型
} catch (Exception e) {
    e.printStackTrace();
}
```

- `Field`：获取属性。

```java
import java.lang.reflect.Field;

Class<?> clz = Person.class;
Person person = new Person("Alice", 18);

Field[] f1 = clz.getDeclaredFields(); // 返回所有属性
Field[] f2 = clz.getFields();         // 返回包括父类的所有 public 属性

try {
    Field f3 = clz.getDeclaredField("name"); // 返回指定属性
    f3.setAccessible(true);                  // private 设置为可访问
    Field f4 = clz.getField("name");         // 返回指定属性

    String s1 = (String) f3.get(person);     // 获取指定对象的该属性
    f3.set(person, "Mike");                  // 对指定对象的该属性赋值
    Class<?> c1 = f3.getType();              // 获取属性类型
    boolean	b1 = f3.isEnumConstant();        // 判断是否枚举类型
    String s2 = f3.getName();                // 属性名称
    String s3 = f3.toGenericString();        // 属性名称，带类型
    Class<?> c2 = getDeclaringClass();       // 获取定义该属性的类
} catch (Exception e) {
    e.printStackTrace();
}
```

### 反射过程及实现

1. 反射类及反射方法的获取，都是通过从列表中搜寻查找匹配的方法，所以查找性能会随类的大小方法多少而变化；
1. 每个类都会有一个与之对应的Class实例，从而每个类都可以获取method反射方法，并作用到其他实例身上；
1. 反射也是考虑了线程安全的，放心使用；
1. 反射使用软引用relectionData缓存class信息，避免每次重新从jvm获取带来的开销；
1. 反射调用多次生成新代理Accessor, 而通过字节码生存的则考虑了卸载功能，所以会使用独立的类加载器；
1. 当找到需要的方法，都会copy一份出来，而不是使用原来的实例，从而保证数据隔离；
1. 调度反射方法，最终是由jvm执行invoke0()执行

## 注解
