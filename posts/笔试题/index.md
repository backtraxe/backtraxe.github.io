# 笔试题


<!--more-->

## 字节跳动2022/9/18

### 1.金字塔

- **题目描述：**



$$
1 \le n \le 5 \times 10^5 \newline
1 \le \sum{m} \le 5 \times 10^5 \newline
0 \le p_i \le 10^9
$$

- **用例 1 输入：**

```text
3
2 100 280
2 190 360
2 150 360
```

- **用例 1 输出：**

```text
4
```

- **用例 2 输入：**

```text
5
5 0 1000 2000 3010 3200
4 40 1050 2049 3100
1 80
1 120
1 160
```

- **用例 2 输出：**

```text
10
```

- **代码：**

```java
package src.字节2022秋招0918.A;

import java.io.File;
import java.util.*;

// 100%
public class Main {
    public static void main(String[] args) throws Exception {
        // try (Scanner sc = new Scanner(System.in)) {
        try (Scanner sc = new Scanner(new File("src\\字节2022秋招0918\\A\\input"))) {
            // 1 <= n <= 5 * 10^5
            // 1 <= sum(m) <= 5 * 10^5
            // 0 <= pi <= 10^9
            int n = sc.nextInt();

            List<Integer> sub = new ArrayList<>(n); // 下一层
            int m = sc.nextInt();
            while (m-- > 0) {
                sub.add(sc.nextInt());
            }

            int ans = sub.size();

            for (int i = 1; i < n; i++) {
                List<Integer> next = new ArrayList<>();
                m = sc.nextInt();
                int len = sub.size();
                int j = 0; // 下一层指针
                while (m-- > 0) {
                    int cur = sc.nextInt();

                    while (j < len && sub.get(j) + 100 <= cur) j++;
                    if (j == len) continue;

                    if (j + 1 < len && sub.get(j + 1) - 100 < cur && cur < sub.get(j) + 100) {
                        // 两块砖支撑
                        next.add(cur);
                        ans++;
                    } else {
                        // 一块砖支撑
                        if (sub.get(j) - 50 < cur && cur < sub.get(j) + 50) {
                            next.add(cur);
                            ans++;
                        } else if (j + 1 < len && sub.get(j + 1) - 50 < cur && cur < sub.get(j + 1) + 50) {
                            next.add(cur);
                            ans++;
                        }
                    }
                }
                sub = next;
            }

            System.out.println(ans);
        }
    }
}
```

### 2.神奇序列

- **题目描述：**

给定一个仅由 0 和 0 构成的字符串，称长度大于 2，且 0 和 1 间隔排列的字符串为“神奇序列”，求子串中“神奇序列”的最长长度。

- **用例 1 输入：**

```text
0101011101
```

- **用例 1 输出：**

```text
6
```

- **代码：**

```java
package src.字节2022秋招0918.B;

import java.io.File;
import java.util.*;

// 100%
public class Main {
    public static void main(String[] args) throws Exception {
        // try (Scanner sc = new Scanner(System.in)) {
        try (Scanner sc = new Scanner(new File("src\\字节2022秋招0918\\B\\input"))) {
            // n >= 3 01相邻 神奇序列
            String s = sc.next();
            int n = s.length();

            // dp[i] 表示以 s[i] 结尾的神奇序列的长度
            int[] dp = new int[n];
            Arrays.fill(dp, 1);

            int ans = 0;

            for (int i = 1; i < n; i++) {
                int cur = s.charAt(i) - '0';
                int pre = s.charAt(i - 1) - '0';

                if (cur + pre == 1) {
                    dp[i] = dp[i - 1] + 1;
                }

                if (dp[i] >= 3) {
                    ans = Math.max(ans, dp[i]);
                }
            }

            System.out.println(ans);
        }
    }
}
```

### 3.ASDF

- **题目描述：**

给定一个长度为 4 的倍数且仅由 $A$、$S$、$D$ 和 $F$ 构成的字符串，我们可以将某个子串中的字符进行任意修改（只能出现四种字符），满足整个字符串中四种字符的数量相等，求子串的最短长度。

