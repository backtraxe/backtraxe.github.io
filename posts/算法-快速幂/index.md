# 算法-快速幂


<!--more-->

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

