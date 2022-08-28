# 算法-二分查找


<!--more-->

## 1.基础

- 要求序列**非递减**，即`nums[i - 1] <= nums[i]`
- 时间复杂度：$O(\log n)$

## 2.等于指定值

- **返回值：**

返回数组中**等于**指定值的元素的下标。

- **测试结果：**

```java
int[] nums = { 4, 5, 5, 6, 7 };
binarySearch(nums, 3); // -1
binarySearch(nums, 4); // 0
binarySearch(nums, 5); // 1 或 2
binarySearch(nums, 6); // 3
binarySearch(nums, 7); // 4
binarySearch(nums, 8); // -1
```

- **闭区间写法：**

```java
static int binarySearch(int[] nums, int target) {
    // 二分查找等于 target 的下标
    // 闭区间 [left, right]
    int left = 0;
    int right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target)     return mid;
        else if (nums[mid] < target) left = mid + 1;
        else                         right = mid - 1;
    }
    return -1;
}
```

- **左闭右开写法：**

```java
static int binarySearch(int[] nums, int target) {
    // 二分查找等于 target 的下标
    // 左闭右开 [left, right)
    int left = 0;
    int right = nums.length;
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target)     return mid;
        else if (nums[mid] < target) left = mid + 1;
        else                         right = mid;
    }
    return -1;
}
```

## 3.第一个大于指定值

- **返回值：**

返回数组中**大于**指定值的**最小元素**的下标。

- **测试结果：**

```java
int[] nums = { 4, 5, 5, 6, 7 };
higher(nums, 3); // 0
higher(nums, 4); // 1
higher(nums, 5); // 3
higher(nums, 6); // 4
higher(nums, 7); // 5
higher(nums, 8); // 5
```

- **闭区间写法：**

```java
static int higher(int[] nums, int target) {
    // 二分查找第一个大于 target 的下标
    // 闭区间 [left, right]
    int left = 0;
    int right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] > target) right = mid - 1;
        else                    left = mid + 1;
    }
    return left;
}
```

- **左闭右开写法：**

```java
static int higher(int[] nums, int target) {
    // 二分查找第一个大于 target 的下标
    // 左闭右开 [left, right)
    int left = 0;
    int right = nums.length;
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] > target) right = mid;
        else                    left = mid + 1;
    }
    return left;
}
```

## 4.第一个大于等于指定值

- **返回值：**

返回数组中**大于等于**指定值的**最小元素**的下标。

- **测试结果：**

```java
int[] nums = { 4, 5, 5, 6, 7 };
ceiling(nums, 3); // 0
ceiling(nums, 4); // 0
ceiling(nums, 5); // 1
ceiling(nums, 6); // 3
ceiling(nums, 7); // 4
ceiling(nums, 8); // 5
```

- **闭区间写法：**

```java
static int ceiling(int[] nums, int target) {
    // 二分查找第一个大于等于 target 的下标
    // 闭区间 [left, right]
    int left = 0;
    int right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] >= target) right = mid - 1;
        else                     left = mid + 1;
    }
    return left;
}
```

- **左闭右开写法：**

```java
static int ceiling(int[] nums, int target) {
    // 二分查找第一个大于等于 target 的下标
    // 左闭右开 [left, right)
    int left = 0;
    int right = nums.length;
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] >= target) right = mid;
        else                     left = mid + 1;
    }
    return left;
}
```

## 5.最后一个小于指定值

- **返回值：**

返回数组中**小于**指定值的**最大元素**的下标。

- **测试结果：**

```java
int[] nums = { 4, 5, 5, 6, 7 };
lower(nums, 3); // -1
lower(nums, 4); // -1
lower(nums, 5); // 0
lower(nums, 6); // 2
lower(nums, 7); // 3
lower(nums, 8); // 4
```

- **闭区间写法：**

```java
static int lower(int[] nums, int target) {
    // 二分查找第一个小于 target 的下标
    // 闭区间 [left, right]
    int left = 0;
    int right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) left = mid + 1;
        else                    right = mid - 1;
    }
    return right;
}
```

- **左闭右开写法：**

```java
static int lower(int[] nums, int target) {
    // 二分查找第一个小于 target 的下标
    // 左闭右开 [left, right)
    int left = 0;
    int right = nums.length;
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) left = mid + 1;
        else                    right = mid;
    }
    return right - 1;
}
```

## 6.最后一个小于等于指定值

- **返回值：**

返回数组中**小于等于**指定值的**最大元素**的下标。