$$
0 \le n \le 10^6
$$

- **用例 1 输入：**

```text
ADDF
```

- **用例 1 输出：**

```text
1
```

- **用例 2 输入：**

```text
ASAFASAFADDD
```

- **用例 2 输出：**

```text
3
```

- **代码：**

```java
package src.字节2022秋招0918.C;

import java.io.File;
import java.util.*;

// 100%
public class Main {
    public static void main(String[] args) throws Exception {
        // try (Scanner sc = new Scanner(System.in)) {
        try (Scanner sc = new Scanner(new File("src\\字节2022秋招0918\\C\\input"))) {
            // n = 4k
            // n <= 10^6
            String s = sc.next();
            int n = s.length();

            // 出现次数
            int ca, cs, cd, cf;
            ca = cs = cd = cf = 0;

            for (int i = 0; i < n; i++) {
                char c = s.charAt(i);
                if (c == 'A') ca++;
                else if (c == 'S') cs++;
                else if (c == 'D') cd++;
                else if (c == 'F') cf++;
            }

            // 每个字符应该出现 n / 4 次，多余的次数
            int t = n / 4;
            int da = Math.max(ca - t, 0);
            int ds = Math.max(cs - t, 0);
            int dd = Math.max(cd - t, 0);
            int df = Math.max(cf - t, 0);

            boolean ba = da == 0;
            boolean bs = ds == 0;
            boolean bd = dd == 0;
            boolean bf = df == 0;

            int ans = n;
            // 滑动窗口求满足出现次数 da ds dd df 的最小窗口长度
            ca = cs = cd = cf = 0;
            int j = 0;
            for (int i = 0; i < n; i++) {
                char c = s.charAt(i);
                if (c == 'A') ca++;
                else if (c == 'S') cs++;
                else if (c == 'D') cd++;
                else if (c == 'F') cf++;

                while (j < i) {
                    c = s.charAt(j);
                    if (c == 'A') {
                        if (!ba && ca <= da) break;
                        ca--;
                    } else if (c == 'S') {
                        if (!bs && cs <= ds) break;
                        cs--;
                    } else if (c == 'D') {
                        if (!bd && cd <= dd) break;
                        cd--;
                    } else if (c == 'F') {
                        if (!bf && cf <= df) break;
                        cf--;
                    }
                    j++;
                }

                if (ca >= da && cs >= ds && cd >= dd && cf >= df) {
                    ans = Math.min(ans, i - j + 1);
                }
            }

            System.out.println(ans);
        }
    }
}
```

### 4.书柜

- **题目描述：**

$n$ 本书按顺序分成若干组，要求每组内书籍的高度差不超过 $k$，求组的最大容量、对应的组的数量、组中书的序号（从 1 开始）。

$$
0 \le n \le 10^6
$$

- **用例 1 输入：**

```text
3 3
14 12 10
```

- **用例 1 输出：**

```text
2 2
1 2
2 3
```

- **用例 2 输入：**

```text
2 0
10 10
```

- **用例 2 输出：**

```text
2 1
1 2
```

- **用例 3 输入：**

```text
4 5
8 19 10 13
```

- **用例 3 输出：**

```text
2 1
3 4
```

- **代码：**

```java

```

## 京东2022/9/17

### 1.12排列

- **题目描述：**

输入`a`代表 1 的数量，`b`代表 2 的数量，要求排列后相邻数字的乘积为偶数，输出任意排列，以空格隔开，无法形成则输出 -1。

$$
1 \le a+b \le 200000 , \quad 0 \le a,b \le 200000
$$

- **输入：**

```text
2 2
```

- **输出：**

```text
1 2 2 1
```

- **代码：**

