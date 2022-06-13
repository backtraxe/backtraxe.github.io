# 算法-排序


<!--more-->

## 1.基础

### 1.1 术语介绍

- **稳定排序**：相同大小的元素在排序前后保持相对顺序不变。
- **不稳定排序**：相同大小的元素在排序前后的相对顺序发生变化。
- **内部排序**：在内存中进行的排序。
- **外部排序**：数据量太大不能全部读入内存，需要通过内存和磁盘结合进行的排序。

<br />

### 1.2 数组中元素交换的方法

```java
static void swap(int[] nums, int i, int j) {
    int temp = nums[i];
    nums[i] = nums[j];
    nums[j] = temp;
}
```

```java
static void swap(int[] nums, int i, int j) {
    if (i == j) return;
    nums[i] = nums[i] ^ nums[j];
    nums[j] = nums[i] ^ nums[j];
    nums[i] = nums[i] ^ nums[j];
}
```
---

## 2.冒泡排序

### 2.1 原理

1. 将整个数组划分为两个区域：未排序区、已排序区。数组左侧为未排序区，右侧为已排序区。
2. 从左往右进行排序，当进行第一轮排序时，未排序区大小为 n，已排序区大小为 0。
3. 每次比较相邻的两个数，若左侧的数字大于右侧的数字，则交换这两个数字。
4. 当比较到未排序区的末尾时，未排序区的最后一个数字即为未排序区的最大数字，将其放入已排序区，则未排序区大小减 1，已排序区大小加 1，此时完成一轮排序。
5. 当进行了 n - 1 轮排序后，未排序区的大小减为 1 时，排序结束。

<br />

### 2.2 优化

- **限制区域（默认）**：每一轮只用比较未排序区的元素。当进行了 i 轮排序后，已排序区的大小为 i，未排序区的大小为 n - i。
- **提前结束**：当某一轮未发生交换时，说明排序已经完成，可以提前结束。可设置一个布尔值记录一轮排序是否有发生交换。
- **冒泡界优化**：若当前轮使多个元素有序，则下一轮只需比较之前的元素。

<br />

### 2.3 代码

```java
// 1.未优化
static void bubbleSort(int[] nums) {
    int n = nums.length;
    for (int epoch = 1; epoch < n; epoch++) {
        for (int i = 0; i < n - epoch; i++) {
            if (nums[i] > nums[i + 1]) {
                swap(nums, i, i + 1);
            }
        }
    }
}
```

```java
// 2.提前结束优化
static void bubbleSort(int[] nums) {
    int n = nums.length;
    for (int epoch = 1; epoch < n; epoch++) {
        boolean swapped = false;
        for (int i = 0; i < n - epoch; i++) {
            if (nums[i] > nums[i + 1]) {
                swap(nums, i, i + 1);
                swapped = true;
            }
        }
        if (!swapped) break;
    }
}
```

```java
// 3.冒泡界优化
static void bubbleSort(int[] nums) {
    int n = nums.length;
    int firstSortedIndex = n - 1;
    for (int epoch = 1; epoch < n; epoch++) {
        boolean swapped = false;
        int lastSwappedIndex;
        for (int i = 0; i < firstSortedIndex; i++) {
            if (nums[i] > nums[i + 1]) {
                swap(nums, i, i + 1);
                swapped = true;
                lastSwappedIndex = i;
            }
        }
        if (!swapped) break;
        firstSortedIndex = lastSwappedIndex;
    }
}
```

<br />

### 2.4 分析

- 时间复杂度：$ O(n^2) $

$$
\sum_{epoch=1}^{n-1}\sum_{i=0}^{n-epoch-1}1=\sum_{epoch=1}^{n-1}(n-epoch-1)=(n-2)+(n-3)+\cdots+0=\frac{(n-1)(n-2)}{2}
$$

- 空间复杂度：$ O(1) $
- 稳定性：稳定

<br />

## 3.选择排序

### 3.1 原理

<br />

### 3.2 优化

<br />

### 3.3 代码

```java
void selectionSort(int[] nums) {
    int len = nums.length;
    // 循环不变量：[0, i) 有序，且该区间里所有元素就是最终排定的样子
    for (int i = 0; i < len - 1; i++) {
        // 选择区间 [i, len - 1] 里最小的元素的索引，交换到下标 i
        int minIdx = i;
        for (int j = i + 1; j < len; j++) {
            if (nums[j] < nums[minIdx]) {
                minIdx = j;
            }
        }
        swap(nums, i, minIdx);
    }
    return nums;
}
```

<br />

### 3.4 分析

- 时间复杂度：$ O(n^2) $

