# 力扣 0005 最长回文子串


[5. 最长回文子串](https://leetcode.cn/problems/longest-palindromic-substring/)

<!--more-->

## 动态规划

```java
class Solution {
    public String longestPalindrome(String s) {
        int n = s.length();
        // [start, end)
        int start = 0;
        int end = 1;
        // dp[i][j] 表示 s[i:j+1] 是否是回文串
        boolean[][] dp = new boolean[n][n];
        for (int i = 0; i < n; i++) {
            dp[i][i] = true;
            if (i > 0 && s.charAt(i - 1) == s.charAt(i)) {
                dp[i - 1][i] = true;
                start = i - 1;
                end = i + 1;
            }
        }
        for (int len = 2; len < n; len++) {
            for (int i = 0; i + len < n; i++) {
                int j = i + len;
                dp[i][j] = dp[i + 1][j - 1] && s.charAt(i) == s.charAt(j);
                if (dp[i][j]) {
                    start = i;
                    end = j + 1;
                }
            }
        }
        return s.substring(start, end);
    }
}
```

- 时间复杂度：$ O(n^2) $
- 空间复杂度：$ O(n^2) $

## 中心扩散

```java
class Solution {
    public String longestPalindrome(String s) {
        int n = s.length();
        int start = 0;
        int end = 0;
        for (int i = 0; i < n; i++) {
            int l = i;
            while (l >= 0 && s.charAt(l) == s.charAt(i)) l--;
            int r = i;
            while (r < n && s.charAt(r) == s.charAt(i)) r++;
            while (l >= 0 && r < n && s.charAt(l) == s.charAt(r)) {
                l--;
                r++;
            }
            if (r - l - 2 > end - start) {
                start = l + 1;
                end = r - 1;
            }
        }
        return s.substring(start, end + 1);
    }
}
```

- 时间复杂度：$ O(n^2) $
- 空间复杂度：$ O(1) $

## Manacher 算法