```java
package src.京东2022秋招0917.A;

import java.io.File;
import java.util.*;

// 100%
public class Main {
    public static void main(String[] args) throws Exception {
        // try (Scanner sc = new Scanner(System.in)) {
        try (Scanner sc = new Scanner(new File("src\\京东2022秋招0917\\A\\input"))) {
            int a = sc.nextInt(); // 1
            int b = sc.nextInt(); // 2

            if (a > b + 1) {
                System.out.println(-1);
            } else {
                int c = Math.min(a, b);
                while (c-- > 0) {
                    System.out.printf("1 2 ");
                    a--;
                    b--;
                }
                while (a-- > 0) {
                    System.out.printf("1 ");
                }
                while (b-- > 0) {
                    System.out.printf("2 ");
                }
            }
        }
    }
}
```

### 2.前缀排列

- **题目描述：**

长度为 $n$ 的排列，每次可以交换相邻的两个数，经过若干次交换后，前 $k$ 个数变为长度为 $k$ 的排列，求最小交换次数。

$$
1 \le k \le n \le 200000 , \quad 1 \le a_i \le n
$$

- **输入：**

```text
5 3
2 4 1 3 5
```

- **输出：**

```text
2
```

- **代码：**

```java
package src.京东2022秋招0917.B;

import java.io.File;
import java.util.*;

// 100%
public class Main {
    public static void main(String[] args) throws Exception {
        // try (Scanner sc = new Scanner(System.in)) {
        try (Scanner sc = new Scanner(new File("src\\京东2022秋招0917\\B\\input"))) {
            // 1 <= k <= n <= 200000
            // 1 <= a[i] <= n
            int n = sc.nextInt();
            int k = sc.nextInt();
            int[] a = new int[n];

            int[] idx = new int[n + 1]; // 存放下标
            long ans = 0L;

            for (int i = 0; i < n; i++) {
                a[i] = sc.nextInt();
                idx[a[i]] = i;
                if (i < k && a[i] > k) ans -= i; // 需要被换出的元素
            }

            for (int i = 1; i <= k; i++) {
                if (idx[i] >= k) {
                    ans += idx[i]; // 需要被换入的元素
                }
            }

            System.out.println(ans);
        }
    }
}
```

### 3.漂亮串

- **题目描述：**

称仅由小写字母组成，$red$ 至少出现一次，并且不能出现 $der$ 的字符串为“漂亮串”。求长度为 $n$ 的漂亮串的数量，对 $1000000007$ 取模。

$$
1 \le n \le 10^5
$$

- **输入：**

```text
4
```

- **输出：**

```text
52
```

- **代码：**

```java

```

