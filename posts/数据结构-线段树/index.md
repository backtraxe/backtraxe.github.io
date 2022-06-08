# 数据结构-线段树


<!--more-->

线段树是常用的用来维护**区间信息**的数据结构。

线段树可以在 $O(\log n)$ 的时间复杂度内实现单点修改、区间修改、区间查询（区间求和，求区间最大值，求区间最小值）等操作。

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

