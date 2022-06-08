# 数据结构-树


<!--more-->

- 一棵 n 个节点的树，有 n-1 条边。
- 一棵 n 个节点的树，有 n 棵子树。
- 根节点：唯一，无入度的节点
- 节点的深度：节点距离根节点的距离。

```cpp
typedef struct treeNode {
    treeNode(int x): value(x) {}
    int value;
    vector<treeNode*> child;
} TreeNode;
```

```java
class TreeNode {
    public int val;
    public TreeNode[] children;
    TreeNode() {
        this.val = 0;
    }
}
```

