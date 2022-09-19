# 算法-排序


<!--more-->

## 1.基础

### 1.1 术语介绍

- **稳定排序**：相同大小的元素在排序前后保持相对顺序不变。
- **不稳定排序**：相同大小的元素在排序前后的相对顺序发生变化。
- **内部排序**：在内存中进行的排序。
- **外部排序**：数据量太大不能全部读入内存，需要通过内存和磁盘结合进行的排序。

### 1.2 数组中元素交换的方法

```java
static void swap(int[] nums, int i, int j) {
    int temp = nums[i];
    nums[i] = nums[j];
    nums[j] = temp;
}
```

## 2.冒泡排序

### 2.1 原理

1. 将整个数组划分为两个区域：未排序区、已排序区。数组左侧为未排序区，右侧为已排序区。
2. 从左往右进行排序，当进行第一轮排序时，未排序区大小为 n，已排序区大小为 0。
3. 每次比较相邻的两个数，若左侧的数字大于右侧的数字，则交换这两个数字。
4. 当比较到未排序区的末尾时，未排序区的最后一个数字即为未排序区的最大数字，将其放入已排序区，则未排序区大小减 1，已排序区大小加 1，此时完成一轮排序。
5. 当进行了 n - 1 轮排序后，未排序区的大小减为 1 时，排序结束。

### 2.2 优化

- **限制区域（默认）**：每一轮只用比较未排序区的元素。当进行了 i 轮排序后，已排序区的大小为 i，未排序区的大小为 n - i。
- **提前结束**：当某一轮未发生交换时，说明排序已经完成，可以提前结束。可设置一个布尔值记录一轮排序是否有发生交换。
- **冒泡界优化**：若当前轮使多个元素有序，则下一轮只需比较之前的元素。

### 2.3 代码

```java
// 1.未优化
static void bubbleSort(int[] nums) {
    int n = nums.length;
    for (int epoch = 1; epoch < n; epoch++)
        for (int i = 0; i < n - epoch; i++)
            if (nums[i] > nums[i + 1])
                swap(nums, i, i + 1);
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

### 2.4 分析

- 时间复杂度：$ O(n^2) $

$$
\sum_{epoch=1}^{n-1}\sum_{i=0}^{n-epoch-1}1=\sum_{epoch=1}^{n-1}(n-epoch-1)=(n-2)+(n-3)+\cdots+0=\frac{(n-1)(n-2)}{2}
$$

- 空间复杂度：$ O(1) $
- 稳定性：稳定

## 3.选择排序

### 3.1 介绍

将数组分为排序区（当前索引以前部分）和未排序区（当前索引及以后部分），每次将未排序区中的最小元素和未排序区的第一个元素进行交换，然后将该元素加入排序区。

### 3.2 特点

- 时间复杂度：$ O(n ^ 2) $
- 空间复杂度：$ O(1) $
- 非稳定排序
- 原地排序
- 每一轮排序至少确定一个元素的位置

### 3.3 步骤

1. 将数组分为排序区（当前索引以前部分）和未排序区（当前索引及以后部分），每次将未排序区中的最小元素和未排序区的第一个元素进行交换，然后将该元素加入排序区。
2. 重复步骤 1，直到未排序区没有元素。

### 3.4 代码

```java
static void selectionSort(int[] arr) {
    int n = arr.length;
    // 排序区：[0, i)，未排序区：[i, n)
    for (int i = 0; i < n; i++) {
        int minIdx = i;
        for (int j = i + 1; j < n; j++)
            if (arr[j] < arr[minIdx]) minIdx = j;
        int temp = arr[i];
        arr[i] = arr[minIdx];
        arr[minIdx] = temp;
    }
}
```

## 4.插入排序

### 4.1 介绍

将数组分为排序区（当前索引以前部分）和未排序区（当前索引及以后部分），每次将未排序区中的第一个元素插入到排序区中的对应位置。

### 4.2 特点

- 时间复杂度：$ O(n ^ 2) $
- 空间复杂度：$ O(1) $
- 稳定排序
- 原地排序

### 4.3 步骤

1. 将数组分为排序区（当前索引以前部分）和未排序区（当前索引及以后部分）。
2. 若当前索引元素比前一个元素小，则将前一个元素后移一位。
3. 重复步骤 2，直到没有上一个元素或者上一个元素小于等于当前元素为止，在当前位置插入该元素。
4. 重复步骤 2 和 3，直到未排序区没有元素。

### 4.4 代码

```java
static void insertionSort(int[] arr) {
    int n = arr.length;
    for (int i = 1; i < n; i++) {
        int temp = arr[i];
        int j = i - 1;
        for (; j >= 0 && arr[j] > temp; j--)
            arr[j + 1] = arr[j];
        arr[j + 1] = temp;
    }
}
```

## 5.希尔排序

### 5.1 介绍

改进插入排序，将插入排序中相邻元素比较改为间隔为 h 的元素进行比较，使数组中任意间隔为 h 的元素都有序。只要最后按 h = 1 进行插入排序，就能将数组排序。对于每个 h，使用插入排序独立进行排序。

一般取 h = 1 / 2 * (3 * k - 1)，称为**递增序列**。

### 5.2 特点

- 时间复杂度：$ O(n ^ {\frac{3}{2}}) $
- 空间复杂度：$ O(1) $
- 稳定排序
- 原地排序

### 5.3 代码

```java
static void shellSort(int[] arr) {
    int n = arr.length;
    int h = 1;
    while (h < n / 3) h = 3 * h - 1; // 1, 4, 13, 40, 121, ...
    while (h >= 1) {
        for (int i = h; i < n; i++) {
            int temp = arr[i];
            int j = i - h;
            for (; j >= 0 && arr[j] > temp; j -= h) // 间隔为 h 的插入排序
                arr[j + h] = arr[j];
            arr[j + h] = temp;
        }
        h /= 3;
    }
}
```

## 6.归并排序

### 6.1 介绍

使用分治的思想将数组划分为大小相等的两部分，然后分别对这两部分进行排序，再将有序的这两部分归并成大的有序数组。

### 6.2 特点

- 时间复杂度：$ O(n \log n) $
- 空间复杂度：$ O(n) $，需要一个辅助数组帮助进行归并。
- 稳定排序
- 非原地排序

### 6.3 步骤

1. 将数组按中间位置分为前后两个部分。
2. 递归对前后两部分进行排序。
3. 将有序的前后两部分归并为有序数组。

### 6.1 自顶向下

```java
static void mergeSort(int[] arr) {
    int[] copy = new int[arr.length];
    mergeSort(arr, 0, arr.length, copy);
}

