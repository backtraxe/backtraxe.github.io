# 力扣 0004 寻找两个正序数组的中位数


[4. 寻找两个正序数组的中位数](https://leetcode.cn/problems/median-of-two-sorted-arrays/)

<!--more-->

## 划分数组

中位数的意义是将一些数据划分称为数量相等或数量差不超过 1 的两部分。其中前一部分的最大值小于等于后一部分的最小值。

1. 已知 A 的长度为 $m$，B 的长度为 $n$，假设 $m<n$，若不满足则交换 A 和 B。

2. 将 A 在任意位置划分成为两部分。其中 $i \in [0, m]$。

$$
A[0], A[1], \cdots, A[i-1] \quad | \quad A[i], A[i+1], \cdots, A[m-1]
$$

3. 同样，将 B 也划分为两个部分，因为 A 的左半部分加上 B 的左半部分的数量为 $\frac{m+n}{2}$，则 $j=\frac{m+n}{2}-i, \quad j \in [\frac{n-m}{2}, \frac{m+n}{2}]$。

$$
B[0], B[1], \cdots, B[j-1] \quad | \quad B[j], B[j+1], \cdots, B[n-1]
$$

4. 为了保证两部分元素数量相等，所以 $i$ 和 $j$ 的移动方向相反。

5. 前一部分的最大值为 $maxL=max(A[i-1], B[j-1])$，而后一部分的最小值为 $minR=min(A[i], B[j])$，满足 $maxL \le minR$ 即可。

6. 若 $maxL > minR$，我们要减小 $maxL$，增大 $minR$。已知 $A[i-1] \le A[i]$ 和 $B[j-1] \le B[j]$，则
    - $A[i-1] \le A[i] \lt B[j-1] \le B[j]$，此时因为 $A[i] \lt B[j-1]$，所以我们应该将 $i$ 向右移。
    - $B[j-1] \le B[j] \lt A[i-1] \le A[i]$，此时因为 $B[j] \lt A[i-1]$，所以我们应该将 $i$ 向左移。

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length;
        int n = nums2.length;
        if (m == 0) return (nums2[n / 2] + nums2[(n - 1) / 2]) / 2.0;
        if (n == 0) return (nums1[m / 2] + nums1[(m - 1) / 2]) / 2.0;
        if (m > n) return findMedianSortedArrays(nums2, nums1);
        int l = 0;
        int r = m;
        int maxL = 0;
        int minR = 0;
        while (l <= r) {
            int i = l + (r - l) / 2; // 将 nums1 划分为 [0:i] [i:m] 两部分
            int j = (m + n) / 2 - i; // 将 nums2 划分为 [0:j] [j:n] 两部分
            maxL = Math.max(
                i >= 1 ? nums1[i - 1] : Integer.MIN_VALUE,
                j >= 1 ? nums2[j - 1] : Integer.MIN_VALUE
            );
            minR = Math.min(
                i < m ? nums1[i] : Integer.MAX_VALUE,
                j < n ? nums2[j] : Integer.MAX_VALUE
            );
            if (maxL <= minR) break; // 完成划分
            else if (i >= 1 && nums1[i - 1] > nums2[j]) r = i;
            else l = i + 1;
        }
        return (m + n) % 2 == 0 ? (maxL + minR) / 2.0 : minR * 1.0;
    }
}
```

- 时间复杂度：$ O(\log(\min(m, n))) $
- 空间复杂度：$ O(1) $

## 二分查找

假设两个有序数组分别是 $A$ 和 $B$。要找到第 $k$ 小的元素，我们可以比较 $A[\frac{k}{2}−1]$ 和 $B[\frac{k}{2}−1]$。由于 $A[\frac{k}{2}−1]$ 和 $B[\frac{k}{2}−1]$ 的前面分别有 $\frac{k}{2}−1$ 个元素，对于 $A[\frac{k}{2}−1]$ 和 $B[\frac{k}{2}−1]$ 中的**较小值**，最多只会有 $\frac{k}{2}-1+\frac{k}{2}-1=k−2$ 个元素比它小，则该元素最多是第 $k-1$ 小的元素，此时可以舍弃该元素及其之前的元素，然后查找第 $\frac{k}{2}$ 小的元素。

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

