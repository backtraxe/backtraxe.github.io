# 力扣 0459 重复的子字符串


[459. 重复的子字符串](https://leetcode.cn/problems/repeated-substring-pattern/)

<!--more-->

## 枚举子串长度

若满足题意，则字符串 $s$ 可以写成 $s_1s_1 \cdots s_1$，其中 $s_1$ 的数量为 $k$，则 $n = k * m \ (2 \le k)$，$1 \le m \le \frac{n}{2}$，其中 $n$ 为字符串 $s$ 的长度，$m$ 为子串 $s_1$ 的长度。

```java
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        int n = s.length();
        for (int len = 1; len * 2 <= n; len++) { // 枚举子串长度
            if (n % len != 0) continue; // 剪枝
            int i;
            for (i = len; i < n; i++)
                if (s.charAt(i - len) != s.charAt(i)) break;
            if (i == n) return true;
        }
        return false;
    }
}
```

## 字符串匹配

若满足要求，则字符串 $s$ 至少可以写成 $s_1s_1$，将两个 $s$ 前后拼接在一起得到 $ss = s_1s_1s_1s_1$，去掉 $ss$ 的首字符和末尾字符得到 $ss_1 = s_2s_1s_1s_2$，则字符串 $s$ 一定是 $ss_1$ 的子串。

```java
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        return (s + s).indexOf(s, 1) != s.length();
    }
}
```