static void mergeSort(int[] arr, int low, int high, int[] copy) {
    // [low, high)
    if (low + 1 >= high) return;
    int mid = low + (high - low) / 2;
    mergeSort(arr, low, mid, copy);
    mergeSort(arr, mid, high, copy);
    merge(arr, low, mid, high, copy);
}

static void merge(int[] arr, int low, int mid, int high, int[] copy) {
    // 将 [low, mid) 和 [mid, high) 归并
    int i = low;
    int j = mid;
    for (int k = low; k < high; k++) copy[k] = arr[k];
    for (int k = low; k < high; k++) {
        if (i >= mid)               arr[k] = copy[j++];
        else if (j >= high)         arr[k] = copy[i++];
        else if (copy[i] < copy[j]) arr[k] = copy[i++];
        else                        arr[k] = copy[j++];
    }
}
```

### 6.2 自底向上

```java
static void mergeSort(int[] arr) {
    int n = arr.length;
    int[] copy = new int[n];
    for (int sz = 1; sz < n; sz *= 2) // [i, i + sz) [i + sz, i + sz * 2)
        for (int i = 0; i + sz <= n; i += sz * 2)
            merge(arr, i, i + sz, Math.min(i + sz * 2, n), copy);
}

static void merge(int[] arr, int low, int mid, int high, int[] copy) {
    // 将 [low, mid) 和 [mid, high) 归并
    int i = low;
    int j = mid;
    for (int k = low; k < high; k++) copy[k] = arr[k];
    for (int k = low; k < high; k++) {
        if (i >= mid)               arr[k] = copy[j++];
        else if (j >= high)         arr[k] = copy[i++];
        else if (copy[i] < copy[j]) arr[k] = copy[i++];
        else                        arr[k] = copy[j++];
    }
}
```

## 7.快速排序

### 7.1 介绍

与归并排序一样，快速排序也是一种利用 **分治思想** 的排序方法，确定 **主轴及分区** 是快速排序的核心操作。

### 7.2 特点

- 时间复杂度：平均/最优为 $O(n \log n)$，最坏为 $O(n^2)$。
- 实现简单
- **原地排序**
- 每一轮确定一个元素的位置
- **非稳定排序**

### 7.3 步骤

1. 在数组中随机取出一个数，称之为基数（pivot）。
2. 遍历数组，将比基数大的数字放到它的右边，比基数小的数字放到它的左边。遍历完成后，数组被分成了左右两个区域。
    - 从左向右遍历找到第一个大于等于基数的元素。
    - 从右向左遍历找到第一个小于等于基数的元素。
    - 交换两个元素。
3. 将左右两个区域视为两个数组，重复前两个步骤，当子数组排序完成即整个数组排序完成。

### 7.4 标准快排

```java
static void quickSort(int[] arr) {
    quickSort(arr, 0, arr.length - 1);
}

