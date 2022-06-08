# 算法-字符串


<!--more-->

### 1 KMP 算法

Knuth-Morris-Pratt 算法。

#### 原理

主串`text`长度为`n`，匹配串`pattern`长度为`m`。

KMP 算法首先算出一个`next`数组，匹配串每轮匹配在`j`位置失配时，匹配串向右滑动的距离为`j - next[j]`。

- `next[0] = -1`
- `j > 0`时`next[j]`为匹配串中区间`[0, j - 1]`的严格前缀子串和严格后缀子串中最长公共子串的长度。

设匹配串为`abcdabd`。

| `j` | 子串 | 严格前缀子串 | 严格后缀子串 | 最长公共子串 | `next[j]` |
|:---:|:---:|:---:|:---:|:---:|:---:|
| 0 |  |  |  |  | -1 |
| 1 | `a` |  |  |  | 0 |
| 2 | `ab` | `a` | `b` |  | 0 |
| 3 | `abc` | `a`、`ab` | `bc`、`c` |  | 0 |
| 4 | `abcd` | `a` 、 `ab`、`abc` | `bcd`、`cd` 、 `d` |  | 0 |
| 5 | `abcda` | `a` 、 `ab` 、 `abc`、`abcd` | `bcda`、`cda` 、 `da` 、 `a` | `a` | 1 |
| 6 | `abcdab` | `a` 、 `ab` 、 `abc` 、 `abcd`、`abcda` | `bcdab`、`cdab`、`dab`、`ab`、`b` | `ab` | 2 |

- 时间复杂度：`O(n + m)`

#### 代码

```java
int[] getNext(char[] pattern) {
    int m = pattern.length;
    int[] next = new int[m];
    next[0] = -1; // 特殊情况
    int i = 0;    // [0, i - 1] 区间的最长公共子串
    int j = -1;
    while (i < m - 1) {
        if (j == -1 || pattern[i] == pattern[j]) {
            i++;
            j++;
            next[i] = j;
        } else {
            j = next[j];
        }
    }
    return next;
}

int kmpSearch(char[] text, char[] pattern) {
    int n = text.length;
    int m = pattern.length;
    if (m == 0) {
        return 0;
    }
    int[] next = getNext(pattern);
    int i = 0; // 主串指针
    int j = 0; // 匹配串指针
    while (i < n && j < m) {
        if (j == -1 || text[i] == pattern[j]) {
            i++;
            j++;
        } else {
            j = next[j];
        }
    }
    return j == m ? i - j : -1;
}
```

#### 参考

