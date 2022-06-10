# 力扣 0069 X的平方根


<!--more-->

[69. x 的平方根](https://leetcode.cn/problems/sqrtx/)

## 方法一：数学

- $\sqrt{x}=x^{\frac{1}{2}}=(e^{\ln x})^{\frac{1}{2}}=e^{\frac{1}{2}\ln x}$

> 由于计算机无法存储浮点数的精确值，因此运算过程中会存在误差。例如当 $x = 2147395600$ 时的计算结果与正确值 $46340$ 相差 $10^{-11}$，这样在对结果取整数部分时，会得到 $46339$ 这个错误的结果。

```java
class Solution {
    public int mySqrt(int x) {
        if (x == 0) return 0;
        int ans = (int) Math.exp(0.5 * Math.log(x));
        return (long) (ans + 1) * (ans + 1) <= x ? ans + 1 : ans;
    }
}
```

