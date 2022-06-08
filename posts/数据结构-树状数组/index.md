# 数据结构-树状数组


<!--more-->

树状数组是一种可以动态维护序列**前缀和**的数据结构（下标从 1 开始），它的功能是：

- 单点修改`add(index, val)`：把序列第`index`个元素增加`val`
- 区间查询`preSum(index)`：查询前`index`个元素的前缀和

#### 查询前缀和

```java
class TreeArray {
    private int[] tree; // sum(nums[i])
    private int n;

    public TreeArray(int[] nums) {
        n = nums.length + 1;
        tree = new int[n];
        for (int i = 0; i < n - 1; i++) {
            add(i, nums[i]);
        }
    }

    public void add(int index, int val) {
        // 下标从 1 开始
        index++;
        // 单点修改，增加数组 index 元素的值
        while (index < n) {
            tree[index] += val;
            // 更新父结点
            index += lowBit(index);
        }
    }

    public int preSum(int index) {
        // 查询前缀和
        int sum = 0;
        while (index > 0) {
            sum += tree[index];
            // 查询子结点
            index -= lowBit(index);
        }
        return sum;
    }

    private static int lowBit(int x) {
        // 返回 x 二进制最低位 1 的值
        // eg. 6(0b110) 返回 2(0b010)
        return x & (-x);
    }
}
```

#### 复杂度分析

- 时间复杂度：
    - 构造函数：$O(n \log n)$
    - `add`函数：$O(\log n)$
    - `preSum`函数：$O(\log n)$
- 空间复杂度：$O(n)$

