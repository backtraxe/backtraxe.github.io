# 算法模板库


<!--more-->

## 滑动窗口



## 二分查找

```cpp
int binarySearch(vector<int> arr, const int target) {
    // 升序数组
    // [low, high]
    int low = 0, high = arr.size() - 1, mid;
    while (low <= high) {
        mid = low + (high - low) / 2;
        if (target == arr[mid]) {
            return mid;
        } else if (target > arr[mid]) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    return -1;
}
```

## 小技巧

头文件

```cpp
#include<bits/stdc++.h>
```

判断奇偶

```cpp
if (x & 1)  // 奇数，等价于 x % 2
else        // 偶数
```

快速乘除2

```cpp
x <<= 1;  // 乘2，等价于 x *= 2;
x >>= 1;  // 除2，等价于 x /= 2;
```

快速交换

```cpp
a ^= b;  // a1 = a ^ b
b ^= a;  // b1 = b ^ a1 = b ^ a ^ b = a
a ^= b;  // a2 = a1 ^ b1 = a ^ b ^ a = b
```

遍历字符串

```cpp
for (int i = 0; s[i]; i++)
```

使用 `emplace_back()` 代替 `push_back()`

内置求最大公约数函数：`__gcd(x, y);`

使用 `inline` 函数

全局数组最大 $ 10^7 $，函数内数组最大 $ 10^6 $

得到最高有效位数字

```cpp
double k = log(n, 10);
k -= floor(k);
x = pow(10, k);
```

得到数字的有效位数

```cpp
n = floor(log(n, 10)) + 1;
```

判断是否是 2 的幂（Brian Kernighan’s Algorithm）

```cpp
// log(n)
n && (!(n & (n - 1)))
```

C++11 内置 STL 函数

```cpp
// 是否全是正数？
all_of(first, first + n, [](int x) { return x > 0; });
// 是否存在正数
any_of(first, first + n, [](int x) { return x > 0; });
// 是否全不是正数？
none_of(first, first + n, [](int x) { return x > 0; });

// 复制
int source[5] = {0, 12, 34, 50, 80};
int target[5];
copy_n(source, 5, target);

// 迭代
int a[5] = {0};
char c[3] = {0};
iota(a, a + 5, 10);   // {10, 11, 12, 13, 14}
iota(c, c + 3, 'a');  // {'a', 'b', 'c'}
```

二进制表示

```cpp
auto number = 0b011;
cout << number;  // 3
```

Using Range based for loop


