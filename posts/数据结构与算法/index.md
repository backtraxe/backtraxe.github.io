# 数据结构与算法


<!--more-->

## 1.基础

### 1.1 存储结构

- **数组**：
    - 数据连续存储
    - 支持随机访问，通过索引快速找到对应元素，时间复杂度 $O(1)$
    - 数组满时需要扩容，重新分配一块更大的空间，再把数据全部复制过去，时间复杂度 $O(n)$
    - 在数组中间进行插入和删除时需要对后面的所有数据进行移位，平均时间复杂度 $O(n)$
- **链表**：
    - 数据不连续存储，而是靠指针指向其他元素的位置
    - 不支持随机访问，平均时间复杂度 $O(n)$
    - 无需扩容
    - 插入和删除时无需移位，时间复杂度 $O(1)$（前驱结点已知），$O(n)$（前驱结点未知）
    - 指针域会占用储存空间，降低空间利用率

### 1.2 逻辑结构

- **栈（stack）**：后进先出（LIFO）。
- **队列（queue）**：先进先出（FIFO）。
- **树（tree）**：子结点也是一棵树，子结点之间相互独立。
- **图（graph）**：任意两个结点间可以连接。

每种逻辑结构均可使用不同存储结构实现，一般具体情况进行选择。

### 1.3 复杂度

- 常见时间复杂度：$O(1)$、$O(\log n)$、$O(n)$、$O(n\log n)$、$O(n^2)$、$O(n^3)$
- 常见空间复杂度：$O(1)$、$O(n)$、$O(n^2)$、$O(n^3)$

复杂度一般去掉常数项，并将系数置为1，$O$ 表示上界。

### 1.4 可视化

