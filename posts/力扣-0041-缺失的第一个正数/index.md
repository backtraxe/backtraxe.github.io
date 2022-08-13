# 力扣 0041 缺失的第一个正数


[41. 缺失的第一个正数](https://leetcode.cn/problems/first-missing-positive/)

<!--more-->

## 原地哈希

- 长度为 $n$ 的数组中未出现的最小正整数一定在 $[1,n+1]$ 之间。
- 所以可以将数组用作哈希表，用 $nums[i]$ 来标识 $i+1$ 这个正整数是否出现过。
- 由于我们只关心范围在 $[1,n]$ 之间的数，所以可以将范围之外的数当做标识，假设选用负数作为标识。
    1. 将数组中的非正数置为 $\gt n$ 的正整数。
    2. 对于数组中在 $[1,n]$ 之间的数 $nums[i]$，将 $nums[nums[i] - 1]$ 置为负数。
    3. 遍历数组找到第一个正数 $nums[i]$，则 $i+1$ 即为答案。

```java
class Solution {
    public int firstMissingPositive(int[] nums) {
        int n = nums.length;
        // 1. 保证 1 <= nums[i]
        for (int i = 0; i < n; i++)
            if (nums[i] <= 0) nums[i] = n + 1;
        // 2. 将满足 1 <= nums[i] <= n 的 nums[nums[i] - 1] 置为负数
        for (int i = 0; i < n; i++) {
            int j = Math.abs(nums[i]); // 不遗漏每个数
            if (j <= n) nums[j - 1] = -Math.abs(nums[j - 1]);
        }
        // 3. 第一个 nums[i] != -1 则返回 i + 1
        for (int i = 0; i < n; i++)
            if (nums[i] > 0) return i + 1;
        return n + 1;
    }
}
```

- 时间复杂度：$ O(n) $
- 空间复杂度：$ O(1) $

## 置换

- 将在范围 $[1,n]$ 之间的 $nums[i]$ 通过交换归位。
- 遍历数组，第一个 $nums[i] \ne i + 1$ 的 $i+1$ 即为答案。

```java
class Solution {
    public int firstMissingPositive(int[] nums) {
        int n = nums.length;
        for (int i = 0; i < n; i++) {
            int p = nums[i] - 1;
            // 将 [1, n] 中不在位的归位
            while (0 <= p && p < n && p + 1 != nums[p]) {
                int next = nums[p] - 1;
                nums[p] = p + 1;
                p = next;
            }
        }
        for (int i = 0; i < n; i++)
            if (nums[i] != i + 1) return i + 1;
        return n + 1;
    }
}
```

- 时间复杂度：$ O(n) $
- 空间复杂度：$ O(1) $

