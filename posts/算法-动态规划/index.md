# 算法-动态规划


<!--more-->

## 1.基础

Dynamic Programming

**定义：**

- 存在「重叠子问题」
- 具备「最优子结构」

**步骤：**

- 确定「初始条件」
- 确定「状态」
- 确定「可选择列表」
- 确定「状态转移方程」
- 数组优化时间
- 降维优化空间

**思路：**

- 自顶向下

```python
def fib(n):
    return fib(n - 1) + fib(n - 2)
```

- 自底向上

```python
def fib(n):
    for i in range(n + 1):
        dp[i] = dp[i - 1] + dp[i - 2]
    return dp[n]
```

### 线性DP

### 二维DP

### 背包问题

### 区间DP

### 树形DP

### 状压DP

### 数位DP

## 2.实战

### 2.1 买卖股票的最佳时机

#### 2.1.1 买卖股票的最佳时机-最多1次

[121. 买卖股票的最佳时机](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/)

1. 定义状态：$dp[i]$ 表示前 $i$ 天的最大利润。
2. 状态转移：

$$
dp[i] = \max(dp[i-1], prices[i] - 前i-1天的最低价格)
$$

```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        int[] dp = new int[n];
        int minPrice = prices[0];
        for (int i = 1; i < n; i++) {
            dp[i] = Math.max(dp[i - 1], prices[i] - minPrice);
            minPrice = Math.min(minPrice, prices[i]);
        }
        return dp[n - 1];
    }
}
```

3. 空间优化：每一天的状态只与前一天的状态有关。

```java
class Solution {
    public int maxProfit(int[] prices) {
        int ans = 0;
        int minPrice = Integer.MAX_VALUE;
        for (int price : prices) {
            ans = Math.max(ans, price - minPrice);
            minPrice = Math.min(minPrice, price);
        }
        return ans;
    }
}
```

#### 2.1.2 买卖股票的最佳时机-无次数限制

[122. 买卖股票的最佳时机 II](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/)

1. 定义状态：$dp[i][0]$ 表示第 $i$ 天未持有股票的最大利润，$dp[i][1]$ 表示第 $i$ 天持有股票的最大利润。
2. 状态转移：

    - 未持有->未持有。
    - 未持有->持有。（购买）
    - 持有->未持有。（出售）
    - 持有->持有。

$$
dp[i][0] = \max(dp[i-1][0],dp[i-1][1] + prices[i]) \newline
dp[i][1] = \max(dp[i-1][1],dp[i-1][0] - prices[i])
$$

```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        int[][] dp = new int[n][2];
        dp[0][1] = -prices[0];
        for (int i = 1; i < n; i++) {
            dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + prices[i]);
            dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] - prices[i]);
        }
        return dp[n - 1][0];
    }
}
```

3. 空间优化：每一天的状态只与前一天的状态有关。

```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        // 最开始不可能持有股票，状态不存在，置为负无穷
        int[] dp = { 0, Integer.MIN_VALUE };
        for (int price : prices) {
            int[] temp = {
                Math.max(dp[0], dp[1] + price),
                Math.max(dp[1], dp[0] - price)
            };
            dp = temp;
        }
        return dp[0];
    }
}
```

#### 2.1.3 买卖股票的最佳时机-最多2次

[123. 买卖股票的最佳时机 III](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iii/)

1. 定义状态：

    - $dp[i][0][0]$ 表示第 $i$ 天**未持有**股票且前 $i$ 天未进行过购买的最大利润。
    - $dp[i][1][0]$ 表示第 $i$ 天**未持有**股票且前 $i$ 天进行过 1 次购买的最大利润。
    - $dp[i][2][0]$ 表示第 $i$ 天**未持有**股票且前 $i$ 天进行过 2 次购买的最大利润。
    - $dp[i][0][1]$ 表示第 $i$ 天**持有**股票且前 $i$ 天未进行过购买的最大利润。
    - $dp[i][1][1]$ 表示第 $i$ 天**持有**股票且前 $i$ 天进行过 1 次购买的最大利润。
    - $dp[i][2][1]$ 表示第 $i$ 天**持有**股票且前 $i$ 天进行过 2 次购买的最大利润。

2. 状态转移：

    - 未持有->未持有。
    - 未持有->持有。（购买，交易数+1）
    - 持有->未持有。（出售）
    - 持有->持有。

$$
\begin{aligned}
dp[i][0][0] &= dp[i-1][0][0] \newline
dp[i][1][0] &= \max(dp[i-1][1][0],dp[i-1][1][1] + prices[i]) \newline
dp[i][2][0] &= \max(dp[i-1][2][0],dp[i-1][2][1] + prices[i]) \newline
dp[i][0][1] &= -\infty \newline
dp[i][1][1] &= \max(dp[i-1][1][1],dp[i-1][0][0] - prices[i]) \newline
dp[i][2][1] &= \max(dp[i-1][2][1],dp[i-1][1][0] - prices[i]) \newline
\end{aligned}
$$

