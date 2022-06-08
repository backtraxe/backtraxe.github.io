# 数据结构-LFU


<!--more-->

LFU (Least Frequently Used)

```java
class Node {
    int key, value;
    Node prev, next;
    public Node (int key, int value) {
        this.key = key;
        this.value = value;
    }
}

class BiLinkedList {
    final Node head, tail;
    int length;

    public BiLinkedList() {
        this.head = new Node(0, 0);
        this.tail = new Node(0, 0);
        this.length = 0;
        head.next = tail;
        tail.prev = head;
    }

    public void addLast(Node node) {
        node.prev = tail.prev;
        node.next = tail;
        tail.prev.next = node;
        tail.prev = node;
        length++;
    }

    public void remove(Node node) {
        if (node.prev == null || node.next == null) {
            return;
        }
        node.prev.next = node.next;
        node.next.prev = node.prev;
        length--;
    }

    public void removeFirst() {
        if (length == 0) {
            return;
        }
        head.next.next.prev = head;
        head.next = head.next.next;
        length--;
    }
}

class LFUCache {
    HashMap<Integer, Node> keyToNode;
    HashMap<Integer, Integer> keyToFreq;
    HashMap<Integer, BiLinkedList> freqToNodes;
    final int capacity;
    int length;
    int minFreq;

    public LFUCache(int capacity) {
        this.keyToNode = new HashMap<>();
        this.keyToFreq = new HashMap<>();
        this.freqToNodes = new HashMap<>();
        this.capacity = capacity;
        this.length = 0;
        this.minFreq = -1;
    }

    public int get(int key) {
        if (keyToNode.containsKey(key)) {
            Node node = keyToNode.get(key);
            // 更新频率
            int freq = keyToFreq.get(key);
            keyToFreq.put(key, freq + 1);
            // 删除结点
            freqToNodes.get(freq).remove(node);
            // 添加结点
            if (freqToNodes.containsKey(freq + 1)) {
                freqToNodes.get(freq + 1).addLast(node);
            } else {
                BiLinkedList list = new BiLinkedList();
                list.addLast(node);
                freqToNodes.put(freq + 1, list);
            }
            // 更新 minFreq
            if (freq == minFreq) {
                while (freqToNodes.get(minFreq).length == 0) {
                    minFreq++;
                }
            }
            return node.value;
        } else {
            return -1;
        }
    }

    public void put(int key, int value) {
        if (keyToNode.containsKey(key)) {
            // 更新值
            Node node = keyToNode.get(key);
            node.value = value;
            // 更新频率
            int freq = keyToFreq.get(key);
            keyToFreq.put(key, freq + 1);
            // 删除结点
            freqToNodes.get(freq).remove(node);
            // 添加结点
            if (freqToNodes.containsKey(freq + 1)) {
                freqToNodes.get(freq + 1).addLast(node);
            } else {
                BiLinkedList list = new BiLinkedList();
                list.addLast(node);
                freqToNodes.put(freq + 1, list);
            }
            // 更新 minFreq
            if (freq == minFreq) {
                while (freqToNodes.get(minFreq).length == 0) {
                    minFreq++;
                }
            }
        } else {
            if (capacity == 0) {
                return;
            }
            if (length == capacity) {
                int rkey = freqToNodes.get(minFreq).head.next.key;
                freqToNodes.get(minFreq).removeFirst();
                keyToNode.remove(rkey);
                keyToFreq.remove(rkey);
                length--;
            }
            Node node = new Node(key, value);
            keyToNode.put(key, node);
            keyToFreq.put(key, 1);
            if (freqToNodes.containsKey(1)) {
                freqToNodes.get(1).addLast(node);
            } else {
                BiLinkedList list = new BiLinkedList();
                list.addLast(node);
                freqToNodes.put(1, list);
            }
            minFreq = 1;
            length++;
        }
    }
}
```

