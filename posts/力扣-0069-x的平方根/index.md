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

- 时间复杂度：$ O(1) $

<br />

## 方法二：二分查找

```java
public class Solution {
    public int mySqrt(int x) {
        int left = 0;
        int right = x;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if ((long) mid * mid <= x) left = mid + 1;
            else right = mid - 1;
        }
        return right;
    }
}
```

- 时间复杂度：$ O(\log x) $

<br />

## 方法三：牛顿迭代法

- 由 $\sqrt{x} = z$ 得 $z^2-x=0 \ (z>0)$，则 $z$ 为 $f(z)=z^2-x$ 的零点。
- 我们任取一个 $z_0$ 作为初始值。
- 在每一步的迭代中，我们找到函数图像上的点 $(z_i, f(z_i))$，过该点作一条斜率为该点导数 $f'(z_i)=2z_i$ 的直线

$$g(z)=2z_i(z-z_i)+f(z_i)=2z_iz-z_i^2-x$$

- 该直线与 x 轴的交点为 $(z_{i+1},0)$，即 $(\frac{x+z_i^2}{2z_i},0)$。
- 经过多次迭代后，$z_i$ 会逐渐接近零点。

```java
class Solution {
    public int mySqrt(int x) {
        if (x == 0) return 0;
        double C = x, x0 = x;
        while (true) {
            double xi = 0.5 * (x0 + C / x0);
            if (Math.abs(x0 - xi) < 1e-7) {
                break;
            }
            x0 = xi;
        }
        return (int) x0;
    }
}
```

- 时间复杂度：$ O(\log x) $

