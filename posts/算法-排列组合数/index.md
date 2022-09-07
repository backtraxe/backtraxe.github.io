# 算法-排列组合数


<!--more-->

## 排列数

$A_n^m$ 表示从 $n$ 个不同的数字中选择 $m$ 个数字，**有序排列**，不同排列的数量。

$$
\begin{aligned}
\newline
A_n^0 &= 1 \newline
A_n^1 &= n \newline
A_n^n &= n(n-1)(n-2) \cdots 1 = n! \newline
A_n^m &= n(n-1)(n-2) \cdots (n-m+1) = \frac{n!}{(n-m)!} = \frac{A_n^n}{A_{n-m}^{n-m}} \newline
A_n^m &= n A_{n-1}^{m-1} \newline
A_n^m &= m A_{n-1}^{m-1} + A_{n-1}^m \newline
\newline
\end{aligned}
$$

## 组合数

### 公式

$C_n^m$ 表示从 $n$ 个不同的数字中选择 $m$ 个数字，**无序**，不同选择的数量。

$$
\begin{aligned}
\newline
C_n^0 &= C_n^n = 1 \newline
C_n^1 &= n \newline
C_n^m &= C_n^{n-m} \newline
C_n^m &= \frac{n(n-1) \cdots (n-m+1)}{m(m-1) \cdots 1} = \frac{A_n^m}{A_m^m} = \frac{n!}{m!(n-m)!} = \frac{A_n^n}{A_m^m A_{n-m}^{n-m}} \newline
C_n^{n-m} &= \frac{n(n-1) \cdots (m+1)}{(n-m)(n-m+1) \cdots 1} \newline
C_n^m &= C_{n-1}^m + C_{n-1}^{m-1} \newline
2^n &= C_n^0 + C_n^1 + \cdots + C_n^n \newline
\newline
\end{aligned}
$$

### 实现

```java
static long combination(int n, int m) {
    // C(n, m)
    m = Math.max(m, n - m); // C(n, m) = C(n, n - m)
    long ans = 1L;
    for (int i = 1; i <= n - m; i++) {
        ans *= m + i;
        ans /= i;
    }
    return ans;
}
```

```java

```

## 计算

- **捆绑法**：n 个不同元素的排列数，要求其中 m 个元素必须相邻。$A_{m}^{m} A_{n-m+1}^{n-m+1}$
- **插空法**：n 个不同元素的排列数，要求其中 m 个元素不能相邻。$A_{n-m}^{n-m} A_{n-m+1}^m$
- **缩倍法**：n 个不同元素的排列数，要求固定其中 m 个元素的顺序。$A_n^n / A_m^m$
- **隔板法**：n 个相同元素分成 m 组的不同划分方法，每组不为空。$C_{n-1}^{m-1}$

## 参考

1. [关于排列组合的一个总结 - 知乎](https://zhuanlan.zhihu.com/p/56326836)

