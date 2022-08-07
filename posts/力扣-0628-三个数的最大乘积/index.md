# 力扣 0628 三个数的最大乘积


[628. 三个数的最大乘积](https://leetcode.cn/problems/maximum-product-of-three-numbers/)

<!--more-->

## 思路

- 全是非负数：选择最大的三个非负数，即**最大的三个数**。
- 至少两个非负数，只有一个负数：
    - 至少三个非负数：选择最大的三个非负数，即**最大的三个数**。
    - 只有两个非负数：只有三个数可以选择，即**最大的三个数**。
- 至少一个非负数，只有两个负数：
    - 至少三个非负数：选择最大的三个非负数，即**最大的三个数**；或者选择两个负数和最大的一个非负数，即**最小的两个数和最大的一个数**。
    - 只有两个非负数：选择两个负数和最大的一个非负数，即**最小的两个数和最大的一个数**。
    - 只有一个非负数：只有三个数可以选择，即**最大的三个数**。
- 全是负数：选择最小的三个负数，即**最大的三个数**。

综上，结果为**最大的三个数**和**最小的两个数和最大的一个数**中的更大值。

## 代码

- 排序

```java
class Solution {
    public int maximumProduct(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        return Math.max(
            nums[n - 1] * nums[n - 2] * nums[n - 3], // 最大的三个数
            nums[n - 1] * nums[0] * nums[1]          // 最小的两个数和最大的一个数
        );
    }
}
```

- 不排序

```java
class Solution {
    public int maximumProduct(int[] nums) {
        int max1, max2, max3, min1, min2; // 最大的三个数、最小的两个数
        max1 = max2 = max3 = Integer.MIN_VALUE;
        min1 = min2 = Integer.MAX_VALUE;
        for (int x : nums) {
            if (x > max1) {
                max3 = max2;
                max2 = max1;
                max1 = x;
            } else if (x > max2) {
                max3 = max2;
                max2 = x;
            } else if (x > max3) {
                max3 = x;
            }
            if (x < min1) {
                min2 = min1;
                min1 = x;
            } else if (x < min2) {
                min2 = x;
            }
        }
        return Math.max(max1 * max2 * max3, max1 * min1 * min2);
    }
}
```