static void quickSort(int[] arr, int low, int high) {
    // [low, high]
    if (low >= high) return;

    // 1. 随机选取 pivot，并交换到第 1 个
    int pid = low + (int) (Math.random() * (high - low + 1));
    int temp = arr[low];
    arr[low] = arr[pid];
    arr[pid] = temp;

    // 2. partition 划分
    pid = partition(arr, low, high);

    // 3. 递归子数组
    quickSort(arr, low, pid - 1);
    quickSort(arr, pid + 1, high);
}

static int partition(int[] arr, int low, int high) {
    int pivot = arr[low];
    int l = low;
    int r = high;
    while (l < r) {
        // pivot 在左侧则先右后左，pivot 在右侧则先左后右
        while (l < r && arr[r] >= pivot) r--;
        arr[l] = arr[r];
        while (l < r && arr[l] <= pivot) l++;
        arr[r] = arr[l];
    }
    arr[l] = pivot;
    return l;
}
```

- partition 的其他实现方法

```java
static int partition(int[] arr, int low, int high) {
    int pivot = arr[low];
    int l = low;
    int r = high + 1;
    while (true) {
        while (l < high && arr[++l] < pivot);
        while (low < r && arr[--r] > pivot);
        if (l >= r) break;
        swap(arr, l, r);
    }
    swap(arr, low, r);
    return r;
}

static void swap(int[] arr, int i, int j) {
    int temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
}
```

- 非递归实现

```java
static void quickSort(int[] arr) {
    Deque<Integer> st = new ArrayDeque<>();
    // [low, high]
    st.push(0);
    st.push(arr.length - 1);

    while (!st.isEmpty()) {
        int high = st.poll();
        int low = st.poll();
        if (low >= high) continue;
        int pid = low + (int) (Math.random() * (high - low + 1));
        int temp = arr[low];
        arr[low] = arr[pid];
        arr[pid] = temp;
        pid = partition(arr, low, high);
        st.push(low);
        st.push(pid - 1);
        st.push(pid + 1);
        st.push(high);
    }
}

static int partition(int[] arr, int low, int high) {
    int pivot = arr[low];
    int l = low;
    int r = high;
    while (l < r) {
        // pivot 在左侧则先右后左，pivot 在右侧则先左后右
        while (l < r && arr[r] >= pivot) r--;
        arr[l] = arr[r];
        while (l < r && arr[l] <= pivot) l++;
        arr[r] = arr[l];
    }
    arr[l] = pivot;
    return l;
}
```

### 7.5 三向切分快排

将数组切分为三个部分：小于 pivot、等于 pivot、大于 pivot。

- 当数组中存在重复元素时效率更高。
- 每一轮确定 r - l + 1 个元素的位置。

```java
static void quickSort(int[] arr) {
    quickSort(arr, 0, arr.length - 1);
}

