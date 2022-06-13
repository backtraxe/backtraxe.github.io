# 数据结构-前缀树


<!--more-->

## 1.介绍

**前缀树**，又称**字典树（Trie）** ，是一种特殊的 N 叉树。


- 前缀树用来存储字符串，每个节点代表一个字符。每一个节点会有多个子节点，通往不同子节点的路径上有着不同的字符。子节点代表的字符串是由节点本身的原始字符串 ，以及通往该子节点路径上所有的字符组成的。
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

## 3.代码

```java
class TrieNode {
    static final int MAX_N = 26;
    boolean isEnd = false;
    TrieNode[] children = new TrieNode[MAX_N];
}

class Trie {
    TrieNode root;

    Trie() {
        root = new TrieNode();
    }

    void insert(String s) {
        // 插入字符串
        TrieNode p = root;
        for (char c : s.toCharArray()) {
            int i = c - 'a';
            if (p.children[i] == null)
                p.children[i] = new TrieNode();
            p = p.children[i];
        }
        p.isEnd = true;
    }

    boolean contains(String s) {
        // 查找字符串 s 是否在树中
        TrieNode p = root;
        for (char c : s.toCharArray()) {
            if (p.children[c - 'a'] == null)
                return false;
            p = p.children[c - 'a'];
        }
        return p.isEnd;
    }

    
}
```

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

