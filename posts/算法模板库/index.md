# 算法模板库


常用算法模板，支持 C++、Java。

<!--more-->

## 排序

**排序模板**

```java
public class SortTemplate {
    public static void sort(Comparable[] array) {}

    private static boolean less(Comparable a, Comparable b) { return a.compareTo(b) < 0; }

    private static void exchange(Comparable[] array, int i, int j) {
        Comparable temp = array[i];
        array[i] = array[j];
        array[j] = temp;
    }

    private static void show(Comparable[] array) {
        for (Comparable elem : array) {
            System.out.print(elem + " ");
        }
        System.out.println();
    }

    private static boolean isSorted(Comparable[] array) {
        for (int i = 1; i < array.length; i++) {
            if (less(array[i], array[i - 1])) {
                return false;
            }
        }
        return true;
    }
}
```

### 1 选择排序

```java
public class SelectionSort {
    public static void sort(Comparable[] array) {
        int len = array.length;
        for (int i = 0; i < len; i++) {
            int minIdx = i;
            for (int j = i + 1; j < len; j++) {
                if (less(array[j], array[minIdx])) {
                    minIdx = j;
                }
            }
            exchange(array, i, minIdx);
        }
    }

    private static boolean less(Comparable a, Comparable b) { return a.compareTo(b) < 0; }

    private static void exchange(Comparable[] array, int i, int j) {
        Comparable temp = array[i];
        array[i] = array[j];
        array[j] = temp;
    }
}
```

### 2 插入排序

```java
public class InsertionSort {
    public static void sort(Comparable[] array) {
        int len = array.length;
        for (int i = 1; i < len; i++) {
            for (int j = i; j > 0 && less(array[j], array[j - 1]); j--) {
                exchange(array, j, j - 1);
            }
        }
    }

    private static boolean less(Comparable a, Comparable b) { return a.compareTo(b) < 0; }

    private static void exchange(Comparable[] array, int i, int j) {
        Comparable temp = array[i];
        array[i] = array[j];
        array[j] = temp;
    }
}
```

### 3 希尔排序

```java
public class ShellSort {
    public static void sort(Comparable[] array) {
        int len = array.length;
        int step = 1;
        while (step < len / 3) {
            // 1, 4, 13, 40, 121, 364, 1093, ...
            step = step * 3 + 1;
        }
        while (step >= 1) {
            for (int i = step; i < len; i++) {
                for (int j = i; j >= step && less(array[j], array[j - step]); j -= step) {
                    exchange(array, j, j - step);
                }
            }
            step /= 3;
        }
    }

    private static boolean less(Comparable a, Comparable b) {
        return a.compareTo(b) < 0;
    }

    private static void exchange(Comparable[] array, int i, int j) {
        Comparable temp = array[i];
        array[i] = array[j];
        array[j] = temp;
    }
}
```

### 4 归并排序

**自顶向下**

```java
public class MergeSort {
    private static Comparable[] copy;

    public static void sort(Comparable[] array) {
        int len = array.length;
        copy = new Comparable[len];
        sort(array, 0, len - 1);
    }

    private static void sort(Comparable[] array, int low, int high) {
        // [low, high]
        if (low >= high) return;
        int mid = low + (high - low) / 2;
        sort(array, low, mid);
        sort(array, mid + 1, high);
        merge(array, low, mid, high);
    }

    private static void merge(Comparable[] array, int low, int mid, int high) {
        // [low, high]
        int i = low;
        int j = mid + 1;
        if (high + 1 - low >= 0) System.arraycopy(array, low, copy, low, high + 1 - low);
        for (int k = low; k <= high; k++) {
            if (i > mid) {
                array[k] = copy[j++];
            } else if (j > high) {
                array[k] = copy[i++];
            } else if (less(copy[j], copy[i])) {
                array[k] = copy[j++];
            } else {
                array[k] = copy[i++];
            }
        }
    }

    private static boolean less(Comparable a, Comparable b) { return a.compareTo(b) < 0; }
}
```

**自底向上**

```java
public class MergeSort {
    private static Comparable[] copy;

    public static void sort(Comparable[] array) {
        int len = array.length;
        copy = new Comparable[len];
        for (int step = 1; step < len; step *= 2) {
            for (int low = 0; low < len - step; low += 2 * step) {
                merge(array, low, low + step - 1, Math.min(low + 2 * step - 1, len - 1));
            }
        }
    }

    private static void merge(Comparable[] array, int low, int mid, int high) {
        // [low, high]
        int i = low;
        int j = mid + 1;
        if (high + 1 - low >= 0) System.arraycopy(array, low, copy, low, high + 1 - low);
        for (int k = low; k <= high; k++) {
            if (i > mid) {
                array[k] = copy[j++];
            } else if (j > high) {
                array[k] = copy[i++];
            } else if (less(copy[j], copy[i])) {
                array[k] = copy[j++];
            } else {
                array[k] = copy[i++];
            }
        }
    }

    private static boolean less(Comparable a, Comparable b) { return a.compareTo(b) < 0; }
}
```

### 5 快速排序

```java
public class QuickSort {
    public static void sort(Comparable[] array) {
        sort(array, 0, array.length - 1);
    }

    public static void sort(Comparable[] array, int low, int high) {
        // [low, high]
        if (low >= high) return;
        int i = partition(array, low, high);
        sort(array, low, i - 1);
        sort(array, i + 1, high);
    }

    private static int partition(Comparable[] array, int low, int high) {
        // [low, high]
        int i = low;
        int j = high + 1;
        Comparable copy = array[low];
        while (true) {
            while (less(array[++i], copy)) {
                if (i == high) break;
            }
            while (less(copy, array[--j])) {
                if (j == low) break;
            }
            if (i >= j) break;
            exchange(array, i, j);
        }
        exchange(array, low, j);
        return j;
    }

    private static boolean less(Comparable a, Comparable b) { return a.compareTo(b) < 0; }

    private static void exchange(Comparable[] array, int i, int j) {
        Comparable temp = array[i];
        array[i] = array[j];
        array[j] = temp;
    }
}
```

## 算法

### 滑动窗口

### 二分查找

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

## C++ 实用技巧

### STL 常用库

```cpp
/*** 函数 ***/
#include<algorithm>
#include<functional> // hash
#include<climits>    // 常量
#include<cmath>
#include<cstdio>
#include<cstdlib>    // random
#include<ctime>
#include<iostream>
#include<sstream>
#include<iomanip>    // 右对齐 std::right 设置宽度 std::setw(width)
/*** 数据结构 ***/
#include<deque>
#include<list>
#include<queue>      // 包括 priority_queue
#include<stack>
#include<string>
#include<vector>
```

### I/O

```cpp
#include<iostream> // cin cout
#include<cstdio>   // scanf printf
// cin does not concern with ’\n’ at end of each line
// however scanf or getline does concern with ’\n’ at end of each line
// ’\n’ will be ignored when you use cin to read char.

// 读取数值数据后在读取字符串
cin >> n;
getline(cin, str) // wasted getline
getline(cin, str) // real input string
```

