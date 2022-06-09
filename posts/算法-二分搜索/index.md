# 算法-二分搜索


<!--more-->

## 1.基础

- 要求序列**非递减**，即`nums[i - 1] <= nums[i]`
- 时间复杂度：$O(\log n)$

<br />

## 2.等于指定值的下标

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

```java {hl_lines=[5, 6, 10]}
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

```java {hl_lines=[5, 6, 10]}
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

<br />

## 3.第一个大于指定值的下标

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

```java {hl_lines=[5, 6, 8, 9]}
static int higher(int[] nums, int target) {
    // 二分查找第一个大于 target 的下标
    // 闭区间 [left, right]
    int left = 0;
    int right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] <= target) left = mid + 1;
        else                     right = mid - 1;
    }
    return left;
}
```

- **左闭右开写法：**

```java {hl_lines=[5, 6, 8, 9]}
static int higher(int[] nums, int target) {
    // 二分查找第一个大于 target 的下标
    // 左闭右开 [left, right)
    int left = 0;
    int right = nums.length;
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] <= target) left = mid + 1;
        else                     right = mid;
    }
    return left;
}
```

<br />

## 4.第一个大于等于指定值的下标

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

```java {hl_lines=[5, 6, 8, 9]}
static int ceiling(int[] nums, int target) {
    // 二分查找第一个大于等于 target 的下标
    // 闭区间 [left, right]
    int left = 0;
    int right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) left = mid + 1;
        else                    right = mid - 1;
    }
    return left;
}
```

- **左闭右开写法：**

```java {hl_lines=[5, 6, 8, 9]}
static int ceiling(int[] nums, int target) {
    // 二分查找第一个大于等于 target 的下标
    // 左闭右开 [left, right)
    int left = 0;
    int right = nums.length;
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) left = mid + 1;
        else                    right = mid;
    }
    return left;
}
```

<br />

## 5.最后一个小于指定值的下标

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

```java {hl_lines=[5, 6, 8, 9, 11]}
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

```java {hl_lines=[5, 6, 8, 9, 11]}
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

<br />

## 6.最后一个小于等于指定值的下标

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

```java {hl_lines=[5, 6, 8, 9, 11]}
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

```java {hl_lines=[5, 6, 8, 9, 11]}
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
if (nums[mid] < target)
return left;

// 小于等于
if (nums[mid] > target)

// 大于
if (nums[mid] <= target)
return left;

 // 小于
if (nums[mid] >= target)
return right; // 闭区间
return right - 1; // 左闭右开
```