[京东9.17笔试第三题](https://www.nowcoder.com/discuss/1055351?type=all&order=recall&pos=&page=1&ncTraceId=&channel=-1&source_id=search_all_nctrack&gio_id=37D8B819210C8444A7193E07BC2D57DB-1663430466097)

## 蚂蚁2022/9/15

[蚂蚁笔试 研发岗 09.15 AC](https://www.nowcoder.com/discuss/1052870?type=2&channel=-1&source_id=discuss_terminal_discuss_hot_nctrack)

### A

### B

### 3.好串

- **题目描述：**

称仅由小写字母组成，只有一个字符出现奇数次，其他字符出现偶数次的字符串为“好串”。求字符串的子串是好串的数量。

$$
1 \le n \le 200000
$$

- **输入：**

```text
7
```

- **输出：**

```text
78
```

- **代码：**

```java
import java.util.*;

public class Main {

    public static int maxCount(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        int status = 0;
        Map<Integer, Integer> map = new HashMap<>();
        int ans = 0;
        map.put(0, 1);
        for (int i = 0; i < s.length(); i++) {
            status = status ^ (1 << (s.charAt(i) - 'a'));
            for (int j = 0; j < 26; j++) {
                int tmp = status ^ (1 << j);
                if (map.containsKey(tmp)) {
                    ans += map.get(tmp);
                }
            }
            map.put(status, map.getOrDefault(status, 0) + 1);
        }
        return ans;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String s = sc.next();
        System.out.println(maxCount(s));
    }
}
```

## 京东

### 3.漂亮串

- **题目描述：**

称仅由小写字母组成，$red$ 至少出现两次的字符串为“漂亮串”。求长度为 $n$ 的漂亮串的数量，对 $1000000007$ 取模。

$$
1 \le n \le 10^5
$$

- **输入：**

```text
7
```

- **输出：**

```text
78
```

- **代码：**

```java
import java.math.BigInteger;
import java.util.*;

public class Main {

    public static int MOD = 1000000007;

    public static long beautifulStr(int n) {
        long[] dp1 = new long[n + 1];
        long[] dp2 = new long[n + 1];
        if (n <= 5) {
            return 0;
        }
        dp1[3] = 1;
        BigInteger mod = new BigInteger(String.valueOf(MOD));
        for (int i = 4; i <= n; i++) {
            // 计算26的(i-3)次幂，然后对MOD取余
            BigInteger bigInteger = new BigInteger("26");
            BigInteger num = bigInteger.pow(i - 3);
            long result = num.mod(mod).longValue();
            // dp1转移方程
            dp1[i] = (result + 26 * dp1[i - 1] - dp1[i - 3] + MOD) % MOD;
        }
        for (int i = 4; i <= n; i++) {
            // dp2转移方程
            dp2[i] = (dp1[i - 3] + 26 * dp2[i - 1] - dp2[i - 3] + MOD) % MOD;
        }
        return dp2[n];
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        System.out.println(beautifulStr(n));
    }
}
```

## 京东

### 3.漂亮串

- **题目描述：**

称仅由小写字母组成，$red$ 至少出现两次的字符串为“漂亮串”。求长度为 $n$ 的漂亮串的数量，对 $1000000007$ 取模。

$$
1 \le n \le 10^5
$$

- **输入：**

```text
7
```

- **输出：**

```text
78
```

- **代码：**

```java
import java.math.BigInteger;
import java.util.*;

public class Main {

    public static int MOD = 1000000007;

    public static long beautifulStr(int n) {
        long[] dp1 = new long[n + 1];
        long[] dp2 = new long[n + 1];
        if (n <= 5) {
            return 0;
        }
        dp1[3] = 1;
        BigInteger mod = new BigInteger(String.valueOf(MOD));
        for (int i = 4; i <= n; i++) {
            // 计算26的(i-3)次幂，然后对MOD取余
            BigInteger bigInteger = new BigInteger("26");
            BigInteger num = bigInteger.pow(i - 3);
            long result = num.mod(mod).longValue();
            // dp1转移方程
            dp1[i] = (result + 26 * dp1[i - 1] - dp1[i - 3] + MOD) % MOD;
        }
        for (int i = 4; i <= n; i++) {
            // dp2转移方程
            dp2[i] = (dp1[i - 3] + 26 * dp2[i - 1] - dp2[i - 3] + MOD) % MOD;
        }
        return dp2[n];
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        System.out.println(beautifulStr(n));
    }
}
```

## 华为2022/9/14

### 1.

- **题目描述：**

给定一个长度为 4 的倍数且仅由 $A$、$S$、$D$ 和 $F$ 构成的字符串，我们可以将某个子串中的字符进行任意修改（只能出现四种字符），满足整个字符串中四种字符的数量相等，求子串的最短长度。

$$
0 \le n \le 10^6
$$

- **用例 1 输入：**

```text
ADDF
```

- **用例 1 输出：**

```text
1
```

- **用例 2 输入：**

```text
ASAFASAFADDD
```

- **用例 2 输出：**

```text
3
```

- **代码：**

```java

```

### 2.

- **题目描述：**

给定一个长度为 4 的倍数且仅由 $A$、$S$、$D$ 和 $F$ 构成的字符串，我们可以将某个子串中的字符进行任意修改（只能出现四种字符），满足整个字符串中四种字符的数量相等，求子串的最短长度。

$$
0 \le n \le 10^6
$$

- **用例 1 输入：**

```text
ADDF
```

- **用例 1 输出：**

```text
1
```

- **用例 2 输入：**

```text
ASAFASAFADDD
```

- **用例 2 输出：**

```text
3
```

- **代码：**

```java

```

### 3.停车场

- **题目描述：**

给定一个长度为 4 的倍数且仅由 $A$、$S$、$D$ 和 $F$ 构成的字符串，我们可以将某个子串中的字符进行任意修改（只能出现四种字符），满足整个字符串中四种字符的数量相等，求子串的最短长度。

$$
0 \le n \le 10^6
$$

- **用例 1 输入：**

```text
ADDF
```

- **用例 1 输出：**

```text
1
```

- **用例 2 输入：**

```text
ASAFASAFADDD
```

- **用例 2 输出：**

```text
3
```

- **代码：**

```java

```

## 华为2022/9/7

### 1.养猪场

- **题目描述：**

给定一个长度为 4 的倍数且仅由 $A$、$S$、$D$ 和 $F$ 构成的字符串，我们可以将某个子串中的字符进行任意修改（只能出现四种字符），满足整个字符串中四种字符的数量相等，求子串的最短长度。

$$
0 \le n \le 10^9
$$

- **用例 1 输入：**

```text
5
2
0 1 2
1 3 4
2 4
```

- **用例 1 输出：**

```text
3
```

- **代码：**

```java

```

### 2.士兵

- **题目描述：**

给定一个长度为 4 的倍数且仅由 $A$、$S$、$D$ 和 $F$ 构成的字符串，我们可以将某个子串中的字符进行任意修改（只能出现四种字符），满足整个字符串中四种字符的数量相等，求子串的最短长度。

$$
0 \le n \le 10^6
$$

- **用例 1 输入：**

```text
3 3
SXE
BXX
BBB
```

- **用例 1 输出：**

```text
8
```

- **用例 2 输入：**

```text
6 6
SBBBBB
BXXXXB
BBXBBB
XBBXXB
BXBBXB
BBXBEB
```

- **用例 2 输出：**

```text
13
```

- **代码：**

```java

```

### 3.停车场

- **题目描述：**

$$
0 \le n \le 10^6
$$

- **用例 1 输入：**

```text
5 6 2 1
2 1 2 2 1 2
0 0 0 0 0 0
2 1 2 1 2 1
1 1 2 1 2 2
0 0 0 0 0 0
```

- **用例 1 输出：**

```text
2 1 2 2 1 2
```

- **代码：**

```java

```

## 华为2022/8/31

### 1.

- **题目描述：**

给定一个长度为 4 的倍数且仅由 $A$、$S$、$D$ 和 $F$ 构成的字符串，我们可以将某个子串中的字符进行任意修改（只能出现四种字符），满足整个字符串中四种字符的数量相等，求子串的最短长度。

$$
0 \le n \le 10^6
$$

- **用例 1 输入：**

```text
ADDF
```

- **用例 1 输出：**

```text
1
```

- **用例 2 输入：**

```text
ASAFASAFADDD
```

- **用例 2 输出：**

```text
3
```

- **代码：**

```java

```

### 2.

- **题目描述：**

给定一个长度为 4 的倍数且仅由 $A$、$S$、$D$ 和 $F$ 构成的字符串，我们可以将某个子串中的字符进行任意修改（只能出现四种字符），满足整个字符串中四种字符的数量相等，求子串的最短长度。

$$
0 \le n \le 10^6
$$

- **用例 1 输入：**

```text
ADDF
```

- **用例 1 输出：**

```text
1
```

- **用例 2 输入：**

```text
ASAFASAFADDD
```

- **用例 2 输出：**

```text
3
```

- **代码：**

```java

```

### 3.停车场

- **题目描述：**

给定一个长度为 4 的倍数且仅由 $A$、$S$、$D$ 和 $F$ 构成的字符串，我们可以将某个子串中的字符进行任意修改（只能出现四种字符），满足整个字符串中四种字符的数量相等，求子串的最短长度。

$$
0 \le n \le 10^6
$$

- **用例 1 输入：**

```text
ADDF
```

- **用例 1 输出：**

```text
1
```

- **用例 2 输入：**

```text
ASAFASAFADDD
```

- **用例 2 输出：**

```text
3
```

- **代码：**

```java

```