- [数据结构和算法动态可视化](https://visualgo.net/zh)
- [Data Structure Visualizations](https://www.cs.usfca.edu/~galles/visualization/Algorithms.html)

## 2.数组

```java
int[] nums = new int[n];
```

### 2.1 前缀和数组

`prefixSum[high + 1] - prefixSum[low]`可以 $O(1)$ 的时间复杂度求出`nums`中`[low,high]`的区间和。

```java
int[] getPrefixSum(int[] nums) {
    int n = nums.length;
    int[] prefixSum = new int[n + 1];
    for (int i = 0; i < n; i++) {
        prefixSum[i + 1] = prefixSum[i] + nums[i];
    }
    return prefixSum;
}
```

### 2.2 差分数组



```java
int[] getDiff(int[] nums) {
    int n = nums.length;
    for (int i = n; i > 0; i--) {
        nums[i] -= nums[i - 1];
    }
    return nums;
}
```

## 3.链表

### 定义

```java
class ListNode {
    int val;
    // 指向后继结点
    ListNode next;
    // 双向链表，指向前驱结点
    ListNode prev;
}

class LinkedList {
    // 头结点，不存放数据
    ListNode head = new ListNode();
    // 双向链表，尾结点，不存放数据
    ListNode tail = new ListNode();
}
```

### 常用方法

- 双指针（快慢指针）
- 虚拟头结点

### 实战

- [合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/)：双指针
- [合并k个升序链表](https://leetcode.cn/problems/merge-k-sorted-lists/)：K指针 + 优先队列
- [返回链表中倒数第k个节点](https://leetcode.cn/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

{{< admonition tip code false >}}
```java
public static ListNode findFromEnd1(ListNode head, int k) {
    // 要求 1 <= k <= n
    ListNode p1 = head, p2 = head;
    // p1 先走 k 步
    for (int i = 0; i < k && p1 != null; i++) {
        p1 = p1.next;
    }
    // 然后 p1 和 p2 同步走，走到头
    // 即 p2 走了 n - k 步，也就是倒数第 k 个
    while (p1 != null) {
        p1 = p1.next;
        p2 = p2.next;
    }
    return p2;
}
```

```java
// 不适用于多测试用例，因为 count 是静态的
// 或者每次调用前将 count 归零
public static count = 0;

public static ListNode findFromEnd2(ListNode head, int k) {
    // 要求 1 <= k <= n
    if (head == null) {
        return null;
    }
    ListNode node = findFromEnd2(head.next, k);
    // 从尾部向前计数
    count++;
    if (count == k) {
        return head;
    }
    return node;
}
```
{{< /admonition >}}

- [删除链表的倒数第n个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

1. 添加虚表头结点。
2. 返回链表的倒数第 k+1 个节点，删除后继结点。

- [找到链表的中间结点](https://leetcode.cn/problems/middle-of-the-linked-list/)：快慢指针，慢走一步，快走两步。
- [判断链表是否存在环](https://leetcode.cn/problems/linked-list-cycle/)

{{< admonition tip code false >}}
```java
boolean hasCycle(ListNode head) {
    // 快慢指针初始化指向 head
    ListNode slow = head, fast = head;
    // 快指针走到末尾时停止
    while (fast != null && fast.next != null) {
        // 慢指针走一步，快指针走两步
        slow = slow.next;
        fast = fast.next.next;
        // 快慢指针相遇，说明含有环
        if (slow == fast) {
            return true;
        }
    }
    // 不包含环
    return false;
}
```
{{< /admonition >}}

- [找出链表中环的起点](https://leetcode.cn/problems/linked-list-cycle-ii/)

{{< admonition tip code false >}}
```java
ListNode detectCycle(ListNode head) {
    ListNode fast = head, slow = head;
    while (fast != null && fast.next != null) {
        fast = fast.next.next;
        slow = slow.next;
        if (fast == slow) {
            break;
        }
    }
    if (fast == null || fast.next == null) {
        // 无环
        return null;
    }
    /*
       相遇时，slow 走了 k 步，fast 走了 2k 步
       则 k 为环的长度，设相遇点距环起点距离为 m
       则表头到环起点距离为 k - m，相遇点向前走到环起点距离为 k - m
       则将 slow 放到表头，然后 slow 和 fast 同步走
       相遇即为环起点
    */
    // 重新指向头结点
    slow = head;
    // 快慢指针同步前进，相交点就是环起点
    while (slow != fast) {
        fast = fast.next;
        slow = slow.next;
    }
    return slow;
}
```
{{< /admonition >}}

- [找出两个链表的交点](https://leetcode.cn/problems/intersection-of-two-linked-lists/)

{{< admonition tip code false >}}
1. 让 p1 遍历完链表 A 之后开始遍历链表 B，让 p2 遍历完链表 B 之后开始遍历链表 A，这样相当于「逻辑上」两条链表接在了一起。

```java
ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    // p1 指向 A 链表头结点，p2 指向 B 链表头结点
    ListNode p1 = headA, p2 = headB;
    while (p1 != p2) {
        // p1 走一步，如果走到 A 链表末尾，转到 B 链表
        if (p1 == null) {
            p1 = headB;
        } else {
            p1 = p1.next;
        }
        // p2 走一步，如果走到 B 链表末尾，转到 A 链表
        if (p2 == null) {
            p2 = headA;
        } else {
            p2 = p2.next;
        }
    }
    return p1;
}
```

2. 如果把两条链表首尾相连，那么「寻找两条链表的交点」的问题转换成了前面讲的「寻找环起点」的问题。

3. 预先计算两条链表的长度。

```java
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    int lenA = 0, lenB = 0;
    // 计算两条链表的长度
    for (ListNode p1 = headA; p1 != null; p1 = p1.next) {
        lenA++;
    }
    for (ListNode p2 = headB; p2 != null; p2 = p2.next) {
        lenB++;
    }
    // 让 p1 和 p2 到达尾部的距离相同
    ListNode p1 = headA, p2 = headB;
    if (lenA > lenB) {
        for (int i = 0; i < lenA - lenB; i++) {
            p1 = p1.next;
        }
    } else {
        for (int i = 0; i < lenB - lenA; i++) {
            p2 = p2.next;
        }
    }
    // 看两个指针是否会相同，p1 == p2 时有两种情况：
    // 1、要么是两条链表不相交，他俩同时走到尾部空指针
    // 2、要么是两条链表相交，他俩走到两条链表的相交点
    while (p1 != p2) {
        p1 = p1.next;
        p2 = p2.next;
    }
    return p1;
}
```

4. 哈希表存储结点。
{{< /admonition >}}

- [反转链表](https://leetcode.cn/problems/reverse-linked-list/)

{{< admonition tip code false >}}
```java
ListNode reverse(ListNode head) {
    if (head == null || head.next == null) {
        return head;
    }
    // 返回从 head.next 开始的链表的逆序的头结点
    // newHead 即为原链表尾结点
    ListNode newHead = reverse(head.next);
    // head.next 原来是待逆序链表的头结点
    // 逆序后变为尾结点
    // 将 head 作为新的尾结点，实现逆序
    head.next.next = head;
    head.next = null;
    // 返回逆序后链表的头结点
    return newHead;
}
```
{{< /admonition >}}

- [反转链表第m个节点到第n个节点](https://leetcode.cn/problems/reverse-linked-list-ii/)

{{< admonition tip code false >}}
```java
// 后继结点
ListNode successor = null;

ListNode reverseN(ListNode head, int n) {
    if (n == 1) {
        successor = head.next;
        return head;
    }
    ListNode newHead = reverseN(head.next, n - 1);
    head.next.next = head;
    // 将尾结点连接到后继结点上
    head.next = successor;
    return newHead;
}
```

```java
ListNode reverseBetween(ListNode head, int m, int n) {
    if (m == 1) {
        return reverseN(head, n);
    }
    head.next = reverseBetween(head.next, m - 1, n - 1);
    return head;
}
```
{{< /admonition >}}

- [K个一组翻转链表](https://leetcode.cn/problems/reverse-nodes-in-k-group/)

{{< admonition tip code false >}}
```java

```
{{< /admonition >}}

## 4.栈

### 单调栈

#### 返回第一个更大元素

```cpp
vector<int> nextGreaterElement(vector<int>& nums) {
    int n = nums.size();
    vector<int> ans(n);
    stack<int> st;
    for (int i = n - 1; i >= 0; i--) {
        while (!st.empty() && st.top() <= nums[i]) {
            // 栈顶元素比当前元素小，弹出
            st.pop();
        }
        // 栈顶元素即为下一个更大元素
        ans[i] = st.empty() ? -1 : st.top();
        // 当前元素入栈
        st.push(nums[i]);
    }
    return ans;
}
```

```java
int[] nextGreaterElement(int[] nums) {
    int n = nums.length;
    int[] ans = new int[n];
    Deque<Integer> stack = new LinkedList<>();
    for (int i = n - 1; i >= 0; i--) {
        while (!stack.isEmpty() && stack.peek() <= nums[i]) {
            stack.pop();
        }
        ans[i] = stack.isEmpty() ? -1 : stack.peek();
        stack.push(nums[i]);
    }
    return ans;
}
```

#### 循环数组

```java
int[] nextGreaterElement(int[] nums) {
    int n = nums.length;
    int[] ans = new int[n];
    Stack<Integer> st = new Stack<>();
    for (int i = 2 * n - 1; i >= 0; i--) {
        while (!st.isEmpty() && st.peek() <= nums[i % n]) {
            st.pop();
        }
        ans[i % n] = st.isEmpty() ? -1 : st.peek();
        st.push(nums[i % n]);
    }
    return ans;
}
```

## 5.队列

## 6.字符串

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

## 7.二叉树

### 基础

满二叉树：一个高度为 d 的二叉树，有 $2^d-1$ 个节点。即除叶节点外，每个节点都有两个孩子，即节点的出度只为 0 或 2。

完全二叉树：只有最后一层可能未满，且节点严格从左往右排列。即出度为 1 的节点一定只有左孩子；若某节点出度小于 2，则其右边的节点出度为 0。

> - 二叉树第 $i$ 层最多有 $2^{i-1}$ 个节点。
> - 高度为 $d$ 的二叉树最多有 $2^d-1$ 个节点。

### 定义

```python
class TreeNode(object):
	def __init__(self, value):
		self.lchild = None
        self.rchild = None
        self.value = 0
```

### 遍历

#### 前序遍历

递归法：

```cpp
void preorder(TreeNode* root) {
    if (!root) return;
    // 处理节点值 root->val
    preorder(root->left);
    preorder(root->right);
}
```

非递归法：

`压栈先右后左`

```cpp
void preorder(TreeNode* root) {
    if (!root) return;
    stack<TreeNode*> st;
    st.push(root);
    while (!st.empty()) {
        root = st.top();
        st.pop();
        // 处理节点值 root->val
        if (root->right) st.push(root->right);
        if (root->left) st.push(root->left);
    }
}
```

```cpp
void preorder(TreeNode* root) {
    stack<TreeNode*> st;
    whilt (root || !st.empty()) {
        if (root) {
            // 处理节点值 root->val
            st.push(root);
            root = root->left;
        } else {
            root = st.top()->right;
            st.pop();
        }
    }
}
```

#### 中序遍历

- 对于二叉搜索树，中序遍历可以得到一个递增的有序序列

递归法：

```cpp
void inorder(TreeNode* root) {
    if (!root) return;
    inorder(root->left);
    // 处理节点值 root->val
    inorder(root->right);
}
```

非递归法：

```cpp
void inorder(TreeNode* root) {
    stack<TreeNode*> st;
    while (root || !st.empty()) {
        if (root) {
            st.push(root);
            root = root->left;
        } else {
            // 处理节点值 st.top()->val
            root = st.top()->right;
            st.pop();
        }
    }
}
```

#### 后序遍历

- 后序遍历是删除节点时的顺序
- 可以配合栈来计算表达式树

递归法：

```cpp
void postorder(TreeNode* root) {
    if (!root) return;
    postorder(root->left);
    postorder(root->right);
    // 处理节点值 root->val
}
```

非递归法：

`前序遍历的非递归方法先左后右，最后逆序即可`

```cpp
void postorder(TreeNode* root) {
    if (!root) return;
    vector<int> res;  // 保存遍历结果
    stack<TreeNode*> st;
    st.push(root);
    while (!st.empty()) {
        root = st.top();
        st.pop();
        res.push_back(root->val);  // 保存节点值
        if (root->left) st.push(st->left);
        if (root->right) st.push(st->right);
    }
    reverse(res.begin(), res.end());  // 逆序
}
```

```cpp

```

#### 层序遍历

```cpp

```

### 二叉搜索树

Binary Search Tree，BST

#### 性质

- 左子树结点均小于根结点，右子树结点均大于根结点
- 左右子树均为二叉搜索树
- 中序遍历结果为升序

#### 插入

小左大右

{{< admonition tip "递归" false >}}
```java
TreeNode insert(TreeNode root, int val) {
    if (root == null) {
        return new TreeNode(val);
    } else if (root.val > val) {
        root.left = insert(root.left, val);
    } else if (root.val < val) {
        root.right = insert(root.right, val);
    }
    // 跳过相同值
    return root;
}
```
{{< /admonition >}}

{{< admonition tip "迭代" false >}}
```java
TreeNode insert(TreeNode root, int val) {
    if (root == null) {
        return new TreeNode(val);
    }
    TreeNode p = root;
    while (p != null) {
        if (p.val > val) {
            if (p.left == null) {
                p.left = new TreeNode(val);
                break;
            }
            p = p.left;
        } else if (p.val < val) {
            if (p.right == null) {
                p.right = new TreeNode(val);
                break;
            }
            p = p.right;
        } else {
            // 跳过相同值
            break;
        }
    }
    return root;
}
```
{{< /admonition >}}

#### 删除

- 若删除结点为叶结点，直接删除。
- 若删除结点为非叶结点，将左子树最大结点或者右子树最小结点移至当前位置。

{{< admonition tip "递归" false >}}
```java

```
{{< /admonition >}}

{{< admonition tip "迭代" false >}}
```java

```
{{< /admonition >}}

## 8.树

- 一棵 n 个节点的树，有 n-1 条边。
- 一棵 n 个节点的树，有 n 棵子树。
- 根节点：唯一，无入度的节点
- 节点的深度：节点距离根节点的距离。

```cpp
typedef struct treeNode {
    treeNode(int x): value(x) {}
    int value;
    vector<treeNode*> child;
} TreeNode;
```

```java
class TreeNode {
    public int val;
    public TreeNode[] children;
    TreeNode() {
        this.val = 0;
    }
}
```

## 9.图

### 术语表

- 图（graph）
- 边（edge）：连接两个顶点。
    - 自环：起点和终点是同一个顶点的边。
    - 平行边：连接同一对顶点的两条边。
- 顶点（vertex）
- 度（degree）：顶点连接的边的数量。
    - 入度（in degree）：有向图专有，终点是当前顶点的边的数量。
    - 出度（out degree）：有向图专有，起点是当前顶点的边的数量。
- 路径（path）：由边连接的一系列顶点。
    - 简单路径：无重复顶点的路径。
- 环：起点和终点相同的路径。
    - 简单环：无重复顶点的环。

是否存在平行边：

- 简单图：无平行边的图。
- 多重图：存在平行边的图。

边是否存在方向：

- 有向图：边有方向。
- 无向图：边无方向。


- 无环图：不存在环的图。
- 子图：边和连接的顶点的子集。
- 连通图：任意一个顶点都存在路径到达另一个任意顶点。
    - 连通子图：
    - 极大连通子图：
- 二分图：能将所有顶点分为两个部分的图，每条边的两个顶点分别属于不同的部分。

### 定义

```java
// 结点
class Vertex {
    int id;
    List<Vertex> neighbors;
}
// 邻接表 无边权
List<Integer>[] adjList1;
// 邻接表 有边权
List<int[]>[] adjList2;
// 邻接表，查询效率更高
TreeSet<Integer>[] adjSet;
// 邻接矩阵
int[][] adjMat;
```

- 邻接表
    - 优点：占用的空间少
    - 缺点：无法快速判断两个节点是否相邻
    - 适用于稀疏图
- 邻接矩阵
    - 优点：占用的空间多
    - 缺点：可以快速判断两个节点是否相邻
    - 适用于稠密图

### 遍历

#### DFS

```java
boolean[] vis;
Deque<Integer> trace;                            // 记录当前路径
boolean[] onPath;                                // 判断某顶点是否在当前路径上
List<List<Integer>> traces = new LinkedList<>(); // 记录所有路径

// 邻接表
void dfs(List<Integer>[] graph, int begin, int end) {
    if (vis[begin]) {
        return;
    }
    // 访问当前顶点
    vis[begin] = true;
    onPath[begin] = true;
    trace.offerLast(begin);
    // 到达终点
    if (begin == end) {
        // 防止已添加路径被修改
        traces.add(new LinkedList<Integer>(trace));
        // 回溯
        vis[begin] = false;
        onPath[begin] = false;
        trace.pollLast();
        return;
    }
    // 下个顶点
    for (int next : graph[begin]) {
        if (!vis[next]) {
            dfs(graph, next, end);
        }
    }
    // 回溯
    vis[begin] = false;
    onPath[begin] = false;
    trace.pollLast();
}
```

```java
boolean[] vis;
Deque<Integer> trace;                            // 记录当前路径
boolean[] onPath;                                // 判断某顶点是否在当前路径上
List<List<Integer>> traces = new LinkedList<>(); // 记录所有路径

// 邻接矩阵
void dfs(int[][] graph, int begin) {
    if (vis[begin]) {
        return;
    }
    // 访问当前顶点
    vis[begin] = true;
    onPath[begin] = true;
    trace.offerLast(begin);
    // 到达终点
    if (begin == end) {
        // 防止已添加路径被修改
        traces.add(new LinkedList<Integer>(trace));
        // 回溯
        vis[begin] = false;
        onPath[begin] = false;
        trace.pollLast();
        return;
    }
    // 下个顶点
    for (int next = 0; next < graph.length; next++) {
        if (!vis[next] && graph[begin][next] != 0) {
            dfs(graph, next);
        }
    }
    // 回溯
    vis[begin] = false;
    onPath[begin] = false;
    trace.pollLast();
}
```

#### BFS

```python
def BFS(开始节点 s):
    q = 队列
    vis = 集合/哈希表/布尔数组
    将s添加到q中
    将s添加到vis中/标记s为已访问
    步数 = 0
    while(q非空):
        for q的每一个节点，记为h # 每次清空队列
            if 满足结束条件:
                return
            for h的每个相邻节点x:
                if x未在vis中/x未访问:
                    将x添加到q中
                    将x添加到vis中/标记x为已访问
        步数 += 1
```

```python
# 双向BFS
def BiBFS(开始节点 s，结束节点 e):
    s1 = 从s开始的搜索集合
    s2 = 从e开始的搜索集合
    vis = 集合/哈希表/布尔数组
    将s添加到s1中
    将s添加到vis中/标记s为已访问
    将e添加到s2中
    将e添加到vis中/标记e为已访问
    步数 = 0
    while(s1非空 and s2非空):
        if s1中元素比s2多:
            # 交换s1和s2
            s1, s2 = s2, s1
        s3 = 过渡集合
        for s1的每一个节点，记为h # 每次清空队列
            if h在s2中存在:
                return
            for h的每个相邻节点x:
                if x未在vis中/x未访问:
                    将x添加到s3中
                    将x添加到vis中/标记x为已访问
        # 交换q1和q2（一边一轮）
        s1 = s2
        s2 = s3
        步数 += 1
```

### Flood Fill

#### 岛屿数量

```java
int[] dx = {0, 1, 0, -1};
int[] dy = {1, 0, -1, 0};
int m, n;

public int numIslands(char[][] grid) {
    m = grid.length;
    n = grid[0].length;
    boolean[][] visited = new boolean[m][n];
    int res = 0;
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (grid[i][j] == '1' && !visited[i][j]) {
                res++;
                dfs(grid, i, j, visited);
            }
        }
    }
    return res;
}

void dfs(char[][] grid, int x, int y, boolean[][] visited) {
    if (x < 0 || x >= m || y < 0 || y >= n || visited[x][y] || grid[x][y] == '0') {
        return;
    }
    visited[x][y] = true;
    for (int i = 0; i < 4; i++) {
        int nextX = x + dx[i];
        int nextY = y + dy[i];
        dfs(grid, nextX, nextY, visited);
    }
}
```

#### 封闭岛屿的数量

将靠边的岛屿变为水，剩下的就是「封闭岛屿」。

```java
void dfs(int[][] grid, int x, int y) {
    if (x < 0 || x >= m || y < 0 || y >= n || grid[x][y] == 0) {
        return;
    }
    grid[x][y] = 0; // 淹没
    for (int i = 0; i < 4; i++) {
        int nextX = x + dx[i];
        int nextY = y + dy[i];
        dfs(grid, nextX, nextY);
    }
}
```

#### 1020. 飞地的数量

先把靠边的陆地淹掉，然后去数剩下的陆地数量。

#### 695. 岛屿的最大面积

淹没岛屿的同时，记录这个岛屿的面积。

#### 1905. 统计子岛屿

岛屿 B 中存在一片陆地，在岛屿 A 的对应位置是海水，那么岛屿 B 就不是岛屿 A 的子岛。

#### 694. 不同岛屿的数量

对于形状相同的岛屿，如果从同一起点出发，dfs 函数遍历的顺序肯定是一样的。

分别用 1, 2, 3, 4 代表上下左右，用 -1, -2, -3, -4 代表上下左右的撤销。

把二维矩阵中的「岛屿」进行转化，变成比如字符串这样的类型，然后利用 HashSet 这样的数据结构去重，最终得到不同的岛屿的个数。

## 10.排序

### 10.1 选择排序

```cpp
void selectionSort(vector<int>& nums) {
    int n = nums.size();
    for (int i = 0; i < n; i++) {
        int minIdx = i;
        for (int j = i + 1; j < n; j++) {
            minIdx = (arr[j] < arr[minIdx]) ? j : minIdx;
        }
        int temp = arr[i];
        arr[i] = arr[minIdx];
        arr[minIdx] = temp;
    }
}
```

```java
void selectionSort(int[] nums) {
    int len = nums.length;
    // 循环不变量：[0, i) 有序，且该区间里所有元素就是最终排定的样子
    for (int i = 0; i < len - 1; i++) {
        // 选择区间 [i, len - 1] 里最小的元素的索引，交换到下标 i
        int minIdx = i;
        for (int j = i + 1; j < len; j++) {
            if (nums[j] < nums[minIdx]) {
                minIdx = j;
            }
        }
        swap(nums, i, minIdx);
    }
    return nums;
}

void swap(int[] nums, int a, int b) {
    int temp = nums[a];
    nums[a] = nums[b];
    nums[b] = temp;
}
```

- 时间复杂度：$ O(n^2) $
- 空间复杂度：$ O(1) $

特点：

- 不稳定
- 每一轮有一个元素（当前最小元素）归位

### 10.2 冒泡排序

```cpp
void bubbleSort(vector<int>& arr) {
    for (int step = 1; step < arr.size(); step++) {
        for (int i = 0; i < arr.size() - step; i++) {
            if (arr[i] > arr[i + 1]) {
                int temp = arr[i];
                arr[i] = arr[i + 1];
                arr[i + 1] = temp;
            }
        }
    }
}
```

- 时间复杂度：$ O(n^2) $
- 空间复杂度：$ O(1) $

特点：

- 稳定
- 每一轮有一个元素（当前最大元素）归位

### 10.3 插入排序

```cpp
void insertionSort(vector<int>& arr) {
    for (int i = 1; i < arr.size(); i++) {
        int temp = arr[i], j;
        for (j = i - 1; j >= 0 && arr[j] > arr[i]; j--) {
            arr[j + 1] = arr[j];
        }
        arr[j + 1] = temp;
    }
}
```

- 时间复杂度：$ O(n^2) $
- 空间复杂度：$ O(1) $

特点：

- 稳定

### 10.4 归并排序

```cpp
void mergeSort() {

}

void merge() {

}
```

### 10.5 快速排序

**基本思想：**

- 从数组中取出一个数，称之为基数（pivot）。
- 遍历数组，将比基数大的数字放到它的右边，比基数小的数字放到它的左边。遍历完成后，数组被分成了左右两个区域。
- 将左右两个区域视为两个数组，重复前两个步骤，直到排序完成。

```cpp
// 把数组分为两半，返回分割中点
int partition(vector<int> &arr, int low, int high) {
    // [low, high]
    int pivotId = low + rand() % (high - low + 1);
    swap(arr[low], arr[pivotId]);
    int pivot = arr[low];
    while (low < right) {
        while (low < high && arr[high] > pivot) high--;
        arr[low] = arr[high];
        while (low < high && arr[low] <= pivot) low++;
        arr[high] = arr[low];
    }
    arr[low] = pivot;
    return low;
}

void quickSort(vector<int> &arr, int low, int high) {
    if (low >= high) return;
    int mid = partition(arr, low, high);
    quickSort(arr, low, mid - 1);
    quickSort(arr, mid + 1, high);
}
```

{{< admonition tip "Java" false >}}
```java
void quickSort(int[] arr) {
    quickSort(arr, 0, arr.length - 1);
}

void quickSort(int[] arr, int low, int high) {
    if (low >= high) return;
    int mid = partition(arr, low, high);
    quickSort(arr, low, mid - 1);
    quickSort(arr, mid + 1, high);
}

int partition(int[] arr, int low, int high) {
    int pivotID = (int) (Math.random() * (high - low + 1)) + low;
    swap(arr, low, pivotID);
    int pivot = arr[low];
    while (low < high) {
        while (low < high && arr[high] > pivot) high--;
        swap(arr, low, high);
        while (low < high && arr[low] <= pivot) low++;
        swap(arr, low, high);
    }
    arr[low] = pivot;
    return low;
}

void swap(int[] arr, int i, int j) {
    int temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
}
```
{{< /admonition >}}

### 总结

| 排序算法 | 时间复杂度 | 稳定性 |
|:---:|:---:|:---:|
| 冒泡排序 | $O\(n^2\)$ | 稳定 |
| 选择排序 | $O\(n^2\)$ | 不稳定 |
| 插入排序 | $O\(n^2\)$ | 稳定 |
| 快速排序 | $O\(n \\log n\)$ | 不稳定 |
| 归并排序 | $O\(n \\log n\)$ | 稳定 |
| 堆排序 | $O\(n \\log n\)$ | 不稳定 |
| 计数排序 | $O\(n\)$ | 稳定 |
| 基数排序 | $O\(n\)$ | 稳定 |

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=AM_HTMLorMML-full"></script>
<table style="text-align:center">
    <tr>
    	<th rowspan='2'>名称</th>
    	<th rowspan='2'>数据对象</th>
    	<th rowspan='2'>稳定性</th>
    	<th colspan='2'>时间复杂度</th>
    	<th rowspan='2'>额外空间复杂度</th>
    	<th rowspan='2'>描述</th>
    </tr>
    <tr>
    	<th>平均</th>
    	<th>最坏</th>
    </tr>
    <tr>
    	<td>冒泡排序</td>
    	<td>数组</td>
    	<td>是</td>
    	<td colspan='2'>$ O(n^2) $</td>
    	<td>$ O(1) $</td>
    	<td>(无序区，有序区)<br/>从无序区通过交换找出最大元素放到有序区前端。</td>
    </tr>
    <tr>
    	<td rowspan='2'>选择排序</td>
    	<td>数组</td>
    	<td>否</td>
    	<td rowspan='2' colspan='2'>$ O(n^2) $</td>
    	<td rowspan='2'>$ O(1) $</td>
    	<td rowspan='2'>(有序区，无序区)<br/>在无序区里找一个最小的元素放到有序区后端。<br/>对数组：比较多，交换少</td>
    </tr>
    <tr>
    	<td>链表</td>
    	<td>是</td>
    </tr>
    <tr>
    	<td>插入排序</td>
    	<td>数组、链表</td>
        <td>是</td>
    	<td colspan='2'>$ O(n^2) $</td>
    	<td>$ O(1) $</td>
    	<td>(有序区，无序区)<br/>把无序区的第一个元素插入到有序区的合适位置。<br/>对数组：比较少，交换多</td>
    </tr>
    <tr>
    	<td>堆排序</td>
    	<td>数组</td>
        <td>否</td>
    	<td colspan='2'>$ O(n \log{n}) $</td>
    	<td>$ O(1) $</td>
    	<td>(最大堆，有序区)<br/>从堆顶把最大值弹出到有序区前端，然后调整堆。</td>
    </tr>
    <tr>
    	<td rowspan='3'>归并排序</td>
    	<td rowspan='2'>数组</td>
        <td rowspan='3'>是</td>
        <td colspan='2'>$ O(n \log{\log{n}}) $</td>
        <td>$ O(1) $</td>
        <td rowspan='3'>将数据分为两段，再从两段中逐个选最小的元素移入新数据段的末尾。<br/>可自上而下，也可自下而上</td>
    </tr>
    <tr>
        <td rowspan='2' colspan='2'>$ O(n \log{n}) $</td>
    	<td>自上而下：$ O(n)+O(\log{n}) $</td>
    </tr>
    <tr>
        <td>链表</td>
    	<td>自下而上：$ O(1) $</td>
    </tr>
    <tr>
    	<td>快速排序</td>
    	<td>数组</td>
        <td>否</td>
    	<td>$ O(n \log{n}) $</td>
        <td>$ O(n^2) $</td>
    	<td>$ O(\log{n}) $</td>
    	<td>(小数区，基准元素，大数区)<br/>在区间中随机挑选一个元素作为基准元素，将小于该基准的元素放到基准之前，大于的放到基准之后，然后递归地对小数区和大数区进行快速排序。</td>
    </tr>
    <tr>
    	<td>希尔排序</td>
    	<td>数组</td>
        <td>否</td>
    	<td>$ O(n \log{\log{n}}) $</td>
    	<td>$ O(n^2) $</td>
        <td>$ O(1) $</td>
    	<td>按从大到小的间距进行多次插入排序，最后一次的间距为1。</td>
    </tr>
    <tr>
    	<td>计数排序</td>
    	<td>数组、链表</td>
        <td>是</td>
    	<td colspan='2'>$ O(n+m) $</td>
    	<td>$ O(n+m) $</td>
    	<td>统计小于等于该元素值的元素的个数i，然后将该元素放在目标数组的第i个位置。</td>
    </tr>
    <tr>
    	<td>桶排序</td>
    	<td>数组、链表</td>
        <td>是</td>
    	<td colspan='2'>$ O(n) $</td>
    	<td>$ O(m) $</td>
    	<td>将值为i的元素放入第i号桶，然后依次把桶里的元素倒出来。</td>
    </tr>
    <tr>
    	<td>基数排序</td>
    	<td>数组、链表</td>
        <td>是</td>
    	<td>$ O(k \times n) $</td>
    	<td>$ O(n^2) $</td>
        <td></td>
    	<td>一种多关键字的排序算法，可用桶排序实现。</td>
    </tr>
</table>

参考文章

1. [当我谈排序时，我在谈些什么🤔](https://leetcode-cn.com/problems/sort-an-array/solution/dang-wo-tan-pai-xu-shi-wo-zai-tan-xie-shi-yao-by-s/)
1. [复习基础排序算法（Java） - 排序数组 - 力扣（LeetCode）](https://leetcode-cn.com/problems/sort-an-array/solution/fu-xi-ji-chu-pai-xu-suan-fa-java-by-liweiwei1419/)

## 11.查找

### 11.1 顺序查找

### 11.2 二分查找

#### 闭区间的二分查找

```java
int binarySearch(int[] nums, int target) {
    // [left, right]
    int left = 0;
    int right = nums.length - 1;

    while(left <= right) {
        // 防止整数溢出
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            return mid;
        } else if (nums[mid] < target) {
            // 右侧
            left = mid + 1;
        } else {
            // 左侧
            right = mid - 1;
        }
    }
    return -1;
}
```

#### 左闭右开区间的二分查找

```java
int binarySearch(int[] nums, int target) {
    // [left, right)
    int left = 0;
    int right = nums.length;

    while(left < right) {
        // 防止整数溢出
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            return mid;
        } else if (nums[mid] < target) {
            // 右侧
            left = mid + 1;
        } else {
            // 左侧
            right = mid;
        }
    }
    return -1;
}
```

#### 左侧边界的二分查找 lower_bound

```java
// 左侧边界
// 寻找第一个大于等于 target 的元素位置
int binarySearch(int[] nums, int target) {
    // [left, right)
    int left = 0;
    int right = nums.length;

    while(left < right) {
        // 防止整数溢出
        int mid = left + (right - left) / 2;
        if (nums[mid] >= target) {
            // 左侧、中间
            right = mid;
        } else {
            // 右侧
            left = mid + 1;
        }
    }
    return left;
    // left 范围为 [0, nums.length]
    // 当 left == nums.length
    // 或者 nums[left] != target
    // 说明 nums 中无 target
}
```

#### 右侧边界的二分查找 upper_bound

```java
// 右侧边界
// 寻找第一个大于 target 的元素位置
int binarySearch(int[] nums, int target) {
    // [left, right)
    int left = 0;
    int right = nums.length;

    while(left < right) {
        // 防止整数溢出
        int mid = left + (right - left) / 2;
        if (nums[mid] > target) {
            // 左侧
            right = mid;
        } else {
            // 中间、右侧
            left = mid + 1;
        }
    }
    return left;
    // left 范围为 [0, nums.length]
    // 当 left == 0
    // 或者 nums[left - 1] != target
    // 说明 nums 中无 target
}
```

### 11.3 分块查找

### 11.4 哈希表

### 11.5 B树

又称**多路平衡查找树**，规定B树的阶 $m$ 为每个结点的最大孩子数量。

- 每个结点最多 $m$ 个孩子。
- 若根结点不是叶结点，则至少有两个孩子。
- 除根结点外，每个非叶结点至少有 $\lceil \frac{m}{2} \rceil$ 个孩子。
- 结点中关键字从小到大排序，关键字两侧为指向孩子的指针。
- 关键字左侧指针指向的子树中的关键字均小于该关键字，右侧则大于。
- 所有叶结点在同一层，不存储信息（空结点）。

**高度**

对任意一棵包含 $n$ 个关键字、高度为 $h$、阶数为 $m$ 的B树（不考虑空的叶结点）：

- 下界：每层关键字越多，则高度越矮。B树中每个结点最多有 $m$ 棵子树，$m-1$ 个关键字，所以

{{< math >}}
$$
\begin{align}
n&\le(m-1)(1+m+m^2+\cdots+m^{h-1})=m^h-1 \\
h&\ge\log_m(n+1)
\end{align}
$$
{{< /math >}}

- 上界：每层关键字越少，则高度越高。B树中根结点最少1个关键字，非叶结点最少 $\lceil \frac{m}{2} \rceil$ 棵子树， $\lceil \frac{m}{2} \rceil-1$ 个关键字，所以

{{< math >}}
$$
\begin{align}
n&\ge1+2\times(\lceil \frac{m}{2} \rceil-1)(1+\lceil \frac{m}{2} \rceil+\lceil \frac{m}{2} \rceil^2+\cdots+\lceil \frac{m}{2} \rceil^{h-2})=2\times\lceil \frac{m}{2} \rceil^{h-1}-1 \\
h&\le\log_{\lceil \frac{m}{2} \rceil}(\frac{n+1}{2})+1
\end{align}
$$
{{< /math >}}

**查找**

1. 查找结点。将结点信息读入内存。
2. 结点中查找关键字。在结点关键字表中二分查找，未找到则查找对应子树。
3. 若子树为空，则查找失败。

**插入**

1. 定位。利用查找算法，找出插入该关键字的最底层中的某个非叶结点。
2. 插入。除了根结点，每个结点的关键字个数都在区间 $[\lceil \frac{m}{2} \rceil-1,m-1]$ 内。若插入后的结点关键字个数在范围内，则可以直接插入，否则必须对结点进行分裂。
3. 分裂。将待分裂结点从中间位置（$\lceil \frac{m}{2} \rceil$）将其中的关键宇分为两部分，左部分包含的关键字放在原结点中，右部分包含的关键字放到新结点中，中间位置（$\lceil \frac{m}{2} \rceil$）的结点插入原结点的父结点。若此时导致其父结点的关键字个数也超过了上限，则继续进行这种分裂操作，直至这个过程传到根结点为止，进而导致B树高度增1。

**删除**

1. 定位。
2. 删除。
3. 借位。

### 11.6 B+树

MySQL 的索引采用这种数据结构。一棵 $m$ 阶的B+树需满足下列条件：

1. 每个分支结点最多有 $m$ 棵子树。
2. 非叶根结点至少有两棵子树，其他每个分支结点至少有 $\lceil \frac{m}{2} \rceil$ 棵子树。
3. 每个结点的子树个数与关键字个数相等。
4. 所有叶结点包含**全部关键字**及**指向相应记录的指针**，叶结点中将关键字从小到大排序，并且相邻叶结点从小到大相互链接起来。
5. 所有分支结点（可视为索引的索引）中仅包含它的各个子结点（即下一级的索引块）中关键字的**最大值**及指向其子结点的指针。

**查找**

B+树的查找、插入和删除操作和B树的基本类似。只是在查找过程中，非叶结点上的关键字值等于给定值时并不终止，而是继续向下查找，直到叶结点上的该关键字为止。所以，在B+树中查找时，无论查找成功与否，每次查找都是一条从根结点到叶结点的路径。

**B+树与B树的差异**

1. 在B+树中，具有n个关键字的结点含有n棵子树；而在B树中，具有n个关键字的结点含有n+1棵子树。
2. 在B+树中，每个结点的关键字个数n的范围是 $\lceil \frac{m}{2} \rceil \le n \le m$（根结点：$1 \le n \le m$）；在B树中，每个结点的关键字个数n的范围是 $\lceil \frac{m}{2} \rceil-1 \le n \le m-1$（根结点：$1 \le n \le m-1$)。
3. 在B+树中，叶结点包含信息，所有非叶结点仅起索引作用，非叶结点中的每个索引项只含有对应子树的最大关键字和指向该子树的指针，不含有该关键字对应记录的存储地址。
4. 在B+树中，叶结点包含了全部关键字，即在非叶结点中出现的关键字也会出现在叶结点中；而在B树中，叶结点（最外层内部结点）包含的关键字和其他结点包含的关键字是不重复的。

### 11.7 跳表

## 12.高级数据结构

### 并查集（Union Find）

### 前缀树（Trie 树）

### 树状数组

树状数组是一种可以动态维护序列**前缀和**的数据结构（下标从 1 开始），它的功能是：

- 单点修改`add(index, val)`：把序列第`index`个元素增加`val`
- 区间查询`preSum(index)`：查询前`index`个元素的前缀和

#### 查询前缀和

```java
class TreeArray {
    private int[] tree; // sum(nums[i])
    private int n;

    public TreeArray(int[] nums) {
        n = nums.length + 1;
        tree = new int[n];
        for (int i = 0; i < n - 1; i++) {
            add(i, nums[i]);
        }
    }

    public void add(int index, int val) {
        // 下标从 1 开始
        index++;
        // 单点修改，增加数组 index 元素的值
        while (index < n) {
            tree[index] += val;
            // 更新父结点
            index += lowBit(index);
        }
    }

    public int preSum(int index) {
        // 查询前缀和
        int sum = 0;
        while (index > 0) {
            sum += tree[index];
            // 查询子结点
            index -= lowBit(index);
        }
        return sum;
    }

    private static int lowBit(int x) {
        // 返回 x 二进制最低位 1 的值
        // eg. 6(0b110) 返回 2(0b010)
        return x & (-x);
    }
}
```

#### 复杂度分析

- 时间复杂度：
    - 构造函数：$O(n \log n)$
    - `add`函数：$O(\log n)$
    - `preSum`函数：$O(\log n)$
- 空间复杂度：$O(n)$

### 线段树

线段树是常用的用来维护**区间信息**的数据结构。

线段树可以在 $O(\log n)$ 的时间复杂度内实现单点修改、区间修改、区间查询（区间求和，求区间最大值，求区间最小值）等操作。

```java
class SegmentTree {
    private int[] tree; // 维护区间
    private int[] lazy; // 惰性标记，维护修改值

    public SegmentTree(int[] nums) {
        int n = nums.length;
        // 线断树有 n 个叶结点，总结点个数设为 x
        // 非叶结点都有 2 棵子树，则
        // x - 1 = (x - n) * 2
        // x = 2 * n - 1
        // 根结点索引从 1 开始
        tree = new int[n * 2];
        lazy = new int[n * 2];
        build(nums, 0, nums.length - 1, 1);
    }

    void build(int[] nums, int left, int right, int root) {
        // 对 [left, right] 区间建立线段树,当前根的编号为 root
        if (left == right) {
            // 叶结点
            tree[root] = nums[left];
            return;
        }
        // + 优先级高于 >>
        int mid = left + ((right - left) >> 1);
        build(nums, left, mid, root << 1);          // root * 2
        // << 优先级高于 |
        build(nums, mid + 1, right, root << 1 | 1); // root * 2 + 1
        // 子树信息更新父结点信息
        tree[root] = tree[root << 1] + tree[root << 1 | 1];
    }

    int rangeSum(int qLeft, int qRight, int left, int right, int root) {
        // 查询区间 [qLeft, qRight] 的元素总和
        if (qLeft <= left && right <= qRight) {
            // 当前区间是查询区间的子区间
            return tree[root];
        }
        pushDown(left, right, root);
        int mid = left + ((right - left) >> 1);
        int sum = 0;
        if (qLeft <= mid) {
            sum += rangeSum(qLeft, qRight, left, mid, root << 1);
        }
        if (mid < qRight) {
            sum += rangeSum(qLeft, qRight, mid + 1, right, root << 1 | 1);
        }
        return sum;
    }

    void update(int qLeft, int qRight, int val, int left, int right, int root) {
        // 将区间 [qLeft, qRight] 内的元素加 val
        if (qLeft <= left && right <= qRight) {
            tree[root] += val * (right - left + 1);
            // 不必此时修改到叶结点，将修改值记录到父结点的 lazy 标记
            // 下次查询到需要修改的叶结点时再修改
            lazy[root] += val;
            return;
        }
        pushDown(left, right, root);
        int mid = left + ((right - left) >> 1);
        if (qLeft <= mid) {
            update(qLeft, qRight, val, left, mid, root << 1);
        }
        if (mid < qRight) {
            update(qLeft, qRight, val, mid + 1, right, root << 1 | 1);
        }
        tree[root] = tree[root << 1] + tree[root << 1 | 1];
    }

    private void pushDown(int left, int right, int root) {
        // lazy 标记处理
        int mid = left + ((right - left) >> 1);
        if (lazy[root] > 0 && left < right) {
            // 如果当前节点的 lazy 标记非空，则更新当前节点两个子节点的值和 lazy 标记
            tree[root << 1] += lazy[root] * (mid - left + 1);
            tree[root << 1 | 1] += lazy[root] * (right - mid);
            lazy[root << 1] += lazy[root];
            lazy[root << 1 | 1] += lazy[root];
            lazy[root] = 0;
        }
    }
}
```

### 平衡二叉查找树（AVL树）

Balanced Binary Search Tree，BBST

通过旋转来保持树的平衡。

```java
class TreeNode {
    int val;
    TreeNode left, right;
}
```

### 红黑树

Red Black Tree

#### 性质

1. 每个结点可红可黑。
2. 根结点是黑色。
3. 叶结点（null）是黑色。
4. 红色结点的孩子是黑色。
5. 任意结点到叶结点的路径都包含相同数量的黑色结点。

{{< image src="/红黑树/tree.svg" width="100%" >}}

6. 红黑树不是完美平衡二叉查找树，左右子树高度差可能大于1。

#### 自平衡

- **左旋**：右孩子V变为当前结点P的父结点，当前结点P变为右孩子V的左孩子，右孩子V的左子树R变为当前结点的右子树。

{{< image src="/红黑树/left_rotate.webp" width="100%" >}}

- **右旋**：左孩子F变为当前结点P的父结点，当前结点P变为左孩子F的右孩子，左孩子F的右子树K变为当前结点的左子树。

{{< image src="/红黑树/right_rotate.webp" width="100%" >}}

- **变色**：结点黑红互转。

#### 插入

- 空树：
-

[30张图带你彻底理解红黑树](https://www.jianshu.com/p/e136ec79235c)

### 缓存数据结构

#### LRU (Least Recently Used)

最近最少使用算法。当容量满时，将最久没有使用过的缓存删除。

```java
class Node {
    int key, val;
    Node prev, next;
    public Node(int key, int val) {
        this.key = key;
        this.val = val;
    }
}

class BiLinkedList {
    final Node head, tail;
    final int capacity;
    int length;

    public BiLinkedList(int capacity) {
        this.capacity = capacity;
        length = 0;
        head = new Node(0, 0);
        tail = new Node(0, 0);
        head.next = tail;
        tail.prev = head;
    }

    /**
     * 将结点加入双向链表的尾部
     *
     * @param node 待加入结点
     */
    public void addLast(Node node) {
        node.prev = tail.prev;
        node.next = tail;
        node.prev.next = node;
        tail.prev = node;
        length++;
    }

    /**
     * 将 node 从双向链表中删除
     *
     * @param node 待加入结点
     */
    public void remove(Node node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
        length--;
    }

    /**
     * 将双向链表的第一个结点删除
     */
    public void removeFirst() {
        if (length == 0) {
            return;
        }
        head.next = head.next.next;
        head.next.prev = head;
        length--;
    }
}

class LRUCache {
    private BiLinkedList list;
    private HashMap<Integer, Node> keyToNode;

    public LRUCache(int capacity) {
        list = new BiLinkedList(capacity);
        keyToNode = new HashMap<>();
    }

    public int get(int key) {
        if (keyToNode.containsKey(key)) {
            Node node = keyToNode.get(key);
            list.remove(node);
            list.addLast(node);
            return node.val;
        } else {
            return -1;
        }
    }

    public void put(int key, int value) {
        if (keyToNode.containsKey(key)) {
            Node node = keyToNode.get(key);
            node.val = value;
            list.remove(node);
            list.addLast(node);
        } else {
            if (list.length == list.capacity) {
                keyToNode.remove(list.head.next.key);
                list.removeFirst();
            }
            Node node = new Node(key, value);
            keyToNode.put(key, node);
            list.addLast(node);
        }
    }
}
```

```java
class LRUCache {
    private LinkedHashMap<Integer, Integer> map;
    private int capacity;

    public LRUCache(int capacity) {
        map = new LinkedHashMap<>();
        this.capacity = capacity;
    }

    public int get(int key) {
        int val = map.getOrDefault(key, -1);
        if (map.containsKey(key)) {
            map.remove(key);
            map.put(key, val);
        }
        return val;
    }

    public void put(int key, int value) {
        map.remove(key);
        map.put(key, value);
        if (map.size() > this.capacity) {
            map.remove(map.keySet().iterator().next());
        }
    }
}
```

#### LFU (Least Frequently Used)

```java
class Node {
    int key, value;
    Node prev, next;
    public Node (int key, int value) {
        this.key = key;
        this.value = value;
    }
}

class BiLinkedList {
    final Node head, tail;
    int length;

    public BiLinkedList() {
        this.head = new Node(0, 0);
        this.tail = new Node(0, 0);
        this.length = 0;
        head.next = tail;
        tail.prev = head;
    }

    public void addLast(Node node) {
        node.prev = tail.prev;
        node.next = tail;
        tail.prev.next = node;
        tail.prev = node;
        length++;
    }

    public void remove(Node node) {
        if (node.prev == null || node.next == null) {
            return;
        }
        node.prev.next = node.next;
        node.next.prev = node.prev;
        length--;
    }

    public void removeFirst() {
        if (length == 0) {
            return;
        }
        head.next.next.prev = head;
        head.next = head.next.next;
        length--;
    }
}

class LFUCache {
    HashMap<Integer, Node> keyToNode;
    HashMap<Integer, Integer> keyToFreq;
    HashMap<Integer, BiLinkedList> freqToNodes;
    final int capacity;
    int length;
    int minFreq;

    public LFUCache(int capacity) {
        this.keyToNode = new HashMap<>();
        this.keyToFreq = new HashMap<>();
        this.freqToNodes = new HashMap<>();
        this.capacity = capacity;
        this.length = 0;
        this.minFreq = -1;
    }

    public int get(int key) {
        if (keyToNode.containsKey(key)) {
            Node node = keyToNode.get(key);
            // 更新频率
            int freq = keyToFreq.get(key);
            keyToFreq.put(key, freq + 1);
            // 删除结点
            freqToNodes.get(freq).remove(node);
            // 添加结点
            if (freqToNodes.containsKey(freq + 1)) {
                freqToNodes.get(freq + 1).addLast(node);
            } else {
                BiLinkedList list = new BiLinkedList();
                list.addLast(node);
                freqToNodes.put(freq + 1, list);
            }
            // 更新 minFreq
            if (freq == minFreq) {
                while (freqToNodes.get(minFreq).length == 0) {
                    minFreq++;
                }
            }
            return node.value;
        } else {
            return -1;
        }
    }

    public void put(int key, int value) {
        if (keyToNode.containsKey(key)) {
            // 更新值
            Node node = keyToNode.get(key);
            node.value = value;
            // 更新频率
            int freq = keyToFreq.get(key);
            keyToFreq.put(key, freq + 1);
            // 删除结点
            freqToNodes.get(freq).remove(node);
            // 添加结点
            if (freqToNodes.containsKey(freq + 1)) {
                freqToNodes.get(freq + 1).addLast(node);
            } else {
                BiLinkedList list = new BiLinkedList();
                list.addLast(node);
                freqToNodes.put(freq + 1, list);
            }
            // 更新 minFreq
            if (freq == minFreq) {
                while (freqToNodes.get(minFreq).length == 0) {
                    minFreq++;
                }
            }
        } else {
            if (capacity == 0) {
                return;
            }
            if (length == capacity) {
                int rkey = freqToNodes.get(minFreq).head.next.key;
                freqToNodes.get(minFreq).removeFirst();
                keyToNode.remove(rkey);
                keyToFreq.remove(rkey);
                length--;
            }
            Node node = new Node(key, value);
            keyToNode.put(key, node);
            keyToFreq.put(key, 1);
            if (freqToNodes.containsKey(1)) {
                freqToNodes.get(1).addLast(node);
            } else {
                BiLinkedList list = new BiLinkedList();
                list.addLast(node);
                freqToNodes.put(1, list);
            }
            minFreq = 1;
            length++;
        }
    }
}
```

## 13.分治策略

## 14.贪心策略

## 15.回溯算法

```python
结果集合

def backtrack(当前路径, 可选择列表):
    if 可选择列表为空:
        结果集合添加当前路径
        return

    for 当前选择 in 可选择列表:
        当前路径添加当前选择
        可选择列表删除当前选择
        backtrack(当前路径, 可选择列表)
        当前路径撤销当前选择
        可选择列表添加当前选择
```

### 排列&组合&子集问题

#### 无重复元素&元素不可重复选择

子集

```java
List<List<Integer>> ans = new ArrayList<>();
LinkedList<Integer> set = new LinkedList<>();

/**
 * @param nums 元素唯一的数组
 * @return 所有子集的集合
 */
List<List<Integer>> subsets(int[] nums) {
    dfs(nums, 0);
    return ans;
}

/**
 * @param nums 元素唯一的数组
 * @param start 可选择元素范围[start, nums.length)
 */
void dfs(int[] nums, int start) {
    // 前序添加
    ans.add(new LinkedList<>(set));
    for (int i = start; i < nums.length; i++) {
        set.add(nums[index]);
        dfs(nums, i + 1);
        // 回溯
        set.pollLast();
    }
}
```

组合

```java
List<List<Integer>> ans = new ArrayList<>();
LinkedList<Integer> set = new LinkedList<>();

/**
 * @param n 元素范围[1, n]
 * @param k 组合中元素个数
 * @return 所有长度为 k 的组合的集合
 */
List<List<Integer>> combine(int n, int k) {
    dfs(n, k, 0);
    return ans;
}

/**
 * @param n     元素范围[1, n]
 * @param k     组合中元素个数
 * @param start 可选择元素范围[start, n]
 */
void dfs(int n, int k, int start) {
    if (set.size() == k) {
        ans.add(new LinkedList<>(set));
        return;
    }
    for (int i = start; i <= n; i++) {
        set.add(i);
        dfs(n, k, i + 1);
        // 回溯
        set.pollLast();
    }
}
```

排列

```java {hl_lines=[26]}
List<List<Integer>> ans = new ArrayList<>();
LinkedList<Integer> set = new LinkedList<>();
boolean[] vis;

/**
 * @param nums 元素唯一的数组
 * @return 所有排列的集合
 */
List<List<Integer>> permute(int[] nums) {
    vis = new boolean[nums.length];
    dfs(nums);
    return ans;
}

/**
 * @param nums 元素唯一的数组
 */
void dfs(int[] nums) {
    int n = nums.length;
    if (set.size() == n) {
        ans.add(new LinkedList<>(set));
        return;
    }
    for (int i = 0; i < n; i++) {
        // 防止重复选
        if (!vis[i]) {
            vis[i] = true;
            set.add(nums[i]);
            dfs(nums);
            // 回溯
            set.pollLast();
            vis[i] = false;
        }
    }
}
```

#### 无重复元素&元素可以重复选择

组合

```java
List<List<Integer>> ans = new ArrayList<>();
LinkedList<Integer> set = new LinkedList<>();

/**
 * @param candidates 无重复元素的数组
 * @param target     目标和
 * @return 所有和为 target 的组合的集合（元素可重复）
 */
List<List<Integer>> combinationSum(int[] candidates, int target) {
    Arrays.sort(candidates);
    dfs(candidates, target, 0, 0);
    return ans;
}

/**
 * @param candidates 有序数组
 * @param target     目标和
 * @param start      可选择元素范围[start, candidates.length)
 * @param sum        当前元素和
 */
void dfs(int[] candidates, int target, int start, int sum) {
    if (sum == target) {
        ans.add(new LinkedList<>(set));
        return;
    }
    for (int i = start; i < candidates.length; i++) {
        // 剪枝
        if (sum + candidates[i] <= target) {
            set.add(candidates[i]);
            // 可选范围不变
            dfs(candidates, target, i, sum + candidates[i]);
            set.pollLast();
        }
    }
}
```

排列

```java
List<List<Integer>> ans = new ArrayList<>();
LinkedList<Integer> set = new LinkedList<>();

/**
 * @param nums 无重复元素的数组
 * @return 所有排列的集合（可重复选择元素）
 */
List<List<Integer>> permuteDup(int[] nums) {
    dfs(nums);
    return ans;
}

/**
 * @param nums 无重复元素的数组
 */
void dfs(int[] nums) {
    int n = nums.length;
    if (set.size() == n) {
        ans.add(new LinkedList<>(set));
        return;
    }
    for (int i = 0; i < n; i++) {
        set.add(nums[i]);
        dfs(nums);
        // 回溯
        set.pollLast();
    }
}
```

#### 有重复元素&元素不可重复选择

子集

```java
List<List<Integer>> ans = new ArrayList<>();
LinkedList<Integer> set = new LinkedList<>();

/**
 * @param nums 可能存在重复元素的数组
 * @return 所有非重复子集的集合
 */
List<List<Integer>> subsetsWithDup(int[] nums) {
    Arrays.sort(nums);
    dfs(nums, 0);
    return ans;
}

/**
 * @param nums  有序数组
 * @param start 可选择元素范围[start, nums.length)
 */
void dfs(int[] nums, int start) {
    ans.add(new LinkedList<>(set));
    for (int i = start; i < nums.length; i++) {
        // 剪枝：相同元素已经选过就不再重复选择
        if (i > start && nums[i - 1] == nums[i]) {
            continue;
        }
        set.add(nums[i]);
        dfs(nums, i + 1);
        // 回溯
        set.pollLast();
    }
}
```

组合

```java {hl_lines=[29]}
List<List<Integer>> ans = new ArrayList<>();
LinkedList<Integer> set = new LinkedList<>();

/**
 * @param candidates 可能存在重复元素的正数数组
 * @param target     目标和
 * @return 所有和为 target 的组合的集合
 */
List<List<Integer>> combinationSum2(int[] candidates, int target) {
    Arrays.sort(candidates);
    dfs(candidates, target, 0, 0);
    return ans;
}

/**
 * @param candidates 有序数组
 * @param target     目标和
 * @param start      可选择元素范围[start, candidates.length)
 * @param sum        当前元素和
 */
void dfs(int[] candidates, int target, int start, int sum) {
    if (sum == target) {
        ans.add(new LinkedList<>(set));
        return;
    }
    for (int i = start; i < candidates.length; i++) {
        // 剪枝1：相同元素已经选过就不再重复选择
        // 即相同元素只能连续选择，不能间隔选择
        if (i > start && candidates[i - 1] == candidates[i]) {
            continue;
        }
        // 剪枝2：只有加上当前元素后元素和不超过 target 才选择
        if (sum + candidates[i] <= target) {
            set.add(candidates[i]);
            dfs(candidates, target, i + 1, sum + candidates[i]);
            set.pollLast();
        }
    }
}
```

排列

```java {hl_lines=[29]}
List<List<Integer>> ans = new ArrayList<>();
LinkedList<Integer> set = new LinkedList<>();
boolean[] vis;

/**
 * @param nums 可能存在重复元素的数组
 * @return 所有排列的集合
 */
List<List<Integer>> permuteUnique(int[] nums) {
    vis = new boolean[nums.length];
    Arrays.sort(nums);
    dfs(nums);
    return ans;
}

/**
 * @param nums 有序数组
 */
void dfs(int[] nums) {
    int n = nums.length;
    if (set.size() == n) {
        ans.add(new LinkedList<>(set));
        return;
    }
    for (int i = 0; i < n; i++) {
        if (!vis[i]) {
            // 剪枝：相同元素已经选过就不再重复选择
            // 即相同元素只能连续选择，不能间隔选择
            if (i > 0 && !vis[i - 1] && nums[i - 1] == nums[i]) {
                continue;
            }
            vis[i] = true;
            set.add(nums[i]);
            dfs(nums);
            // 回溯
            set.pollLast();
            vis[i] = false;
        }
    }
}
```

## 16.动态规划（DP）

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

## 17.计算几何

### 凸包问题

在给定点集中找出所有的边界点。

#### Jarvis 算法

暴力

#### Graham 算法

#### Andrew 算法

[安装栅栏 - 安装栅栏 - 力扣（LeetCode）](https://leetcode-cn.com/problems/erect-the-fence/solution/an-zhuang-zha-lan-by-leetcode-solution-75s3/)

## 18.常用算法

### 快速幂

快速求`x`的`n`次幂。

#### 迭代

```java
double fastPow(double x, int n) {
    if (x == 0) {
        return 0;
    }
    // 防止 n = -214748328 时，-n 溢出
    long nn = n;
    double ans = 1;
    if (nn < 0) {
        // 处理 n < 0
        x = 1 / x;
        nn = -nn;
    }
    while (nn > 0) {
        if ((nn & 1) == 1) {
            ans *= x;
        }
        x *= x;
        n >>= 1;
    }
    return ans;
}
```

#### 递归

```java
double fastPow(double x, int n) {
    // 防止 n = -214748328 时，-n 溢出
    long nn = n;
    if (nn < 0) {
        // 处理 n < 0
        x = 1 / x;
        nn = -nn;
    }
    return pow(x, nn);
}

private double pow(double x, long n) {
    if (n == 0) {
        return 1;
    } else if (n == 1) {
        return x;
    } else {
        double half = pow(x, n >> 1);
        if ((n & 1) == 1) {
            return half * half * x;
        } else {
            return half * half;
        }
    }
}
```

### 滑动窗口

#### 窗口大小可任意调整

```java
// 窗口大小可任意调整
int left = 0;
int right = 0;
int ans = 0;
// [left, right]
while (right < n) {
    // 增大窗口
    // 右端点 right 操作
    // 修改约束值
    while (condition) { // 约束值满足调整窗口的条件
        // 缩小窗口
        // 左端点 left 操作
        // 修改约束值
        left++;
    }
    ans = Math.max(ans, right - left + 1);
    right++;
}
```

#### 窗口大小单调递增

```java
// 窗口大小单调递增
int left = 0;
int right = 0;
// [left, right]
while (right < n) {
    // 增大窗口
    // 右端点 right 操作
    // 修改约束值
    if (condition) { // 约束值满足调整窗口的条件
        // 此时左端点移动最多使窗口大小不变
        // 左端点 left 操作
        // 修改约束值
        left++;
    }
    right++;
}
int ans = right - left;
```

### 约瑟夫环

[找出游戏的获胜者](https://leetcode.cn/problems/find-the-winner-of-the-circular-game/)

**倒推法**

1. 还剩`1`个人，此人获胜，下标为

$$f(1,k)=0$$

2. 还剩`2`个人，淘汰第`k`个人，即淘汰下标`(k-1)%2`，下次从`k%2`开始，则获胜者下标为

$$f(2,k)=(f(1,k)+k)\mod 2=(0+k)\mod 2$$

3. 还剩`3`个人，淘汰第`k`个人，即淘汰下标`(k-1)%3`，下次从`k%3`开始，则获胜者下标为

$$f(3,k)=(f(2,k)+k)\mod 3=((0+k)\mod 2+k)\mod 3$$

4. ...

5. `n`个人，淘汰第`k`个人，即淘汰下标`(k-1)%n`，下次从`k%n`开始，则获胜者下标为

$$f(n,k)=(f(n-1,k)+k)\bmod{n}=(\cdots((0+k)\bmod{2}+k)\bmod{3}\cdots)\bmod{n}$$

```java
int findTheWinner(int n, int k) {
    int ans = 0;
    for (int i = 2; i <= n; i++) {
        ans = (ans + k) % i;
    }
    return ans + 1;
}
```

