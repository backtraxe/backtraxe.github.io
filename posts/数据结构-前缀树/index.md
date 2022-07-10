# 数据结构-前缀树


<!--more-->

## 1.介绍

**前缀树**，又称**字典树（Trie）** ，是一种特殊的 N 叉树。

- 前缀树用来存储字符串，每条边代表一个字符。每一个节点会有多个子节点，则通往不同子节点的路径上有着不同的字符。
- 从根节点通往某节点路径上所有的字符组成对应的字符串。
- 根节点表示`空字符串`。
- 节点所有的后代都与该节点相关的字符串有着共同的前缀。

<br />

## 2.实现

**数组：**

- 优点：访问结点快速。
- 缺点：空间浪费。通过下标访问结点。

**哈希表：**

- 优点：通过字符访问结点。节约空间。
- 缺点：速度稍慢。

<br />

## 3.代码

### 3.1 数组

```java
class TrieNode {
    boolean isEnd = false;
    TrieNode[] next = new TrieNode[26];
}

class Trie {
    TrieNode root;

    public Trie() {
        root = new TrieNode();
    }

    public void insert(String s) {
        // 插入字符串
        TrieNode p = root;
        for (char c : s.toCharArray()) {
            int i = c - 'a';
            if (p.next[i] == null)
                p.next[i] = new TrieNode();
            p = p.next[i];
        }
        p.isEnd = true;
    }

    public boolean search(String s) {
        // 查找字符串 s 是否在树中
        TrieNode p = root;
        for (char c : s.toCharArray()) {
            if (p.next[c - 'a'] == null)
                return false;
            p = p.next[c - 'a'];
        }
        return p.isEnd;
    }

    public boolean startsWith(String s) {
        // 查找是否存在前缀字符串 s
        TrieNode p = root;
        for (char c : s.toCharArray()) {
            if (p.next[c - 'a'] == null)
                return false;
            p = p.next[c - 'a'];
        }
        return true;
    }
}
```

- 时间复杂度：字符串长度为 len。$ O(len) $
- 空间复杂度：节点数量为 n，字符集大小为 k。$ O(n * k) $

<br />

### 3.2 哈希表

```java
class TrieNode {
    Map<Character, TrieNode> children = new HashMap<>();
    isEnd = false;
}

class Trie {
    TrieNode root;

    Trie() {
        root = new TrieNode();
    }

    void insert(String s) {
        TrieNode p = root;
        for (char c : s.toCharArray()) {
            p.children.putIfAbsent(c, new TrieNode());
            p = p.children.get(c);
        }
        p.isEnd = true;
    }
}
```

<br />

### 3.3 二维数组

```java
class Trie {
    static final int MAX_N = 100009;

    int[][] trie;

    // 以当前字符结尾的单词的数量
    int[] count;

    // 尚未使用的索引开始位置
    int index;

    public Trie() {
        trie = new int[MAX_N][26];
        count = new int[MAX_N];
        index = 0;
    }

    public void insert(String s) {
        // 插入 s
        int p = 0;
        for (int i = 0; i < s.length(); i++) {
            int u = s.charAt(i) - 'a';
            if (trie[p][u] == 0) trie[p][u] = ++index;
            p = trie[p][u];
        }
        count[p]++;
    }

    public boolean search(String s) {
        // 是否存在 s
        int p = 0;
        for (int i = 0; i < s.length(); i++) {
            int u = s.charAt(i) - 'a';
            if (trie[p][u] == 0) return false;
            p = trie[p][u];
        }
        return count[p] != 0;
    }

    public boolean startsWith(String s) {
        // 是否存在前缀 s
        int p = 0;
        for (int i = 0; i < s.length(); i++) {
            int u = s.charAt(i) - 'a';
            if (trie[p][u] == 0) return false;
            p = trie[p][u];
        }
        return true;
    }
}
```

- 时间复杂度：字符串长度为 len。$ O(len) $
- 空间复杂度：节点数量为 n，字符集大小为 k。$ O(n * k) $