static void quickSort(int[] arr, int low, int high) {
    // [low, high]
    if (low >= high) return;
    // 1. 随机选取 pivot
    int pid = low + (int) (Math.random() * (high - low + 1));
    // 2. partition
    int pivot = arr[pid];
    int l = low;
    int m = low;
    int r = high;
    while (m <= r) {
        if (arr[m] < pivot) swap(arr, l++, m++);
        else if (arr[m] > pivot) swap(arr, m, r--);
        else m++;
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

## 8.堆排序

### 8.1 介绍

利用二叉堆实现一个能够插入元素、快速找出最大（或最小）元素的数据结构。

- 饿汉式：不保证元素的顺序，在需要找出最大（或最小）的元素时才采取行动。
- 懒汉式：保证元素的相对顺序，则可快速找出最大（或最小）的元素。

### 8.2 特点

- 时间复杂度：$ O(n \log n) $
- 空间复杂度：$ O(1) $

|数据结构|插入元素|删除最值|
|:---:|:---:|:---:|
|有序数组|$O(n)$|$O(1)$|
|无需数组|$O(1)$|$O(n)$|
|二叉堆|$O(\log n)$|$O(\log n)$|
|理想情况|$O(1)$|$O(1)$|

### 8.3 步骤

1. 使用数组实现最大堆（或最小堆）。
    - 用数组实现一棵完全二叉树，根结点为 arr[1]，arr[k] 的父结点为 arr[k / 2]，左孩子为 arr[k * 2]，右孩子为 arr[k * 2 + 1]。
    - 若为最大堆（或最小堆），父结点的值大于（或小于）等于两个孩子的值。
2. 建堆（堆的有序化）。
    - 上浮：当堆底添加了一个新元素时，需要**自下而上**恢复堆。
    - 下沉：当堆顶被删除而用堆底元素替换时，需要**自上而下**恢复堆。
3. 插入。
    - 将新元素加到数组末尾，增大堆的大小，然后从该位置进行上浮操作。
4. 删除最值。
    - 将根结点元素和数组末尾元素交换，减小堆的大小，然后从根结点开始下沉操作。

### 8.4 代码

```java
public static void heapSort(int[] arr) {
    int n = arr.length;
    int[] heap = new int[n + 1];
    for (int i = 1; i <= n; i++) {
        heap[i] = arr[i - 1];
        swim(heap, i, i);
    }
    for (int i = n; i >= 1; i--) {
        swap(heap, 1, i);
        sink(heap, i - 1, 1);
    }
    for (int i = 1; i <= n; i++)
        arr[i - 1] = heap[i];
}

private static void swim(int[] heap, int n, int k) {
    while (k > 1 && heap[k / 2] < heap[k]) {
        swap(heap, k / 2, k);
        k /= 2;
    }
}

private static void sink(int[] heap, int n, int k) {
    while (k * 2 < n) {
        int i = k * 2;
        if (i + 1 < n && heap[i] < heap[i + 1]) i++;
        if (heap[k] >= heap[i]) break;
        swap(heap, k, i);
        k = i;
    }
}

private static void swap(int[] arr, int i, int j) {
    int temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
}
```

## 9.计数排序

1. 统计每个元素的出现次数，入桶。
2. 按元素大小从小到大排序，桶有序。
3. 依次取出元素进行排序，出桶。

```java
static void countingSort(int[] arr) {
    if (arr == null || arr.length < 2) return;
    // 压缩桶的数量
    int minVal = arr[0];
    int maxVal = arr[0];
    for (int x : arr) {
        minVal = Math.min(minVal, x);
        maxVal = Math.max(maxVal, x);
    }
    int[] bucket = new int[maxVal - minVal + 1];
    // 入桶
    for (int x : arr) bucket[x]++;
    // 出桶，排序
    int k = 0;
    for (int i = minVal; i <= maxVal; i++)
        while (bucket[i - minVal]-- > 0)
            arr[k++] = i;
}
```

## 10.基数排序

<br />

## 11.桶排序

<br />

## 12.总结

| 算法   | 最好                | 平均             | 最坏            | 空间      | 稳定性 |
|:-----:|:------------------:|:----------------:|:---------------:|:--------:|:---:|
| 冒泡排序 | $ O(n) $          | $ O(n^2) $      | $ O(n^2) $      | $ O(1) $ | 稳定  |
| 选择排序 | $ O(n^2) $        | $ O(n^2) $      | $ O(n^2) $      | $ O(1) $ | 不稳定 |
| 插入排序 | $ O(n) $          | $ O(n^2) $      | $ O(n^2) $      | $ O(1) $ | 稳定 |
| 希尔排序 | $ O(n \log(2n)) $ | $ O(n^{1.5}) $ | $ O(n^2) $      | $ O(1) $ | 不稳定 |
| 归并排序 | $ O(n \log n) $   | $ O(n \log n) $ | $ O(n \log n) $ | $ O(n) $ | 稳定  |
| 快速排序 | $ O(n \log n) $   | $ O(n \log n) $ | $ O(n^2) $      | $ O(1) $ | 不稳定 |
| 堆排序  | $ O(n \log n) $   | $ O(n \log n) $ | $ O(n \log n) $ | $ O(1) $ | 不稳定 |
| 计数排序 | $ O(n + k) $      | $ O(n + k) $    | $ O(n + k) $    | $ O(k) $ | 稳定  |
| 基数排序 | $ O(nk) $         | $ O(nk) $       | $ O(nk) $       | $ O(k) $ | 稳定  |

## 13.实战

## 参考

1. [当我谈排序时，我在谈些什么🤔](https://leetcode-cn.com/problems/sort-an-array/solution/dang-wo-tan-pai-xu-shi-wo-zai-tan-xie-shi-yao-by-s/)
1. [复习基础排序算法（Java） - 排序数组 - 力扣（LeetCode）](https://leetcode-cn.com/problems/sort-an-array/solution/fu-xi-ji-chu-pai-xu-suan-fa-java-by-liweiwei1419/)
1. [十大排序从入门到入赘 - 力扣（LeetCode）](https://leetcode.cn/circle/discuss/eBo9UB/)