```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        int[][][] dp = new int[n][3][2];
        dp[0][0][1] = -100000; // 防止溢出
        dp[0][1][1] = -prices[0];
        dp[0][2][1] = -prices[0];
        for (int i = 1; i < n; i++) {
            dp[i][0][0] = dp[i - 1][0][0];
            dp[i][1][0] = Math.max(dp[i - 1][1][0], dp[i - 1][1][1] + prices[i]);
            dp[i][2][0] = Math.max(dp[i - 1][2][0], dp[i - 1][2][1] + prices[i]);
            dp[i][0][1] = -100000; // 防止溢出
            dp[i][1][1] = Math.max(dp[i - 1][1][1], dp[i - 1][0][0] - prices[i]);
            dp[i][2][1] = Math.max(dp[i - 1][2][1], dp[i - 1][1][0] - prices[i]);
        }
        return dp[n - 1][2][0];
    }
}
```

3. 空间优化：每一天的状态只与前一天的状态有关。

```java
class Solution {
    public int maxProfit(int[] prices) {
        int sell1 = 0;                 // 已经卖出 1 次的最大利润，等价于 dp[i][1][0]
        int sell2 = 0;                 // 已经卖出 2 次的最大利润，等价于 dp[i][2][0]
        int buyy1 = Integer.MIN_VALUE; // 已经买入 1 次的最大利润，等价于 dp[i][1][1]
        int buyy2 = Integer.MIN_VALUE; // 已经买入 2 次的最大利润，等价于 dp[i][2][1]
        for (int price : prices) {
            sell2 = Math.max(sell2, buyy2 + price);
            buyy2 = Math.max(buyy2, sell1 - price);
            sell1 = Math.max(sell1, buyy1 + price);
            buyy1 = Math.max(buyy1, -price);
        }
        return sell2;
    }
}
```

#### 2.1.4 买卖股票的最佳时机-最多k次

[188. 买卖股票的最佳时机 IV](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iv/)

1. 定义状态：

    - $dp[i][j][0]$ 表示第 $i$ 天**未持有**股票且前 $i$ 天进行过 $j$ 次购买的最大利润。
    - $dp[i][j][1]$ 表示第 $i$ 天**持有**股票且前 $i$ 天进行过 $j$ 次购买的最大利润。

2. 状态转移：

    - 未持有->未持有。
    - 未持有->持有。（购买，交易数+1）
    - 持有->未持有。（出售）
    - 持有->持有。

$$
\begin{aligned}
dp[i][0][0] &= dp[i-1][0][0] \newline
dp[i][j][0] &= \max(dp[i-1][j][0],dp[i-1][j][1] + prices[i]) \ (j \gt 0) \newline
dp[i][0][1] &= -\infty \newline
dp[i][j][1] &= \max(dp[i-1][j][1],dp[i-1][j-1][0] - prices[i]) \ (j \gt 0) \newline
\end{aligned}
$$

```java
class Solution {
    public int maxProfit(int k, int[] prices) {
        int n = prices.length;
        if (n == 0) return 0;
        int[][][] dp = new int[n][k + 1][2];
        dp[0][0][1] = -100000; // 防止溢出
        for (int i = 1; i <= k; i++) dp[0][i][1] = -prices[0];
        for (int i = 1; i < n; i++) {
            dp[i][0][0] = dp[i - 1][0][0];
            dp[i][0][1] = -100000; // 防止溢出
            for (int j = 1; j <= k; j++) {
                dp[i][j][0] = Math.max(dp[i - 1][j][0], dp[i - 1][j][1] + prices[i]);
                dp[i][j][1] = Math.max(dp[i - 1][j][1], dp[i - 1][j - 1][0] - prices[i]);
            }
        }
        return dp[n - 1][k][0];
    }
}
```

3. 空间优化：每一天的状态只与前一天的状态有关。

```java
class Solution {
    public int maxProfit(int k, int[] prices) {
        int n = prices.length;
        if (n == 0) return 0;
        int[][] dp = new int[k + 1][2];
        for (int i = 0; i <= k; i++)
            dp[i][1] = -100000; // 防止溢出
        for (int price : prices) {
            for (int j = 1; j <= k; j++) {
                dp[j][0] = Math.max(dp[j][0], dp[j][1] + price);
                dp[j][1] = Math.max(dp[j][1], dp[j - 1][0] - price);
            }
        }
        return dp[k][0];
    }
}
```

#### 2.1.5 买卖股票的最佳时机-冷冻期

