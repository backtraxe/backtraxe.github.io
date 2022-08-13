# 力扣 0004 寻找两个正序数组的中位数


[4. 寻找两个正序数组的中位数](https://leetcode.cn/problems/median-of-two-sorted-arrays/)

<!--more-->

## 二分查找

假设两个有序数组分别是 $A$ 和 $B$。要找到第 $k$ 小的元素，我们可以比较 $A[k/2−1]$ 和 $B[k/2−1]$。由于 $A[k/2−1]$ 和 $B[k/2−1]$ 的前面分别有 $k/2−1$ 个元素，对于 $A[k/2−1]$ 和 $B[k/2−1]$ 中的**较小值**，最多只会有 $k/2-1+k/2-1=k−2$ 个元素比它小，则该元素最多是第 $k-1$ 小的元素，此时可以舍弃该元素及其之前的元素，然后查找第 $k/2$ 小的元素。

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length;
        int n = nums2.length;
        double ans = findMedian(nums1, 0, m, nums2, 0, n, (m + n) / 2);
        if ((m + n) % 2 == 0)
            ans = (ans + findMedian(nums1, 0, m, nums2, 0, n, (m + n - 1) / 2)) / 2.0;
        return ans;
    }

    int findMedian(int[] nums1, int l1, int r1, int[] nums2, int l2, int r2, int k) {
        // 返回在 nums1[l1:r1] 和 nums2[l2:r2] 中第 k + 1 大的数
        int m = r1 - l1;
        int n = r2 - l2;
        if (m == 0) return nums2[l2 + k];
        if (n == 0) return nums1[l1 + k];
        if (k == 0) return Math.min(nums1[l1], nums2[l2]);
        int h1 = Math.min((k - 1) / 2, m - 1);
        int h2 = Math.min((k - 1) / 2, n - 1);
        if (nums1[l1 + h1] >= nums2[l2 + h2])
            return findMedian(nums1, l1, r1, nums2, l2 + h2 + 1, r2, k - h2 - 1);
        return findMedian(nums1, l1 + h1 + 1, r1, nums2, l2, r2, k - h1 - 1);
    }
}
```

- 时间复杂度：$ O(\log(m+n)) $
- 空间复杂度：$ O(1) $

## 双指针

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length;
        int n = nums2.length;
        int pre = 0;
        int cur = 0;
        for (int i = 0, j = 0; i < m || j < n;) {
            int a = i < m ? nums1[i] : Integer.MAX_VALUE;
            int b = j < n ? nums2[j] : Integer.MAX_VALUE;
            if (a < b) {
                cur = a;
                i++;
            } else {
                cur = b;
                j++;
            }
            if ((i + j) == (m + n) / 2 + 1) break; // 找到中点
            pre = cur;
        }
        return (m + n) % 2 == 0 ? (pre + cur) / 2.0 : cur * 1.0;
    }
}
```

- 时间复杂度：$ O(m+n) $
- 空间复杂度：$ O(1) $

## 排序

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length;
        int n = nums2.length;
        int[] arr = Arrays.copyOf(nums1, m + n);
        for (int i = 0; i < n; i++) arr[m + i] = nums2[i];
        Arrays.sort(arr);
        int pre = arr[(m + n - 1) / 2];
        int cur = arr[(m + n) / 2];
        return (m + n) % 2 == 0 ? (pre + cur) / 2.0 : cur * 1.0;
    }
}
```

- 时间复杂度：$ O((m+n) \log (m+n)) $
- 空间复杂度：$ O(m+n) $

