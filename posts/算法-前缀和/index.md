# 算法-前缀和


<!--more-->

## 1.介绍

- 前缀和数组长度为原数组**长度加 1**
- `nums[left] + nums[left + 1] + ... + nums[right] = prefixSum[right + 1] - prefixSum[left]`

$$
\sum_{i=left}^{right}nums[i]=prefixSum[right + 1]-prefixSum[left]
$$

<br />

## 2.结果

```text
1 2 3 4 5     // nums
0 1 3 6 10 15 // prefixSum
```

<br />

## 3.代码

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