[309. 最佳买卖股票时机含冷冻期](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

1. 定义状态：

    - $dp[i][j][0]$ 表示第 $i$ 天**未持有**股票且前 $i$ 天进行过 $j$ 次购买的最大利润。
    - $dp[i][j][1]$ 表示第 $i$ 天**持有**股票且前 $i$ 天进行过 $j$ 次购买的最大利润。

2. 状态转移：

    - 未持有->未持有。
    - 未持有->持有。（购买，交易数+1）
    - 持有->未持有。（出售）
    - 持有->持有。

$$
\begin{aligned}
dp[i][0][0] &= dp[i-1][0][0] \newline
dp[i][j][0] &= \max(dp[i-1][j][0],dp[i-1][j][1] + prices[i]) \ (j \gt 0) \newline
dp[i][0][1] &= -\infty \newline
dp[i][j][1] &= \max(dp[i-1][j][1],dp[i-1][j-1][0] - prices[i]) \ (j \gt 0) \newline
\end{aligned}
$$

```java

```

3. 空间优化：每一天的状态只与前一天的状态有关。

```java

```

#### 2.1.6 买卖股票的最佳时机-手续费

[714. 买卖股票的最佳时机含手续费](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)

1. 定义状态：

    - $dp[i][j][0]$ 表示第 $i$ 天**未持有**股票且前 $i$ 天进行过 $j$ 次购买的最大利润。
    - $dp[i][j][1]$ 表示第 $i$ 天**持有**股票且前 $i$ 天进行过 $j$ 次购买的最大利润。

2. 状态转移：

    - 未持有->未持有。
    - 未持有->持有。（购买，交易数+1）
    - 持有->未持有。（出售）
    - 持有->持有。

$$
\begin{aligned}
dp[i][0][0] &= dp[i-1][0][0] \newline
dp[i][j][0] &= \max(dp[i-1][j][0],dp[i-1][j][1] + prices[i]) \ (j \gt 0) \newline
dp[i][0][1] &= -\infty \newline
dp[i][j][1] &= \max(dp[i-1][j][1],dp[i-1][j-1][0] - prices[i]) \ (j \gt 0) \newline
\end{aligned}
$$

```java

```

3. 空间优化：每一天的状态只与前一天的状态有关。

```java

```

### 最长递增子序列（LIS）

Longest Increasing Subsequence

**动态规划**

```java
int lengthOfLIS(int[] nums) {
    int n = nums.length;
    int[] dp = new int[n];
    int ans = 1;
    for (int i = 0; i < n; i++) {
        dp[i] = 1;
        for (int j = 0; j < i; j++) {
            if (nums[i] > nums[j]) {
                dp[i] = Math.max(dp[i], dp[j] + 1);
                ans = Math.max(ans, dp[i]);
            }
        }
    }
    return ans;
}
```

- $O(n^2)$

**贪心 + 二分查找**

```java
/**
 * list 中每个链表都是降序排列，只能将更小的值加入末尾
 * 若均无法添加，则作为一个新链表加入 list
 * 二分查找找到能够添加的下标最小的链表，然后加入
 * 最后保证了每个链表的尾结点单调递增
 * list 的大小即为最长子序列的长度
 */
int lengthOfLIS(int[] nums) {
    int[] top = new int[nums.length];
    int size = 0;
    for (int num : nums) {
        int left = 0;
        int right = size;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (top[mid] < num) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        top[size] = num;
        if (left == size) {
            size++;
        }
    }
    return size;
}
```

**二分查找**

```java
/**
 * 数组 d[i]，表示长度为 i 的最长上升子序列的末尾元素的最小值
 * 用 len 记录目前最长上升子序列的长度
 */
int lengthOfLIS(int[] nums) {
    int[] dis = new int[nums.length];
    int len = 0;
    for (int num : nums) {
        int left = 0;
        int right = len;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (dis[mid] < num) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        dis[left] = num;
        if (left == len) {
            len++;
        }
    }
    return len;
}
```

### 统计不同回文子序列

[力扣-0730-统计不同回文子序列](../../posts/力扣-0730-统计不同回文子序列/)

### 火柴拼正方形

[473. 火柴拼正方形](https://leetcode.cn/problems/matchsticks-to-square/)

- 状压DP

```java
class Solution {
    public boolean makesquare(int[] matchsticks) {
        int totalLen = Arrays.stream(matchsticks).sum();
        if (totalLen % 4 != 0) {
            return false;
        }
        int len = totalLen / 4, n = matchsticks.length;
        int[] dp = new int[1 << n];
        Arrays.fill(dp, -1);
        dp[0] = 0;
        for (int s = 1; s < (1 << n); s++) {
            for (int k = 0; k < n; k++) {
                if ((s & (1 << k)) == 0) {
                    continue;
                }
                int s1 = s & ~(1 << k);
                if (dp[s1] >= 0 && dp[s1] + matchsticks[k] <= len) {
                    dp[s] = (dp[s1] + matchsticks[k]) % len;
                    break;
                }
            }
        }
        return dp[(1 << n) - 1] == 0;
    }
}
```

