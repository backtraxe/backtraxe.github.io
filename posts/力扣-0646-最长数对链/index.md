# 力扣 0646 🟨最长数对链


[646. 最长数对链](https://leetcode.cn/problems/maximum-length-of-pair-chain/)

<!--more-->

## 动态规划

- $dp[i]$ 表示以 $pairs[i]$ 结尾的最长数对链的长度。
- 时间复杂度：$ O(n^2) $

```java
class Solution {
    public int findLongestChain(int[][] pairs) {
        int n = pairs.length;
        int[] dp = new int[n];
        Arrays.sort(pairs, (a, b) -> a[0] - b[0]);
        for (int i = 0; i < n; i++) {
            int maxLen = 0;
            for (int j = 0; j < i; j++)
                if (pairs[j][1] < pairs[i][0])
                    maxLen = Math.max(maxLen, dp[j]);
            dp[i] = maxLen + 1;
        }
        return dp[n - 1];
    }
}
```

## 动态规划

- $dp[i]$ 表示长度为 $i + 1$ 的数对链的末尾最小数字。
- 时间复杂度：$ O(n \log n) $

```java
class Solution {
    public int findLongestChain(int[][] pairs) {
        int n = pairs.length;
        int[] dp = new int[n];
        Arrays.fill(dp, Integer.MAX_VALUE);
        int ans = 0;
        Arrays.sort(pairs, (a, b) -> a[0] - b[0]);
        for (int[] p : pairs) {
            // 查找第一个大于等于 p[0] 的下标
            int l = 0;
            int r = n;
            while (l < r) {
                int m = l + (r - l) / 2;
                if (dp[m] >= p[0]) r = m;
                else l = m + 1;
            }
            dp[l] = Math.min(dp[l], p[1]);
            ans = Math.max(ans, l + 1);
        }
        return ans;
    }
}
```

## 贪心

- 每次选择第二个数字更小的数对加入数对链。
- 时间复杂度：$ O(n \log n) $

```java
class Solution {
    public int findLongestChain(int[][] pairs) {
        Arrays.sort(pairs, (a, b) -> a[0] - b[0]);
        int[] pre = pairs[0];
        int ans = 1;
        for (int[] p : pairs) {
            if (pre[1] < p[0] || pre[1] > p[1]) {
                if (pre[1] < p[0]) ans++;
                pre = p;
            }
        }
        return ans;
    }
}
```

```java
class Solution {
    public int findLongestChain(int[][] pairs) {
        // 最长数对链的末尾最小数字。
        int tail = Integer.MIN_VALUE;
        int ans = 0;
        Arrays.sort(pairs, (a, b) -> a[1] - b[1]);
        for (int[] p : pairs) {
            if (tail < p[0]) {
                tail = p[1];
                ans++;
            }
        }
        return ans;
    }
}
```

