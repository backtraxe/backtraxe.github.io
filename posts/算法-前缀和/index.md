# 算法-前缀和


<!--more-->

`prefixSum[high + 1] - prefixSum[low]`可以 $O(1)$ 的时间复杂度求出`nums`中`[low,high]`的区间和。

```java
int[] getPrefixSum(int[] nums) {
    int n = nums.length;
    int[] prefixSum = new int[n + 1];
    for (int i = 0; i < n; i++) {
        prefixSum[i + 1] = prefixSum[i] + nums[i];
    }
    return prefixSum;
}
```

