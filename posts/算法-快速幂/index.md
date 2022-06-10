# 算法-快速幂


<!--more-->

## 1.介绍

- 快速求`x`的`n`次幂。
- 时间复杂度：$O(\log n)$

<br />

## 2.迭代写法

```java
static double fastPow(double x, int n) {
    if (x == 0)      return 0;
    else if (x == 1) return 1.0;
    // 防止 n = -214748328 时，-n 溢出
    long nn = n;
    double ans = 1.0;
    if (nn < 0) {
        x = 1 / x;
        nn = -nn;
    }
    while (nn > 0) {
        if ((nn & 1) == 1) ans *= x;
        x *= x;
        nn >>= 1;
    }
    return ans;
}
```

<br />

## 3.递归写法

```java
static double fastPow(double x, int n) {
    if (x == 0)      return 0;
    else if (x == 1) return 1.0;
    // 防止 n = -214748328 时，-n 溢出
    long nn = n;
    if (nn < 0) {
        x = 1 / x;
        nn = -nn;
    }
    return fastPow(x, nn);
}
```

```java
static double fastPow(double x, long n) {
    if (n == 0)      return 1.0;
    else if (n == 1) return x;
    else {
        double half = fastPow(x, n >> 1);
        if ((n & 1) == 1) return half * half * x;
        else              return half * half;
    }
}
```

<br />

## 4.矩阵快速幂

```java
static int[][] fastPow(int[][] mat, int n) {
    if (n == 0) return mat;
    // 单位矩阵
    int m = mat.length;
    int[][] ans = new int[m][m];
    for (int i = 0; i < m; i++)
        for (int j = 0; j < m; j++)
            if (i == j) ans[i][j] = 1;
    while (n > 0) {
        if ((n & 1) == 1) ans = matMul(ans, mat);
        mat = matMul(mat, mat);
        n >>= 1;
    }
    return ans;
}
```

```java
static int[][] matMul(int[][] a, int[][] b) {
    // [m, h] * [h, n] -> [m, n]
    int m = a.length;
    int h = a[0].length;
    int h1 = b.length;
    int n = b[0].length;
    if (h != h1) return null;
    int[][] ans = new int[m][n];
    for (int i = 0; i < m; i++)
        for (int j = 0; j < n; j++)
            for (int k = 0; k < h; k++)
                ans[i][j] += a[i][k] * b[k][j];
    return ans;
}
```

<br />

## 5.实战

- [372. 超级次方](https://leetcode.cn/problems/super-pow/)
- [50. Pow(x, n)](https://leetcode.cn/problems/powx-n/)

