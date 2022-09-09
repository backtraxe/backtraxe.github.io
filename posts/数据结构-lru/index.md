# 数据结构-LRU


<!--more-->

LRU (Least Recently Used)

最近最少使用算法。当容量满时，将最久没有使用过的缓存删除。

## 实现

```java
class Node {
    int key, val;
    Node prev, next;

    public Node(int key, int val) {
        this.key = key;
        this.val = val;
    }
}

class BiLinkedList {
    final Node head, tail;
    final int capacity;
    int length;

    public BiLinkedList(int capacity) {
        this.capacity = capacity;
        length = 0;
        head = new Node(0, 0);
        tail = new Node(0, 0);
        head.next = tail;
        tail.prev = head;
    }

    /**
     * 将结点加入双向链表的尾部
     *
     * @param node 待加入结点
     */
    public void addLast(Node node) {
        node.prev = tail.prev;
        node.next = tail;
        node.prev.next = node;
        tail.prev = node;
        length++;
    }

    /**
     * 将 node 从双向链表中删除
     *
     * @param node 待加入结点
     */
    public void remove(Node node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
        length--;
    }

    /**
     * 将双向链表的第一个结点删除
     */
    public void removeFirst() {
        if (length == 0) {
            return;
        }
        head.next = head.next.next;
        head.next.prev = head;
        length--;
    }
}

class LRUCache {
    private BiLinkedList list;
    private HashMap<Integer, Node> keyToNode;

    public LRUCache(int capacity) {
        list = new BiLinkedList(capacity);
        keyToNode = new HashMap<>();
    }

    public int get(int key) {
        if (keyToNode.containsKey(key)) {
            Node node = keyToNode.get(key);
            list.remove(node);
            list.addLast(node);
            return node.val;
        } else {
            return -1;
        }
    }

    public void put(int key, int value) {
        if (keyToNode.containsKey(key)) {
            Node node = keyToNode.get(key);
            node.val = value;
            list.remove(node);
            list.addLast(node);
        } else {
            if (list.length == list.capacity) {
                keyToNode.remove(list.head.next.key);
                list.removeFirst();
            }
            Node node = new Node(key, value);
            keyToNode.put(key, node);
            list.addLast(node);
        }
    }
}
```

```java
class LRUCache {
    class ListNode {
        int key, val;
        ListNode prev, next;

        public ListNode(int key, int val) {
            this.key = key;
            this.val = val;
        }
    }

    HashMap<Integer, ListNode> key2node;
    ListNode head; // 双链表头结点
    int capacity;

    public LRUCache(int capacity) {
        key2node = new HashMap<>();
        head = new ListNode(0, 0);
        head.prev = head.next = head; // 循环链表
        this.capacity = capacity;
    }

    public int get(int key) {
        ListNode node = key2node.get(key);
        if (node == null) return -1;
        // 取出 node
        node.prev.next = node.next;
        node.next.prev = node.prev;
        // node 插入末尾
        head.prev.next = node;
        node.prev = head.prev;
        node.next = head;
        head.prev = node;
        return node.val;
    }

    public void put(int key, int value) {
        ListNode node = key2node.get(key);
        if (node != null) {
            // 取出 node
            node.prev.next = node.next;
            node.next.prev = node.prev;
            node.val = value;
        } else {
            node = new ListNode(key, value);
            key2node.put(key, node);
        }
        // node 插入末尾
        head.prev.next = node;
        node.prev = head.prev;
        node.next = head;
        head.prev = node;
        if (key2node.size() > capacity) {
            // 删除第一个结点
            key2node.remove(head.next.key);
            head.next = head.next.next;
            head.next.prev = head;
        }
    }
}
```

## API

```java
class LRUCache {
    private LinkedHashMap<Integer, Integer> data;
    private int capacity;

    public LRUCache(int capacity) {
        this.data = new LinkedHashMap<>();
        this.capacity = capacity;
    }

    public int get(int key) {
        int val = data.getOrDefault(key, -1);
        if (data.containsKey(key)) {
            data.remove(key);
            data.put(key, val);
        }
        return val;
    }

    public void put(int key, int value) {
        data.remove(key);
        data.put(key, value);
        if (data.size() > this.capacity) {
            data.remove(data.keySet().iterator().next());
        }
    }
}
```

