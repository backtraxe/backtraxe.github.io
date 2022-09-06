# 算法-数论


<!--more-->

## 质数

### 判断质数

```java
static boolean isPrime(int n) {
    if (n < 2) return false;
    // i * i <= n 可能溢出
    for (int i = 2; i <= n / i; i++)
        if (n % i == 0) return false;
    return true;
}
```

### 埃氏筛法求质数

```java
// isNotPrime[i] 表示 i 是否不是质数
static boolean[] isNotPrime = new boolean[10000];

static void getPrime(int n) {
    // 判断 [1, n] 中每个数是否是质数
    for (int i = 2; i <= n; i++) {
        if (isNotPrime[i]) continue;
        for (int j = i; j <= n / i; j++)
            isNotPrime[i * j] = true;
    }
}
```

### 质因数分解

```java
static List<List<Integer>> decomposition(int n) {
    // 质因数从小到大排序
    // factors[i][0] 表示第 i 个质因数
    // factors[i][1] 表示第 i 个质因数的数量
    List<List<Integer>> factors = new ArrayList<>();
    for (int i = 2; i <= n / i; i++) {
        if (n % i == 0) { // 质因数
            List<Integer> factor = new ArrayList<>();
            factor.add(i);
            int cnt = 0;
            while (n % i == 0) { // 计算质因数数量
                cnt++;
                n /= i;
            }
            factor.add(cnt);
            factors.add(factor);
        }
    }
    if (n > 1) { // 最后剩下的质因数
        List<Integer> factor = new ArrayList<>();
        factor.add(n);
        factor.add(1);
        factors.add(factor);
    }
    return factors;
}
```

```cpp
// 质因数从小到大排序
// factors[i][0] 表示第 i 个质因数
// factors[i][1] 表示第 i 个质因数的数量
int factors[100][2];
int n_factor = 0; // 不同质因数的数量

void decomposition(int n) {
    for (int i = 2; i <= n / i; i++) {
        if (n % i == 0) {
            factors[n_factor][0] = i;
            while (n % i == 0) {
                factors[n_factor][1]++;
                n /= i;
            }
            n_factor++;
        }
    }
    if (n > 1) {
        factors[n_factor][0] = n;
        factors[n_factor++][1] = 1;
    }
}
```

## 最大公约数、最小公倍数

Greatest Common Divisor

Least Common Multiple

```java
static int gcd(int a, int b) {
    return b == 0 ? a : gcd(b, a % b);
}

static int lcm(int a, int b) {
    return a * b / gcd(a, b);
}
```

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
