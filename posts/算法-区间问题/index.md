# 算法-区间问题


<!--more-->

## 区间重叠

<br />

## 区间合并

[56. 合并区间](https://leetcode.cn/problems/merge-intervals/)

**方法一：排序**

- 按照区间的左端点升序排序。
- 若下个区间的左端点**小于等于**上个区间的右端点，则合并两个区间，即更新上个区间的右端点为两个区间右端点的最大值。
- 否则直接添加，不用合并。

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        int[][] ans = new int[intervals.length][2];
        int lastIdx = -1;
        for (int[] interval: intervals) {
            if (lastIdx == -1 || interval[0] > ans[lastIdx][1]) {
                ans[++lastIdx] = interval;
            } else {
                ans[lastIdx][1] = Math.max(ans[lastIdx][1], interval[1]);
            }
        }
        return Arrays.copyOf(ans, lastIdx + 1);
    }
}
```

<br />

**方法二：位图**

- 位图上的 1 表示在区间内，0 表示不在区间内。
- 将所有区间表示在位图上，最后每个连续的全 1 区间即为合并后的区间。

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        BitSet bitSet = new BitSet();
        int maxRight = 0;
        for (int[] interval : intervals) {
            // 乘 2 为了使像 [1, 2] 和 [3, 4] 这样的区间不连续
            // [1, 2] -> [2, 4]
            // [3, 4] -> [6, 8]
            // [left, right)
            int left = interval[0] * 2;
            int right = interval[1] * 2 + 1;
            bitSet.set(left, right, true);
            maxRight = Math.max(maxRight, right);
        }
        int left = 0;
        int count = 0;
        while (left < maxRight) {
            // [left, right)
            left = bitSet.nextSetBit(left);
            int right = bitSet.nextClearBit(left);
            intervals[count][0] = left / 2;
            intervals[count][1] = right / 2;
            count++;
            left = right;
        }
        return Arrays.copyOf(intervals, count);
    }
}
```

<br />

## 插入区间

## 参考

- [秒懂力扣区间题目：重叠区间、合并区间、插入区间](https://mp.weixin.qq.com/s/ioUlNa4ZToCrun3qb4y4Ow)