$$
\sum_{epoch=1}^{n-1}\sum_{i=0}^{n-epoch-1}1=\sum_{epoch=1}^{n-1}(n-epoch-1)=(n-2)+(n-3)+\cdots+0=\frac{(n-1)(n-2)}{2}
$$

- 空间复杂度：$ O(1) $
- 稳定性：不稳定

<br />

## 4.插入排序

```cpp
void insertionSort(vector<int>& arr) {
    for (int i = 1; i < arr.size(); i++) {
        int temp = arr[i], j;
        for (j = i - 1; j >= 0 && arr[j] > arr[i]; j--) {
            arr[j + 1] = arr[j];
        }
        arr[j + 1] = temp;
    }
}
```

- 时间复杂度：$ O(n^2) $
- 空间复杂度：$ O(1) $

特点：

- 稳定

<br />

## 5.希尔排序

<br />

## 6.归并排序

#### 10.4.1 自顶向下

```java
static int[] copy;

static void mergeSort(int[] arr) {
    copy = new int[arr.length];
    mergeSort(arr, 0, arr.length - 1);
}

static void mergeSort(int[] arr, int low, int high) {
    if (low >= high) return;
    int mid = low + (high - low) / 2;
    mergeSort(arr, low, mid);
    mergeSort(arr, mid + 1, high);
    merge(arr, low, mid, high);
}

static void merge(int[] arr, int low, int mid, int high) {

}
```

#### 10.4.2 自底向上

```java

```

## 7.快速排序

- 在数组中随机取出一个数，称之为基数（pivot）。
- 遍历数组，将比基数大的数字放到它的右边，比基数小的数字放到它的左边。遍历完成后，数组被分成了左右两个区域。
- 将左右两个区域视为两个数组，重复前两个步骤，直到排序完成。

#### 10.5.1 二切分

```java
static void quickSort(int[] arr) {
    quickSort(arr, 0, arr.length - 1);
}

static void quickSort(int[] arr, int low, int high) {
    if (low >= high) return;
    int pos = partition(arr, low, high);
    quickSort(arr, low, pos - 1);
    quickSort(arr, pos + 1, high);
}

static int partition(int[] arr, int low, int high) {
    int pivotID = (int) (Math.random() * (high - low + 1)) + low;
    swap(arr, low, pivotID);
    int pivot = arr[low];
    while (low < high) {
        while (low < high && arr[high] > pivot) high--;
        while (low < high && arr[low] <= pivot) low++;
        if (low < high) swap(arr, low, high);
    }
    arr[low] = pivot;
    return low;
}

static void swap(int[] arr, int i, int j) {
    int temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
}
```

#### 10.5.2 三切分

```java
static void quickSort(int[] arr) {
    quickSort(arr, 0, arr.length - 1);
}

static void quickSort(int[] arr, int low, int high) {
    if (low >= high) return;
    int[] pos = partition(arr, low, high);
    quickSort(arr, low, pos[0] - 1);
    quickSort(arr, pos[1] + 1, high);
}

static int[] partition(int[] arr, int low, int high) {
    int pivotID = (int) (Math.random() * (high - low + 1)) + low;
    swap(arr, low, pivotID);
    int pivot = arr[low];
    int mid = low + 1;
    while (mid <= high) {
        if (arr[mid] == pivot) {
            mid++;
        } else if (arr[mid] > pivot) {
            swap(arr, mid, high);
            high--;
        } else {
            swap(arr, low, mid);
            low++;
            mid++;
        }
    }
    return new int[] { low, high };
}
```

<br />

## 8.堆排序

<br />

## 9.计数排序

<br />

## 10.基数排序

<br />

## 11.桶排序

<br />

## 12.总结

