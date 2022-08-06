# 力扣 1545 找出第N个二进制字符串中的第K位


[1545. 找出第 N 个二进制字符串中的第 K 位](https://leetcode.cn/problems/find-kth-bit-in-nth-binary-string/)

<!--more-->

```java
class Solution {
    public char findKthBit(int n, int k) {
        if (k == 1) return '0';
        int mid = 1 << (n - 1); // 找中点
        if (k == mid) return '1';
        else if (k < mid) return findKthBit(n - 1, k); // 顺序找左边
        else return (char) ('0' + '1' - findKthBit(n - 1, mid * 2 - k)); // 逆序找右边，且翻转
    }
}
```