1. [字符串匹配的KMP算法 - 阮一峰的网络日志](https://www.ruanyifeng.com/blog/2013/05/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm.html)
1. [如何更好地理解和掌握 KMP 算法? - 知乎](https://www.zhihu.com/question/21923021/answer/281346746)
1. [字符串匹配算法详解 - 云+社区 - 腾讯云](https://cloud.tencent.com/developer/article/1770486)

### 2 BM 算法

Boyer-Moore 算法。

#### 原理

主串`text`长度为`n`，匹配串`pattern`长度为`m`。

1. 匹配串从后往前匹配

2. 坏字符：将主串中未与匹配串匹配的第一个字符`pattern[j]`称为坏字符，然后匹配串向右滑动的距离为`j - 匹配串中该字符上次出现的位置（未出现返回-1）`。

{{< admonition tip "示例" false >}}
设匹配串为`abcdabc`。

| 字符 | 匹配串中该字符上次出现的位置 |
|:---:|:---:|
| `a` | 4 |
| `b` | 5 |
| `c` | 6 |
| `d` | 3 |
| 其他 | -1 |
{{< /admonition >}}


3. 好后缀：匹配串中已匹配的后缀子串称为好后缀，然后然后匹配串向右滑动的距离为`m - 好后缀和匹配串前缀子串的最长公共子串长度`。特殊地，当`j == m - 1`时无已匹配部分，定义`goodSuffix[m - 1] = m - 1`。

{{< admonition tip "示例" false >}}
设匹配串为`abcdabc`。

| `j` | 好后缀 | 最长公共子串 | `goodSuffix[j]` |
|:---:|:---:|:---:|:---:|
| 6 |  |  | 6 |
| 5 | `c` |  | 0 |
| 4 | `bc`、`c` |  | 0 |
| 3 | `abc`、`bc`、`c` | `abc` | 3 |
| 2 | `dabc`、`abc`、`bc`、`c` | `abc` | 3 |
| 1 | `cdabc`、`dabc`、`abc`、`bc`、`c` | `abc` | 3 |
| 0 | `bcdabc`、`cdabc` 、 `dabc` 、 `abc` 、 `bc` 、 `c` | `abc` | 3 |
{{< /admonition >}}

4. 每次匹配串向右滑动这两个规则之中的较大值。可以预处理出`badChar<char, int>`和`goodSuffix[]`。

#### 代码

```java
HashMap<Character, Integer> getBadChar(String pattern) {
    int m = pattern.length();
    // 坏字符
    HashMap<Character, Integer> badChar = new HashMap<>();
    for (int i = 0; i < m; i++) {
        badChar.put(pattern.charAt(i), i);
    }
    return badChar;
}

int[] getGoodSuffix(String pattern) {
    int m = pattern.length();
    // 好后缀
    int[] goodSuffix = new int[m];
    goodSuffix[m - 1] = m - 1;
    int maxLen = 0;
    for (int i = m - 2; i >= 0; i--) {
        int j = 0;
        // 查找公共子串
        while (i + j + 1 < m && pattern.charAt(j) == pattern.charAt(i + j + 1)) {
            j++;
        }
        if (i + j + 1 < m) {
            // 不存在公共子串
            goodSuffix[i] = maxLen;
        } else {
            goodSuffix[i] = j;
            maxLen = j;
        }
    }
    return goodSuffix;
}

int bmSearch(String text, String pattern) {
    int n = text.length();
    int m = pattern.length();
    if (n == 0 || m == 0) {
        return -1;
    }
    HashMap<Character, Integer> badChar = getBadChar(pattern);
    int[] goodSuffix = getGoodSuffix(pattern);
    int i = m - 1;
    while (i < n) {
        // 匹配串从后往前匹配
        int j = m - 1;
        while (j >= 0) {
            char c = text.charAt(i + j - m + 1);
            if (c != pattern.charAt(j)) {
                int badCharDis = j - badChar.getOrDefault(c, -1);
                int goodSuffixDis = m - goodSuffix[j];
                i += Math.max(badCharDis, goodSuffixDis);
                break;
            }
            j--;
        }
        if (j == -1) {
            return i - m + 1;
        }
    }
    return -1;
}
```

#### 参考

1. [字符串匹配的Boyer-Moore算法 - 阮一峰的网络日志](https://www.ruanyifeng.com/blog/2013/05/boyer-moore_string_search_algorithm.html)
1. [字符串匹配算法详解 - 云+社区 - 腾讯云](https://cloud.tencent.com/developer/article/1770486)

### 3 Sunday 算法

#### 原理

- 主串`text`长度为`n`，匹配串`pattern`长度为`m`。

- 当`text[i + j] != pattern[j]`时，观察主串中匹配串的下一个字符`text[i + m]`：
    - 若`text[i + m]`在`pattern`中存在，则`i += m - c最后出现的位置`
    - 若`text[i + m]`在`pattern`中不存在，则`i += m + 1`

- 时间复杂度
    - 平均：`O(n)`
    - 最坏：`O(n * m)`

#### 代码

```java
int sundaySearch(char[] text, char[] pattern) {
    int n = text.length;
    int m = pattern.length;
    // 字符最后出现的位置
    HashMap<Character, Integer> pos = new HashMap<>();
    for (int i = 0; i < m; i++) {
        pos.put(pattern[i], i);
    }
    int i = 0;
    while (i + m <= n) {
        int j = 0;
        while (j < m) {
            if (text[i + j] != pattern[j]) {
                if (i + m < n && pos.containsKey(text[i + m])) {
                    i += m - pos.get(text[i + m]);
                } else {
                    i += m + 1;
                }
                break;
            }
            j++;
        }
        if (j == m) {
            return i;
        }
    }
    return -1;
}
```

#### 参考

1. [Sunday 解法 - 实现 strStr() - 力扣（LeetCode）](https://leetcode-cn.com/problems/implement-strstr/solution/python3-sundayjie-fa-9996-by-tes/)

### 4. Rabin Karp 算法

#### 原理

- 主串`text`长度为`n`，匹配串`pattern`长度为`m`。

- 使用字符串哈希算法将字符串比较转化为整数比较。然后通过滚动计算哈希来降低时间复杂度。最后防止出现哈希冲突，再朴素比较一遍。

- 区间`[a,b]`的哈希值为

$$hash1=text[a] \times k^{b-a} + \cdots + text[b] \times k^{0}$$

- 区间`[a+1,b+1]`的哈希值为

$$hash2=text[a+1] \times k^{b-a} + \cdots + text[b+1] \times k^{0}$$

$$hash2=(hash1-text[a] \times k^{b-a}) \times k + text[b + 1] \times k^{0}$$

- 如果字符串过长，最后计算哈希可能会溢出。为了解决这个问题，使用取余。

$$hash2=((hash1-text[a] \times k^{b-a} \mod q) \times k + text[b + 1] \times k^{0}) \mod q$$

- 最后，`k`取一个大于`text[i]`取值范围的质数即可。

- 时间复杂度：`O(n + m)`

#### 代码

```java
int rkSearch(char[] text, char[] pattern) {
    int n = text.length;
    int m = pattern.length;
    final int MOD = (int) 1e7 + 7; // 取余
    final int K = 31; // 任意数字即可，一般为质数
    final int POWER = (int) Math.pow(K, m - 1) % MOD;
    int pHash = 0;
    int tHash = 0;
    for (int i = 0; i < m; i++) {
        pHash = (pHash * K + pattern[i]) % MOD;
        tHash = (tHash * K + text[i]) % MOD;
    }
    for (int i = 0; i + m <= n; i++) {
        if (pHash == tHash) {
            boolean equal = true;
            // 二次判断，防止哈希冲突
            for (int j = 0; j < m; j++) {
                if (text[i + j] != pattern[j]) {
                    equal = false;
                    break;
                }
            }
            if (equal) {
                return i;
            }
        }
        if (i + m >= n) {
            break;
        }
        // 滚动计算哈希
        tHash = ((tHash - text[i] * POWER % MOD) * K + text[i + m]) % MOD;
        if (tHash < 0) {
            tHash += MOD;
        }
    }
    return -1;
}
```

#### 参考

1. [字符串匹配算法-Rabin Karp算法 | coolcao的小站](https://coolcao.com/2020/08/20/rabin-karp/)
1. [简单易懂的Rabin Karp算法详解！ - 实现 strStr() - 力扣（LeetCode）](https://leetcode-cn.com/problems/implement-strstr/solution/yi-dong-de-rabin-karpsuan-fa-hao-xiang-mei-ren-xie/)

