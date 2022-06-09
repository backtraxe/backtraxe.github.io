# 算法-二分搜索


<!--more-->

#### 闭区间的二分查找

```java
int binarySearch(int[] nums, int target) {
    // [left, right]
    int left = 0;
    int right = nums.length - 1;

    while(left <= right) {
        // 防止整数溢出
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            return mid;
        } else if (nums[mid] < target) {
            // 右侧
            left = mid + 1;
        } else {
            // 左侧
            right = mid - 1;
        }
    }
    return -1;
}
```

#### 左闭右开区间的二分查找

```java
int binarySearch(int[] nums, int target) {
    // [left, right)
    int left = 0;
    int right = nums.length;

    while(left < right) {
        // 防止整数溢出
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            return mid;
        } else if (nums[mid] < target) {
            // 右侧
            left = mid + 1;
        } else {
            // 左侧
            right = mid;
        }
    }
    return -1;
}
```

#### 左侧边界的二分查找 lower_bound

```java
// 左侧边界
// 寻找第一个大于等于 target 的元素位置
int binarySearch(int[] nums, int target) {
    // [left, right)
    int left = 0;
    int right = nums.length;

    while(left < right) {
        // 防止整数溢出
        int mid = left + (right - left) / 2;
        if (nums[mid] >= target) {
            // 左侧、中间
            right = mid;
        } else {
            // 右侧
            left = mid + 1;
        }
    }
    return left;
    // left 范围为 [0, nums.length]
    // 当 left == nums.length
    // 或者 nums[left] != target
    // 说明 nums 中无 target
}
```

#### 右侧边界的二分查找 upper_bound

```java
// 右侧边界
// 寻找第一个大于 target 的元素位置
int binarySearch(int[] nums, int target) {
    // [left, right)
    int left = 0;
    int right = nums.length;

    while(left < right) {
        // 防止整数溢出
        int mid = left + (right - left) / 2;
        if (nums[mid] > target) {
            // 左侧
            right = mid;
        } else {
            // 中间、右侧
            left = mid + 1;
        }
    }
    return left;
    // left 范围为 [0, nums.length]
    // 当 left == 0
    // 或者 nums[left - 1] != target
    // 说明 nums 中无 target
}
```

<br>

<br>

<br>

<br>

<br>

## 2.查找第一个大于指定元素的下标

**测试结果：**

```java
int[] nums = { 4, 5, 5, 6, 7 };
higher(nums, 3); // 0
higher(nums, 4); // 1
higher(nums, 5); // 3
higher(nums, 6); // 4
higher(nums, 7); // 5
higher(nums, 8); // 5
```

**闭区间写法：**

```java
static int higher(int[] nums, int target) {
    // 二分查找第一个大于 target 的下标
    // 闭区间 [left, right]
    int left = 0;
    int right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] <= target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return left;
}
```

**左闭右开区间写法：**

```java
static int higher(int[] nums, int target) {
    // 二分查找第一个大于 target 的下标
    // 左闭右开 [left, right)
    int left = 0;
    int right = nums.length;
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] <= target) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    return left;
}
```

<br>

<br>

<br>

<br>

<br>

## 3.查找第一个大于等于指定元素的下标

**测试结果：**

```java
int[] nums = { 4, 5, 5, 6, 7 };
ceiling(nums, 3); // 0
ceiling(nums, 4); // 0
ceiling(nums, 5); // 1
ceiling(nums, 6); // 3
ceiling(nums, 7); // 4
ceiling(nums, 8); // 5
```

**闭区间写法：**

```java
static int ceiling(int[] nums, int target) {
    // 二分查找第一个大于等于 target 的下标
    // 闭区间 [left, right]
    int left = 0;
    int right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return left;
}
```

**左闭右开区间写法：**

```java
static int ceiling(int[] nums, int target) {
    // 二分查找第一个大于等于 target 的下标
    // 左闭右开 [left, right)
    int left = 0;
    int right = nums.length;
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    return left;
}
```

