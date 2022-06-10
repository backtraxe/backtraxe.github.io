# 力扣 0730 统计不同回文子序列


[730. 统计不同回文子序列](https://leetcode.cn/problems/count-different-palindromic-subsequences/)

<!--more-->

## 方法一：三维DP

**状态定义：**

- `dp[x][i][j]`表示在字符串区间`s[i:j]（包含j）`中以字符`x`开头和结尾的不同回文序列数量。
- 则答案为求 $\sum_{i=0}^Cdp[x_i][0][n-1] \bmod 1000000007$，其中 $x_i \in S$，$S$ 为字符串中出现的字符集合，$C$ 为该字符集合的大小。

**状态转移方程：**

- `s[i]==x && s[j]==x`时，`s[i+1:j-1]`中的回文序列加上`s[i]`和`s[j]`会构成新的以`x`开头和结尾的回文序列。再加上`xx`和`x`两个回文序列。

$$
dp[x][i][j]=2+\sum_{k=0}^Cdp[x_k][i+1][j-1]
$$

- `s[i]==x && s[j]!=x`时

$$
dp[x][i][j]=dp[x][i][j-1]
$$

- `s[i]!=x && s[j]==x`时

$$
dp[x][i][j]=dp[x][i+1][j]
$$

- `s[i]!=x && s[j]!=x`时

$$
dp[x][i][j]=dp[x][i+1][j-1]
$$

**边界条件：**

- 当`i==j && s[i]==x`时，`dp[x][i][j]=1`
- 当`i==j && s[i]!=x`时，`dp[x][i][j]=0`
- 当`i>j`时，`dp[x][i][j]=0`

**代码实现：**

```java
class Solution {
    static final int MOD = (int) 1e9 + 7;

    public int countPalindromicSubsequences(String s) {
        int n = s.length();
        int[][][] dp = new int[4][n][n];
        // 初始化边界条件
        for (int i = 0; i < n; i++) {
            dp[s.charAt(i) - 'a'][i][i] = 1;
        }
        // 遍历 i 和 j
        for (int step = 1; step < n; step++) {
            for (int i = 0; i + step < n; i++) {
                int j = i + step;
                int x1 = s.charAt(i) - 'a';
                int x2 = s.charAt(j) - 'a';
                // 状态转移
                for (int x = 0; x < 4; x++) {
                    if (x1 == x && x2 == x) {
                        dp[x][i][j] = 2;
                        for (int k = 0; k < 4; k++) {
                            dp[x][i][j] = (dp[x][i][j] + dp[k][i + 1][j - 1]) % MOD;
                        }
                    } else if (x1 == x && x2 != x) {
                        dp[x][i][j] = dp[x][i][j - 1];
                    } else if (x1 != x && x2 == x) {
                        dp[x][i][j] = dp[x][i + 1][j];
                    } else {
                        dp[x][i][j] = dp[x][i + 1][j - 1];
                    }
                }
            }
        }
        int ans = 0;
        for (int x = 0; x < 4; x++) {
            ans = (ans + dp[x][0][n - 1]) % MOD;
        }
        return ans;
    }
}
```

**复杂度分析：**

- 时间复杂度：$ O(C^2 \times n^2) $
- 空间复杂度：$ O(C \times n^2) $

