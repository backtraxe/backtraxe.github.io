# 算法-前缀和


<!--more-->

## 1.介绍

- 快速求出数组中某一段**连续区间的和**。
- 前缀和数组长度为原数组**长度加 1**

```text
     nums = {    1, 2, 3,  4,  5 }
prefixSum = { 0, 1, 3, 6, 10, 15 }

nums[l] + nums[l + 1] + ... + nums[r] = prefixSum[r + 1] - prefixSum[l]
```

```java
static int[] getPrefixSum(int[] nums) {
    int n = nums.length;
    int[] pre = new int[n + 1];
    for (int i = 0; i < n; i++) pre[i + 1] = pre[i] + nums[i];
    return pre;
}
```

## 2.实战

### 🟨和为 K 的子数组

[560. 和为 K 的子数组](https://leetcode.cn/problems/subarray-sum-equals-k/)

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int n = nums.length;
        int[] pre = new int[n + 1];
        for (int i = 0; i < n; i++) pre[i + 1] = pre[i] + nums[i];
        HashMap<Integer, Integer> map = new HashMap<>();
        int ans = 0;
        for (int x : pre) {
            ans += map.getOrDefault(x - k, 0); // pre[j + 1] - pre[i] == k
            map.put(x, map.getOrDefault(x, 0) + 1);
        }
        return ans;
    }
}
```

### 🟨除自身以外数组的乘积

[238. 除自身以外数组的乘积](https://leetcode.cn/problems/product-of-array-except-self/)

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] ans = new int[n];
        Arrays.fill(ans, 1);
        int left = 1;
        int right = 1;
        for (int i = 0; i < n; i++) {
            ans[i] *= left;
            left *= nums[i];
            ans[n - 1 - i] *= right;
            right *= nums[n - 1 - i];
        }
        return ans;
    }
}
```

## 参考

