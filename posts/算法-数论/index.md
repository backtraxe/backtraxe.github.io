# 算法-数论


<!--more-->

## 素数

### 判断素数

```java
static boolean isPrime(int n) {
    if (n < 2) return false;
    if (n > 2 && n % 2 == 0) return false;
    for (int i = 3; i <= (int) Math.sqrt(n); i += 2) // i * i <= n 可能溢出
        if (n % i == 0) return false;
    return true;
}
```

<br>

## 平方根

### 二分法

```java
static double sqrt(double x) {
    if (x < 0) return Double.NaN;
    double threshold = 1e-15;
    double left = 0.0;
    double right = x;
    while (true) {
        double mid = (left + right) / 2;
        if (Math.abs(mid * mid - x) < threshold) return mid;
        else if (mid * mid > x) right = mid;
        else left = mid;
    }
}
```

<br>

### 牛顿迭代法

1. 对于函数 $y=\sqrt{x}$，计算 $y_0=\sqrt{x_0}$。
2.

$$
y = \sqrt{x} \newline
y' = \frac{1}{2\sqrt{x}}
$$

```java
static double sqrt(double x) {
    if (x < 0) return Double.NaN;
    double threshold = 1e-15;
    double x1 = x;
    while (Math.abs(x1 - x / x1) > threshold * x1)
        x1 = (x / x1 + x1) / 2;
    return x1;
}
```
