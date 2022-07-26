# 力扣 0919 完全二叉树插入器


[919. 完全二叉树插入器](https://leetcode.cn/problems/complete-binary-tree-inserter/)

<!--more-->

## 方法一：每次`insert`时BFS查找待插入结点的位置

### 解题思路

插入。BFS 遍历树，找到的第一个左孩子或者右孩子不存在的结点则为待插入结点的父结点。
- 若左孩子不存在，则新插入结点为父结点的左孩子。
- 若右孩子不存在，则新插入结点为父结点的右孩子。

### 代码

```java
class CBTInserter {
    TreeNode root;

    public CBTInserter(TreeNode root) {
        this.root = root;
    }

    public int insert(int val) {
        Queue<TreeNode> que = new ArrayDeque<>();
        que.offer(root);
        while (!que.isEmpty()) {
            TreeNode node = que.poll();
            if (node.left == null) {
                node.left = new TreeNode(val);
                return node.val;
            } else if (node.right == null) {
                node.right = new TreeNode(val);
                return node.val;
            }
            que.offer(node.left);
            que.offer(node.right);
        }
        return -1;
    }

    public TreeNode get_root() {
        return root;
    }
}
```

### 复杂度分析

{{< figure src="https://pic.leetcode-cn.com/1658715644-PxHdiO-image.png" >}}

- 初始化：$O(1)$
- `insert`：$O(n)$，$n$ 为二叉树结点数量，每次`insert`时执行一遍 BFS 需要遍历大概一半的结点数。

---

## 方法二：存储最后两层的结点

### 解题思路

1. 初始化。通过 BFS 遍历树，存储最后两层的结点。
2. 插入。
    - 若最后一层已满，新插入结点为新的一层的第一个结点，其父结点为上一层第一个结点。然后更新最后两层。
    - 若最后一层未满，大小为`k`，新插入结点插入该层的末尾，其父结点为上一层的第`k/2`个结点，若`k`为偶数，则作为左孩子插入，否则是右孩子。

### 代码

```java
class CBTInserter {
    TreeNode root;
    List<TreeNode> row1, row2; // 最后一层，倒数第二层。
    int level; // 层数

    public CBTInserter(TreeNode root) {
        this.root = root;
        Queue<TreeNode> que = new ArrayDeque<>();
        que.offer(root);
        while (!que.isEmpty()) {
            int size = que.size();
            List<TreeNode> row = new ArrayList<>();
            while (size-- > 0) {
                TreeNode node = que.poll();
                row.add(node);
                if (node.left != null) que.offer(node.left);
                if (node.right != null) que.offer(node.right);
            }
            level++;
            // 更新最后两层
            row2 = row1;
            row1 = row;
        }
        ensureLastRow();
    }

    public int insert(int val) {
        TreeNode node = new TreeNode(val);
        int k = row1.size();
        TreeNode parent = row2.get(k / 2);
        if (k % 2 == 0) parent.left = node;
        else parent.right = node;
        row1.add(node);
        ensureLastRow();
        return parent.val;
    }

    public TreeNode get_root() {
        return root;
    }

    void ensureLastRow() {
        if (row1.size() == 1 << (level - 1)) {
            // 最后一层已满
            row2 = row1;
            row1 = new ArrayList<>();
            level++;
        }
    }
}
```

### 复杂度分析

{{< figure src="https://pic.leetcode-cn.com/1658717668-jwxakP-image.png" >}}

- 初始化：$O(n)$，$n$ 为二叉树结点数量，执行了一遍 BFS。
- `insert`：$O(1)$。

---

## 方法三：队列存储出度小于 2 的结点（即左右孩子不全有）

### 解题思路

1. 初始化。通过 BFS 遍历树，保留遍历队列，找到第一个左孩子或右孩子不存在的结点为止。
2. 插入。队列的队头结点即为父结点，若新插入结点为右孩子则从队列中删除父结点，然后将新插入结点加入队列。

### 代码

```java
class CBTInserter {
    TreeNode root;
    Queue<TreeNode> que = new ArrayDeque<>();

    public CBTInserter(TreeNode root) {
        this.root = root;
        que.offer(root);
        while (!que.isEmpty()) {
            TreeNode node = que.peek();
            if (node.left != null) que.offer(node.left);
            else break;
            if (node.right != null) {
                que.offer(node.right);
                que.poll();
            } else break;
        }
    }

    public int insert(int val) {
        TreeNode node = new TreeNode(val);
        TreeNode parent = que.peek();
        if (parent.left == null) parent.left = node;
        else {
            parent.right = node;
            que.poll();
        }
        que.offer(node);
        return parent.val;
    }

    public TreeNode get_root() {
        return root;
    }
}
```

### 复杂度分析

{{< figure src="https://pic.leetcode-cn.com/1658718645-jtiElx-image.png" >}}

- 初始化：$O(n)$，$n$ 为二叉树结点数量，执行了一遍 BFS。
- `insert`：$O(1)$。

---

## 方法四：根据结点的序号的二进制查找结点

### 解题思路

根据完全二叉树可以通过数组存储的性质，定义根结点序号为 1，则序号为`k`的结点的父结点序号为`k/2`，左孩子序号为`2*k`，右孩子序号为`2*k+1`。

将序号进行二进制表示（忽略最高位的 1），`0`表示取左子树，`1`取右子树，则该二进制为根结点到指定序号结点的路径。

例：序号为 5 (二进制为 0b1**01**) 的结点为根结点的左孩子(0)的右孩子(1)。

1. 初始化。通过 DFS 遍历树计算结点数量`n`。
2. 插入。待插入结点为第`n+1`个结点，通过其序号二进制找到父结点。

### 代码

```java
class CBTInserter {
    TreeNode root;
    int n; // 结点数

    public CBTInserter(TreeNode root) {
        this.root = root;
        dfs(root);
    }

    public int insert(int val) {
        String path = Integer.toBinaryString(++n);
        TreeNode node = new TreeNode(val);
        TreeNode parent = root;
        // 忽略最高位 1；倒数第二位为父结点
        for (int i = 1; i + 1 < path.length(); i++) {
            if (path.charAt(i) == '0') parent = parent.left;
            else parent = parent.right;
        }
        if (parent.left == null) parent.left = node;
        else parent.right = node;
        return parent.val;
    }

    public TreeNode get_root() {
        return root;
    }

    void dfs(TreeNode root) {
        if (root == null) return;
        n++;
        dfs(root.left);
        dfs(root.right);
    }
}
```

### 复杂度分析

{{< figure src="https://pic.leetcode-cn.com/1658719704-lxHKls-image.png" >}}

- 初始化：$O(n)$，$n$ 为二叉树结点数量，执行了一遍 DFS。
- `insert`：$O(\log{n})$，$n$ 个结点的完全二叉树的最大高度为 $\log_2{n}$。
