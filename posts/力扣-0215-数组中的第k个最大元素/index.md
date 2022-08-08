# 力扣 0215 数组中的第K个最大元素


[215. 数组中的第K个最大元素](https://leetcode.cn/problems/kth-largest-element-in-an-array/)

<!--more-->

## 1.快速选择

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        int n = nums.length;
        return partition(nums, n - k, 0, n - 1);
    }

    static int partition(int[] nums, int k, int left, int right) {
        // [left, right] 返回 nums 按升序排序后索引 k 位置上的元素，即第 k + 1 小的元素
        if (left == right) return nums[left];
        int p = left + (int) (Math.random() * (right - left + 1));
        int pivot = nums[p];
        nums[p] = nums[left];
        nums[left] = pivot;
        int l = left;
        int r = right;
        while (l < r) {
            while (l < r && nums[r] >= pivot) r--;
            nums[l] = nums[r];
            while (l < r && nums[l] <= pivot) l++;
            nums[r] = nums[l];
        }
        nums[l] = pivot;
        if (l == k) return pivot;
        else if (l > k) return partition(nums, k, left, l - 1);
        else return partition(nums, k, l + 1, right);
    }
}
```

时间复杂度：$ O(n) $
空间复杂度：$ O(1) $

## 2.手动堆排序

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        int heapSize = nums.length;
        buildMaxHeap(nums, heapSize);
        for (int i = nums.length - 1; i >= nums.length - k + 1; --i) {
            swap(nums, 0, i);
            --heapSize;
            maxHeapify(nums, 0, heapSize);
        }
        return nums[0];
    }

    public void buildMaxHeap(int[] a, int heapSize) {
        for (int i = heapSize / 2; i >= 0; --i) {
            maxHeapify(a, i, heapSize);
        }
    }

    public void maxHeapify(int[] a, int i, int heapSize) {
        int l = i * 2 + 1, r = i * 2 + 2, largest = i;
        if (l < heapSize && a[l] > a[largest]) {
            largest = l;
        }
        if (r < heapSize && a[r] > a[largest]) {
            largest = r;
        }
        if (largest != i) {
            swap(a, i, largest);
            maxHeapify(a, largest, heapSize);
        }
    }

    public void swap(int[] a, int i, int j) {
        int temp = a[i];
        a[i] = a[j];
        a[j] = temp;
    }
}
```

时间复杂度：$ O(n \log n) $
空间复杂度：$ O(\log n) $

## 3.API堆排序

```java

```

时间复杂度：$ O(n \log n) $
空间复杂度：$ O(n) $

