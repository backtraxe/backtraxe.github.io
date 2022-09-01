# 算法-单调栈


<!--more-->

## 1.介绍

单调栈可以帮助快速找到每个元素**之前/之后第一个大于/大于等于/小于/小于等于当前元素**的元素或者其下标。

## 2.单调栈存储下标

### 2.1 下一个元素的下标

|说明|元素|
|:---:|:---:|
|输入|`[5, 8, 3, 1, 3, 3, 5]`|
|下个更大的元素的下标|`[1, -1, 6, 4, 6, 6, -1]`|
|下个大于等于的元素的下标|`[1, -1, 4, 4, 5, 6, -1]`|
|下个更小的元素的下标|`[2, 2, 3, -1, -1, -1, -1]`|
|下个小于等于的元素的下标|`[2, 2, 3, -1, 5, -1, -1]`|

```java
static int[] getMonoStack(int[] nums) {
    int n = nums.length;
    int[] ans = new int[n];
    Deque<Integer> st = new ArrayDeque<>();
    for (int i = n - 1; i >= 0; i--) {
        while (!st.isEmpty() && nums[st.peek()] <= nums[i]) st.pop(); // 下个更大的元素的下标
        while (!st.isEmpty() && nums[st.peek()] <  nums[i]) st.pop(); // 下个大于等于的元素的下标
        while (!st.isEmpty() && nums[st.peek()] >= nums[i]) st.pop(); // 下个更小的元素的下标
        while (!st.isEmpty() && nums[st.peek()] >  nums[i]) st.pop(); // 下个小于等于的元素的下标
        ans[i] = st.isEmpty() ? -1 : st.peek();
        st.push(i); // 存下标
    }
    return ans;
}
```

### 2.2 上一个元素的下标

|说明|元素|
|:---:|:---:|
|输入|`[5, 8, 3, 1, 3, 3, 5]`|
|上个更大的元素的下标|`[-1, -1, 1, 2, 1, 1, 1]`|
|上个大于等于的元素的下标|`[-1, -1, 1, 2, 2, 4, 1]`|
|上个更小的元素的下标|`[-1, 0, -1, -1, 3, 3, 5]`|
|上个小于等于的元素的下标|`[-1, 0, -1, -1, 3, 4, 5]`|

```java
static int[] getMonoStack(int[] nums) {
    int n = nums.length;
    int[] ans = new int[n];
    Deque<Integer> st = new ArrayDeque<>();
    for (int i = 0; i < n; i++) {
        while (!st.isEmpty() && nums[st.peek()] <= nums[i]) st.pop(); // 上个更大的元素的下标
        while (!st.isEmpty() && nums[st.peek()] <  nums[i]) st.pop(); // 上个大于等于的元素的下标
        while (!st.isEmpty() && nums[st.peek()] >= nums[i]) st.pop(); // 上个更小的元素的下标
        while (!st.isEmpty() && nums[st.peek()] >  nums[i]) st.pop(); // 上个小于等于的元素的下标
        ans[i] = st.isEmpty() ? -1 : st.peek();
        st.push(i); // 存下标
    }
    return ans;
}
```

### 2.3 上一个和下一个元素的下标

```java
static int[][] getMonoStack(int[] nums) {
    int n = nums.length;
    int[][] ans = new int[2][n];
    Arrays.fill(ans[1], -1);
    Deque<Integer> st = new ArrayDeque<>();
    for (int i = 0; i < n; i++) {
        // 上个更大的元素的下标 + 下个大于等于的元素的下标
        while (!st.isEmpty() && nums[st.peek()] <= nums[i]) ans[1][st.pop()] = i;
        // 上个大于等于的元素的下标 + 下个更大的元素的下标
        while (!st.isEmpty() && nums[st.peek()] <  nums[i]) ans[1][st.pop()] = i;
        // 上个更小的元素的下标 + 下个小于等于的元素的下标
        while (!st.isEmpty() && nums[st.peek()] >= nums[i]) ans[1][st.pop()] = i;
        // 上个小于等于的元素的下标 + 下个更小的元素的下标
        while (!st.isEmpty() && nums[st.peek()] >  nums[i]) ans[1][st.pop()] = i;
        ans[0][i] = st.isEmpty() ? -1 : st.peek();
        st.push(i); // 存下标
    }
    return ans;
}
```

## 3.单调栈存储元素

### 3.1 下一个元素

|说明|元素|
|:---:|:---:|
|输入|`[5, 8, 3, 1, 3, 3, 5]`|
|下个更大的元素|`[8, -1, 5, 3, 5, 5, -1]`|
|下个大于等于的元素|`[8, -1, 3, 3, 3, 5, -1]`|
|下个更小的元素|`[3, 3, 1, -1, -1, -1, -1]`|
|下个小于等于的元素|`[3, 3, 1, -1, 3, -1, -1]`|

```java
static int[] getMonoStack(int[] nums) {
    int n = nums.length;
    int[] ans = new int[n];
    Deque<Integer> st = new ArrayDeque<>();
    for (int i = n - 1; i >= 0; i--) {
        while (!st.isEmpty() && st.peek() <= nums[i]) st.pop(); // 下个更大的元素
        while (!st.isEmpty() && st.peek() <  nums[i]) st.pop(); // 下个大于等于的元素
        while (!st.isEmpty() && st.peek() >= nums[i]) st.pop(); // 下个更小的元素
        while (!st.isEmpty() && st.peek() >  nums[i]) st.pop(); // 下个小于等于的元素
        ans[i] = st.isEmpty() ? -1 : st.peek();
        st.push(nums[i]); // 存元素
    }
    return ans;
}
```