- **测试结果：**

```java
int[] nums = { 4, 5, 5, 6, 7 };
floor(nums, 3); // -1
floor(nums, 4); // 0
floor(nums, 5); // 2
floor(nums, 6); // 3
floor(nums, 7); // 4
floor(nums, 8); // 4
```

- **闭区间写法：**

```java
static int floor(int[] nums, int target) {
    // 二分查找最后一个小于等于 target 的下标
    // 闭区间 [left, right]
    int left = 0;
    int right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] <= target) left = mid + 1;
        else                     right = mid - 1;
    }
    return right;
}
```

- **左闭右开写法：**

```java
static int floor(int[] nums, int target) {
    // 二分查找最后一个小于等于 target 的下标
    // 左闭右开 [left, right)
    int left = 0;
    int right = nums.length;
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] <= target) left = mid + 1;
        else                     right = mid;
    }
    return right - 1;
}
```

## 7.总结

- **闭区间** vs **左闭右开**：

```java
// 闭区间
int right = nums.length - 1;
while (left <= right)
right = mid - 1;

// 左闭右开
int right = nums.length;
while (left < right)
right = mid;
```

- **大于等于** vs **小于等于** vs **大于** vs **小于**：

```java
// 大于等于
if (nums[mid] >= target) // right
return left;

// 小于等于
if (nums[mid] <= target) // left
return right;            // 闭区间
return right - 1;        // 左闭右开

// 大于
if (nums[mid] > target)  // right
return left;

// 小于
if (nums[mid] < target)  // left
return right;            // 闭区间
return right - 1;        // 左闭右开
```

## 8.实战

### 二分查找

[704. 二分查找](https://leetcode.cn/problems/binary-search/)

```java
class Solution {
    public int search(int[] nums, int target) {
        return binarySearch(nums, target);
    }

    static int binarySearch(int[] nums, int target) {
        // 二分查找等于 target 的下标
        // 左闭右开 [left, right)
        int left = 0;
        int right = nums.length;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] == target)     return mid;
            else if (nums[mid] < target) left = mid + 1;
            else                         right = mid;
        }
        return -1;
    }
}
```

### 第一个错误的版本

[278. 第一个错误的版本](https://leetcode.cn/problems/first-bad-version/)

```java
public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        int l = 1;
        int r = n;
        while (l < r) {
            int m = l + (r - l) / 2;
            if (isBadVersion(m)) r = m;
            else l = m + 1;
        }
        return l;
    }
}
```

### 搜索插入位置

[35. 搜索插入位置](https://leetcode.cn/problems/search-insert-position/)

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        return ceiling(nums, target);
    }

    static int ceiling(int[] nums, int target) {
        // 二分查找第一个大于等于 target 的下标
        // 左闭右开 [left, right)
        int left = 0;
        int right = nums.length;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] >= target) right = mid;
            else                     left = mid + 1;
        }
        return left;
    }
}
```

### 🟥阶乘函数后 K 个零

[793. 阶乘函数后 K 个零](https://leetcode.cn/problems/preimage-size-of-factorial-zeroes-function/)

- 一个数字末尾 0 的数量就是其因子中 10 的数量，也就是 2 的数量和 5 的数量的更小值。
- x! 的因子中 5 的数量一定少于 2 的数量，所以 x! 的末尾有 k 个 0 即因子中 5 的数量为 k。
- 形如 f * 5 的数，每个数字在阶乘中贡献 1 个 5。
- 形如 f * 25 的数，每个数字在阶乘中额外贡献 1 个 5，共贡献了 2 个 5。
- 形如 f * 125 的数，每个数字在阶乘中额外贡献 1 个 5，共贡献了 3 个 5。
- ...
- 总结，形如 f * 5 ^ p 的数，每个数字在阶乘中共贡献了 p 个 5。
- 数 f * 5、f * 5 + 1、f * 5 + 2、f * 5 + 3、f * 5 + 4 中因子 5 的数量不变，所以若 k 合法，则返回 5，若 k 不合法，则返回 0。

```java
class Solution {
    public int preimageSizeFZF(int k) {
        int l = 0;
        int r = k;
        while (l <= r) {
            int m = l + (r - l) / 2;
            long x = m * 5L;
            int cnt = 0;
            while (x > 0) {
                cnt += x / 5;
                x /= 5;
            }
            if (cnt == k) return 5;
            else if (cnt > k) r = m - 1;
            else l = m + 1;
        }
        return 0;
    }
}
```

## 参考


