# 数据结构-线段树


<!--more-->

## 1. 介绍

线段树是常用的用来维护**区间信息**的数据结构，在区间动态修改的同时还能够高效查询区间信息。

<img src="/imgs/线段树/线段树01.svg">

**功能：**

1. 单点修改
    - 第`i`个元素增加`x`
    - 第`i`个元素减少`x`
    - 第`i`个元素修改为`x`
1. 区间修改
    - 区间`[i, j]`中的元素都增加`x`
    - 区间`[i, j]`中的元素都减少`x`
    - 区间`[i, j]`中的元素都修改为`x`
1. 区间查询
    - 查询区间`[i, j]`中的元素和
    - 查询区间`[i, j]`中的元素最大值
    - 查询区间`[i, j]`中的元素最小值
    - 查询区间`[i, j]`中的元素最大公因数

**结构：**

- 每个结点代表一个区间，结点的值根据问题进行定义（区间和、区间最大值等）。
- 结点的左右孩子平分父结点的区间。

<br />

## 2. 实现

### 2.1 基于链表实现

- **结点定义**

```java
class Node {
    int val;
    Node left, right;

    public Node() {}

    public Node(int val) {
        this.val = val;
    }
}
```

- **建树**

```java
void buildTree(Node node, int start, int end) {
    //
    
}
```

<br />

### 2.2 基于数组实现

```java
class SegmentTree {
    private int[] tree; // 维护区间
    private int[] lazy; // 惰性标记，维护修改值

    public SegmentTree(int[] nums) {
        int n = nums.length;
        // 线断树有 n 个叶结点，总结点个数设为 x
        // 非叶结点都有 2 棵子树，则
        // x - 1 = (x - n) * 2
        // x = 2 * n - 1
        // 根结点索引从 1 开始
        tree = new int[n * 2];
        lazy = new int[n * 2];
        build(nums, 0, nums.length - 1, 1);
    }

    void build(int[] nums, int left, int right, int root) {
        // 对 [left, right] 区间建立线段树,当前根的编号为 root
        if (left == right) {
            // 叶结点
            tree[root] = nums[left];
            return;
        }
        // + 优先级高于 >>
        int mid = left + ((right - left) >> 1);
        build(nums, left, mid, root << 1);          // root * 2
        // << 优先级高于 |
        build(nums, mid + 1, right, root << 1 | 1); // root * 2 + 1
        // 子树信息更新父结点信息
        tree[root] = tree[root << 1] + tree[root << 1 | 1];
    }

    int rangeSum(int qLeft, int qRight, int left, int right, int root) {
        // 查询区间 [qLeft, qRight] 的元素总和
        if (qLeft <= left && right <= qRight) {
            // 当前区间是查询区间的子区间
            return tree[root];
        }
        pushDown(left, right, root);
        int mid = left + ((right - left) >> 1);
        int sum = 0;
        if (qLeft <= mid) {
            sum += rangeSum(qLeft, qRight, left, mid, root << 1);
        }
        if (mid < qRight) {
            sum += rangeSum(qLeft, qRight, mid + 1, right, root << 1 | 1);
        }
        return sum;
    }

    void update(int qLeft, int qRight, int val, int left, int right, int root) {
        // 将区间 [qLeft, qRight] 内的元素加 val
        if (qLeft <= left && right <= qRight) {
            tree[root] += val * (right - left + 1);
            // 不必此时修改到叶结点，将修改值记录到父结点的 lazy 标记
            // 下次查询到需要修改的叶结点时再修改
            lazy[root] += val;
            return;
        }
        pushDown(left, right, root);
        int mid = left + ((right - left) >> 1);
        if (qLeft <= mid) {
            update(qLeft, qRight, val, left, mid, root << 1);
        }
        if (mid < qRight) {
            update(qLeft, qRight, val, mid + 1, right, root << 1 | 1);
        }
        tree[root] = tree[root << 1] + tree[root << 1 | 1];
    }

    private void pushDown(int left, int right, int root) {
        // lazy 标记处理
        int mid = left + ((right - left) >> 1);
        if (lazy[root] > 0 && left < right) {
            // 如果当前节点的 lazy 标记非空，则更新当前节点两个子节点的值和 lazy 标记
            tree[root << 1] += lazy[root] * (mid - left + 1);
            tree[root << 1 | 1] += lazy[root] * (right - mid);
            lazy[root << 1] += lazy[root];
            lazy[root << 1 | 1] += lazy[root];
            lazy[root] = 0;
        }
    }
}
```

<br />

## 3. 分析

<br />

## 参考

- [线段树详解「汇总级别整理 🔥🔥🔥」 - 我的日程安排表 I - 力扣（LeetCode）](https://leetcode.cn/problems/my-calendar-i/solution/by-lfool-xvpv/)