### 3.2 上一个元素

|说明|元素|
|:---:|:---:|
|输入|`[5, 8, 3, 1, 3, 3, 5]`|
|上个更大的元素|`[-1, -1, 8, 3, 8, 8, 8]`|
|上个大于等于的元素|`[-1, -1, 8, 3, 3, 3, 8]`|
|上个更小的元素|`[-1, 5, -1, -1, 1, 1, 3]`|
|上个小于等于的元素|`[-1, 5, -1, -1, 1, 3, 3]`|

```java
static int[] getMonoStack(int[] nums) {
    int n = nums.length;
    int[] ans = new int[n];
    Deque<Integer> st = new ArrayDeque<>();
    for (int i = 0; i < n; i++) {
        while (!st.isEmpty() && st.peek() <= nums[i]) st.pop(); // 上个更大的元素
        while (!st.isEmpty() && st.peek() <  nums[i]) st.pop(); // 上个大于等于的元素
        while (!st.isEmpty() && st.peek() >= nums[i]) st.pop(); // 上个更小的元素
        while (!st.isEmpty() && st.peek() >  nums[i]) st.pop(); // 上个小于等于的元素
        ans[i] = st.isEmpty() ? -1 : st.peek();
        st.push(nums[i]); // 存元素
    }
    return ans;
}
```

## 4.总结

```java
// 找下一个元素
for (int i = n - 1; i >= 0; i--)
// 找上一个元素
for (int i = 0; i < n; i++)

// 存下标
while (!st.isEmpty() && nums[st.peek()] ...)
st.push(i);
// 存元素
while (!st.isEmpty() && st.peek() ...)
st.push(nums[i]);

// 更大的元素
while (!st.isEmpty() && ... <= nums[i])
// 大于等于的元素
while (!st.isEmpty() && ... <  nums[i])
// 更小的元素
while (!st.isEmpty() && ... >= nums[i])
// 小于等于的元素
while (!st.isEmpty() && ... >  nums[i])
```

## 5.实战

### 🟩商品折扣后的最终价格

[1475. 商品折扣后的最终价格](https://leetcode.cn/problems/final-prices-with-a-special-discount-in-a-shop/)

```java
class Solution {
    public int[] finalPrices(int[] prices) {
        int n = prices.length;
        int[] ans = new int[n];
        // 单调栈，找到下个小于等于当前值的元素
        Deque<Integer> st = new ArrayDeque<>();
        for (int i = n - 1; i >= 0; i--) {
            while (!st.isEmpty() && st.peek() > prices[i]) st.pop();
            ans[i] = prices[i] - (st.isEmpty() ? 0 : st.peek());
            st.push(prices[i]);
        }
        return ans;
    }
}
```

### 🟩下一个更大元素 I

[496. 下一个更大元素 I](https://leetcode.cn/problems/next-greater-element-i/)

```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        int n1 = nums1.length;
        int n2 = nums2.length;
        HashMap<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < n1; i++) map.put(nums1[i], i);
        int[] nextGT = getMonoStack(nums2);
        int[] ans = new int[n1];
        for (int i = 0; i < n2; i++)
            if (map.containsKey(nums2[i]))
                ans[map.get(nums2[i])] = nextGT[i];
        return ans;
    }

    static int[] getMonoStack(int[] nums) {
        int n = nums.length;
        int[] ans = new int[n];
        Deque<Integer> st = new ArrayDeque<>();
        for (int i = n - 1; i >= 0; i--) {
            while (!st.isEmpty() && st.peek() <= nums[i]) st.pop(); // 下个更大的元素
            ans[i] = st.isEmpty() ? -1 : st.peek();
            st.push(nums[i]); // 存元素
        }
        return ans;
    }
}
```

### 🟨下一个更大元素 II

[503. 下一个更大元素 II](https://leetcode.cn/problems/next-greater-element-ii/)

```java
class Solution {
    public int[] nextGreaterElements(int[] nums) {
        int n = nums.length;
        int[] ans = new int[n * 2];
        Deque<Integer> st = new ArrayDeque<>();
        for (int i = 2 * n - 1; i >= 0; i--) {
            while (!st.isEmpty() && st.peek() <= nums[i % n]) st.pop(); // 下个更大的元素
            ans[i] = st.isEmpty() ? -1 : st.peek();
            st.push(nums[i % n]); // 存元素
        }
        return Arrays.copyOf(ans, n);
    }
}
```

### 🟨每日温度

[739. 每日温度](https://leetcode.cn/problems/daily-temperatures/)

```java
class Solution {
    public int[] dailyTemperatures(int[] temperatures) {
        int n = temperatures.length;
        int[] ans = new int[n];
        Deque<Integer> st = new ArrayDeque<>();
        for (int i = n - 1; i >= 0; i--) {
            while (!st.isEmpty() && temperatures[st.peek()] <= temperatures[i])
                st.pop(); // 下个更大的元素的下标
            ans[i] = st.isEmpty() ? 0 : st.peek() - i;
            st.push(i); // 存下标
        }
        return ans;
    }
}
```

## 参考


