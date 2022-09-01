# 力扣 2209 🟥用地毯覆盖后的最少白色砖块


[2209. 用地毯覆盖后的最少白色砖块](https://leetcode.cn/problems/minimum-white-tiles-after-covering-with-carpets/)

<!--more-->

## 动态规划

### 状态定义

$dp[i][j]$ 表示用 $i$ 块地毯铺前 $j$ 块砖块剩余最少白色砖块数量。

### 边界条件

不使用地毯，则前 $j$ 块砖块剩余最少白色砖块数量为白色砖块的数量。

$$
dp[0][j] = dp[0][j - 1] + I(s[j] == '1')
$$

### 状态转移方程

- 不铺地毯：若第 $i$ 块砖块为白色砖块，则剩余白色砖块数量加一。
- 铺地毯：用一个地毯覆盖砖块，并且其末尾刚好覆盖住第 $i$ 块砖块。

$$
dp[i][j] = \min(dp[i - 1][j] + I(s[j] == '1'), dp[i - len][j - 1])
$$

### 递推代码

```java
class Solution {
    public int minimumWhiteTiles(String floor, int numCarpets, int carpetLen) {
        int n = floor.length();
        // 能够覆盖所有砖块
        if (numCarpets * carpetLen >= n) return 0;
        // dp[i][j] 表示用 i 块地毯覆盖前 j 块砖块剩余最少白色砖块数量。
        int[][] dp = new int[numCarpets + 1][n];
        // 不铺地毯时前 j 块砖块剩余最少白色砖块数量为前 j 块砖块中白色砖块的数量。
        dp[0][0] = floor.charAt(0) == '1' ? 1 : 0;
        for (int j = 1; j < n; j++)
            dp[0][j] = dp[0][j - 1] + (floor.charAt(j) == '1' ? 1 : 0);
        // 不铺地毯时，若第 j 块砖块为白色砖块，则剩余白色砖块数量加一。
        // 铺地毯时，用地毯的边缘覆盖第 j 块砖块。
        // 当铺第 i 块地毯时，至少可以覆盖 i * carpetLen 块砖块，所以少于。
        for (int i = 1; i <= numCarpets; i++)
            for (int j = i * carpetLen; j < n; j++)
                dp[i][j] = Math.min(dp[i][j - 1] + (floor.charAt(j) == '1' ? 1 : 0), dp[i - 1][j - carpetLen]);
        // 返回用 numCarpets 块地毯覆盖前 n - 1 块砖块剩余最少白色砖块数量。
        return dp[numCarpets][n - 1];
    }
}
```

### 空间优化递推代码

```java
class Solution {
    public int minimumWhiteTiles(String floor, int numCarpets, int carpetLen) {
        int n = floor.length();
        if (numCarpets * carpetLen >= n) return 0;
        int[] pre = new int[n];
        int[] cur = new int[n];
        pre[0] = floor.charAt(0) == '1' ? 1 : 0;
        for (int i = 1; i < n; i++)
            pre[i] = pre[i - 1] + (floor.charAt(i) == '1' ? 1 : 0);
        for (int i = 1; i <= numCarpets; i++) {
            cur[i * carpetLen - 1] = 0; // 设置起点为 0
            for (int j = i * carpetLen; j < n; j++)
                cur[j] = Math.min(cur[j - 1] + (floor.charAt(j) == '1' ? 1 : 0), pre[j - carpetLen]);
            int[] tmp = cur; // 交换，避免创建新数组
            cur = pre;
            pre = tmp;
        }
        return pre[n - 1];
    }
}
```

## 参考

1. [动态规划的典型思考策略（Python/Java/C++/Go） - 用地毯覆盖后的最少白色砖块 - 力扣（LeetCode）](https://leetcode.cn/problems/minimum-white-tiles-after-covering-with-carpets/solution/by-endlesscheng-pa3v/)

