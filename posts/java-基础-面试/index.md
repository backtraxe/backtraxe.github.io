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

## 反射


