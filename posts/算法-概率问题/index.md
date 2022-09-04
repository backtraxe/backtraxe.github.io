# 算法-概率问题


<!--more-->

## 1.基础

### 水塘抽样

$$
\begin{aligned}
P &= P(第i个节点的值成为最后被返回的值) \newline
&= P(第i次随机选择的值=0) \times P(第i+1次随机选择的值 \ne 0) \times \cdots \times P(第n次随机选择的值 \ne 0) \newline
&= \frac{1}{i} \times (1-\frac{1}{i+1}) \times \cdots \times (1-\frac{1}{n}) \newline
&= \frac{1}{i} \times \frac{i}{i+1} \times \cdots \times \frac{n-1}{n} \newline
&= \frac{1}{n}
\end{aligned}
$$

## 2.实战

### 非重叠矩形中的随机点

[497. 非重叠矩形中的随机点](https://leetcode.cn/problems/random-point-in-non-overlapping-rectangles/)

```java

```

### 🟨链表随机节点

[382. 链表随机节点](https://leetcode.cn/problems/linked-list-random-node/)

- 水塘抽样

```java
class Solution {
    ListNode head;
    Random random;

    public Solution(ListNode head) {
        this.head = head;
        random = new Random();
    }

    public int getRandom() {
        int i = 1;
        int ans = 0;
        for (ListNode node = head; node != null; node = node.next) {
            // 1/i 的概率选中（替换为答案）
            if (random.nextInt(i) == 0) ans = node.val;
            ++i;
        }
        return ans;
    }
}
```

## 参考

1. [链表随机节点 - 链表随机节点 - 力扣（LeetCode）](https://leetcode.cn/problems/linked-list-random-node/solution/lian-biao-sui-ji-jie-dian-by-leetcode-so-x6it/)

