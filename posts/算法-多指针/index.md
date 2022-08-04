# 算法-多指针


<!--more-->

## 1.首尾指针

- 要求数组有序。

**两数之和：**

[167. 两数之和 II - 输入有序数组](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/)

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int l = 0;
        int r = numbers.length - 1;
        while (l < r) {
            int sum = numbers[l] + numbers[r];
            if (sum > target) r--;
            else if (sum < target) l++;
            else return new int[] { l + 1, r + 1 };
        }
        return null;
    }
}
```

## 2.快慢指针

**检测链表中的环：**

[]()

## 3.二路归并

```java

```

## 3.三指针

**荷兰国旗问题：**

[75. 颜色分类](https://leetcode.cn/problems/sort-colors/)

```java
class Solution {
    public void sortColors(int[] nums) {
        int l = 0;
        int m = 0;
        int r = nums.length - 1;
        while (m <= r) {
            if (nums[m] == 0) {
                swap(nums, l, m);
                l++;
                m++;
            } else if (nums[m] == 1) {
                m++;
            } else if (nums[m] == 2) {
                swap(nums, m, r);
                r--;
            }
        }
    }

    static void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

**三向切分快排：**

```java
static void quickSort(int[] arr, int low, int high) {
    // [low, high]
    if (low >= high) return;
    // 1. 随机选取 pivot
    int pivotIndex = low + (int) (Math.random() * (high - low + 1));
    // 2. partition
    int pivot = arr[pivotIndex];
    int l = low;
    int m = low;
    int r = high;
    while (m <= r) {
        if (arr[m] < pivot) {
            swap(arr, l, m);
            l++;
            m++;
        } else if (arr[m] > pivot) {
            swap(arr, m, r);
            r--;
        } else {
            m++;
        }
    }
    // 3. 递归子数组
    quickSort(arr, low, l - 1);
    quickSort(arr, r + 1, high);
}

static void swap(int[] arr, int i, int j) {
    int temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
}
```

## 4. k路归并