| 排序算法 | 时间复杂度 | 稳定性 |
|:---:|:---:|:---:|
| 冒泡排序 | $O\(n^2\)$ | 稳定 |
| 选择排序 | $O\(n^2\)$ | 不稳定 |
| 插入排序 | $O\(n^2\)$ | 稳定 |
| 快速排序 | $O\(n \\log n\)$ | 不稳定 |
| 归并排序 | $O\(n \\log n\)$ | 稳定 |
| 堆排序 | $O\(n \\log n\)$ | 不稳定 |
| 计数排序 | $O\(n\)$ | 稳定 |
| 基数排序 | $O\(n\)$ | 稳定 |

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=AM_HTMLorMML-full"></script>
<table style="text-align:center">
    <tr>
    	<th rowspan='2'>名称</th>
    	<th rowspan='2'>数据对象</th>
    	<th rowspan='2'>稳定性</th>
    	<th colspan='2'>时间复杂度</th>
    	<th rowspan='2'>额外空间复杂度</th>
    	<th rowspan='2'>描述</th>
    </tr>
    <tr>
    	<th>平均</th>
    	<th>最坏</th>
    </tr>
    <tr>
    	<td>冒泡排序</td>
    	<td>数组</td>
    	<td>是</td>
    	<td colspan='2'>$ O(n^2) $</td>
    	<td>$ O(1) $</td>
    	<td>(无序区，有序区)<br/>从无序区通过交换找出最大元素放到有序区前端。</td>
    </tr>
    <tr>
    	<td rowspan='2'>选择排序</td>
    	<td>数组</td>
    	<td>否</td>
    	<td rowspan='2' colspan='2'>$ O(n^2) $</td>
    	<td rowspan='2'>$ O(1) $</td>
    	<td rowspan='2'>(有序区，无序区)<br/>在无序区里找一个最小的元素放到有序区后端。<br/>对数组：比较多，交换少</td>
    </tr>
    <tr>
    	<td>链表</td>
    	<td>是</td>
    </tr>
    <tr>
    	<td>插入排序</td>
    	<td>数组、链表</td>
        <td>是</td>
    	<td colspan='2'>$ O(n^2) $</td>
    	<td>$ O(1) $</td>
    	<td>(有序区，无序区)<br/>把无序区的第一个元素插入到有序区的合适位置。<br/>对数组：比较少，交换多</td>
    </tr>
    <tr>
    	<td>堆排序</td>
    	<td>数组</td>
        <td>否</td>
    	<td colspan='2'>$ O(n \log{n}) $</td>
    	<td>$ O(1) $</td>
    	<td>(最大堆，有序区)<br/>从堆顶把最大值弹出到有序区前端，然后调整堆。</td>
    </tr>
    <tr>
    	<td rowspan='3'>归并排序</td>
    	<td rowspan='2'>数组</td>
        <td rowspan='3'>是</td>
        <td colspan='2'>$ O(n \log{\log{n}}) $</td>
        <td>$ O(1) $</td>
        <td rowspan='3'>将数据分为两段，再从两段中逐个选最小的元素移入新数据段的末尾。<br/>可自上而下，也可自下而上</td>
    </tr>
    <tr>
        <td rowspan='2' colspan='2'>$ O(n \log{n}) $</td>
    	<td>自上而下：$ O(n)+O(\log{n}) $</td>
    </tr>
    <tr>
        <td>链表</td>
    	<td>自下而上：$ O(1) $</td>
    </tr>
    <tr>
    	<td>快速排序</td>
    	<td>数组</td>
        <td>否</td>
    	<td>$ O(n \log{n}) $</td>
        <td>$ O(n^2) $</td>
    	<td>$ O(\log{n}) $</td>
    	<td>(小数区，基准元素，大数区)<br/>在区间中随机挑选一个元素作为基准元素，将小于该基准的元素放到基准之前，大于的放到基准之后，然后递归地对小数区和大数区进行快速排序。</td>
    </tr>
    <tr>
    	<td>希尔排序</td>
    	<td>数组</td>
        <td>否</td>
    	<td>$ O(n \log{\log{n}}) $</td>
    	<td>$ O(n^2) $</td>
        <td>$ O(1) $</td>
    	<td>按从大到小的间距进行多次插入排序，最后一次的间距为1。</td>
    </tr>
    <tr>
    	<td>计数排序</td>
    	<td>数组、链表</td>
        <td>是</td>
    	<td colspan='2'>$ O(n+m) $</td>
    	<td>$ O(n+m) $</td>
    	<td>统计小于等于该元素值的元素的个数i，然后将该元素放在目标数组的第i个位置。</td>
    </tr>
    <tr>
    	<td>桶排序</td>
    	<td>数组、链表</td>
        <td>是</td>
    	<td colspan='2'>$ O(n) $</td>
    	<td>$ O(m) $</td>
    	<td>将值为i的元素放入第i号桶，然后依次把桶里的元素倒出来。</td>
    </tr>
    <tr>
    	<td>基数排序</td>
    	<td>数组、链表</td>
        <td>是</td>
    	<td>$ O(k \times n) $</td>
    	<td>$ O(n^2) $</td>
        <td></td>
    	<td>一种多关键字的排序算法，可用桶排序实现。</td>
    </tr>
</table>

## 13.实战

## 参考

1. [当我谈排序时，我在谈些什么🤔](https://leetcode-cn.com/problems/sort-an-array/solution/dang-wo-tan-pai-xu-shi-wo-zai-tan-xie-shi-yao-by-s/)
1. [复习基础排序算法（Java） - 排序数组 - 力扣（LeetCode）](https://leetcode-cn.com/problems/sort-an-array/solution/fu-xi-ji-chu-pai-xu-suan-fa-java-by-liweiwei1419/)
1. [十大排序从入门到入赘 - 力扣（LeetCode）](https://leetcode.cn/circle/discuss/eBo9UB/)

