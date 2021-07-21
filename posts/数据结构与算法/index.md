# 数据结构与算法


数据结构，是抽象的表示数据的方式；算法，则是计算的一系列有效、通用的步骤。算法与数据结构是程序设计中相辅相成的两个方面，是计算机学科的重要基石。

<!--more-->

## 1.基础

**存储结构：**

- **数组**：由于是紧凑连续存储，可以随机访问，通过索引快速找到对应元素，而且相对节约存储空间。但正因为连续存储，内存空间必须一次性分配够，所以说数组如果要扩容，需要重新分配一块更大的空间，再把数据全部复制过去，时间复杂度 O(N)；而且你如果想在数组中间进行插入和删除，每次必须搬移后面的所有数据以保持连续，时间复杂度 O(N)。
- **链表**：因为元素不连续，而是靠指针指向下一个元素的位置，所以不存在数组的扩容问题；如果知道某一元素的前驱和后驱，操作指针即可删除该元素或者插入新元素，时间复杂度 O(1)。但是正因为存储空间不连续，你无法根据一个索引算出对应元素的地址，所以不能随机访问；而且由于每个元素必须存储指向前后元素位置的指针，会消耗相对更多的储存空间。

## 2.排序

### 1.选择排序

```cpp
void selectionSort(vector<int>& arr) {
    for (int i = 0; i < arr.size(); i++) {
        int minIdx = i;
        for (int j = i + 1; j < arr.size(); j++) {
            minIdx = (arr[j] < arr[minIdx]) ? j : minIdx;
        }
        int temp = arr[i];
        arr[i] = arr[minIdx];
        arr[minIdx] = temp;
    }
}
```

- 时间复杂度：$ O(n^2) $
- 空间复杂度：$ O(1) $

特点：

- 不稳定
- 每一轮有一个元素（当前最小元素）归位

### 2.冒泡排序

```cpp
void bubbleSort(vector<int>& arr) {
    for (int step = 1; step < arr.size(); step++) {
        for (int i = 0; i < arr.size() - step; i++) {
            if (arr[i] > arr[i + 1]) {
                int temp = arr[i];
                arr[i] = arr[i + 1];
                arr[i + 1] = temp;
            }
        }
    }
}
```

- 时间复杂度：$ O(n^2) $
- 空间复杂度：$ O(1) $

特点：

- 稳定
- 每一轮有一个元素（当前最大元素）归位

### 3.插入排序

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

### 归并排序

```cpp
void mergeSort() {

}

void merge() {

}
```

### 快速排序

**基本思想：**

- 从数组中取出一个数，称之为基数（pivot）。
- 遍历数组，将比基数大的数字放到它的右边，比基数小的数字放到它的左边。遍历完成后，数组被分成了左右两个区域。
- 将左右两个区域视为两个数组，重复前两个步骤，直到排序完成。

```cpp
// 把数组分为两半，返回分割中点
int partition(vector<int> arr, int low, int high) {
    int pivot = arr[low];
    while (low < right) {
        while (low < high && arr[high] > pivot) high--;
        arr[low] = arr[high];
        while (low < high && arr[low] <= pivot) low++;
        arr[high] = arr[low];
    }
    arr[low] = pivot;
    return low;
}

void quickSort(vector<int> arr, int low, int high) {
    if (low >= high) return;
    int mid = partition(arr, low, high);
    quickSort(arr, low, mid - 1);
    quickSort(arr, mid + 1, high);
}
```

### 总结

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

参考文章

1. [当我谈排序时，我在谈些什么🤔](https://leetcode-cn.com/problems/sort-an-array/solution/dang-wo-tan-pai-xu-shi-wo-zai-tan-xie-shi-yao-by-s/)

## 树

- 一棵 n 个节点的树，有 n-1 条边。
- 一棵 n 个节点的树，有 n 棵子树。
- 根节点：唯一，无入度的节点
- 节点的深度：节点距离根节点的距离。

```cpp
typedef struct treeNode {
    treeNode(int x): value(x) {}
    int value;
    vector<treeNode*> child;
} TreeNode;
```

## 二叉树

满二叉树：一个高度为 d 的二叉树，有 $2^d-1$ 个节点。即除叶节点外，每个节点都有两个孩子，即节点的出度只为 0 或 2。

完全二叉树：只有最后一层可能未满，且节点严格从左往右排列。即出度为 1 的节点一定只有左孩子；若某节点出度小于 2，则其右边的节点出度为 0。

> - 二叉树第 $i$ 层最多有 $2^{i-1}$ 个节点。
> - 高度为 $d$ 的二叉树最多有 $2^d-1$ 个节点。

```python
class TreeNode(object):
	def __init__(self, value):
		self.lchild = None
        self.rchild = None
        self.value = 0
```

### 遍历

#### 前序遍历

递归法：

```cpp
void preorder(TreeNode* root) {
    if (!root) return;
    // 处理节点值 root->val
    preorder(root->left);
    preorder(root->right);
}
```

非递归法：

`压栈先右后左`

```cpp
void preorder(TreeNode* root) {
    if (!root) return;
    stack<TreeNode*> st;
    st.push(root);
    while (!st.empty()) {
        root = st.top();
        st.pop();
        // 处理节点值 root->val
        if (root->right) st.push(root->right);
        if (root->left) st.push(root->left);
    }
}
```

```cpp
void preorder(TreeNode* root) {
    stack<TreeNode*> st;
    whilt (root || !st.empty()) {
        if (root) {
            // 处理节点值 root->val
            st.push(root);
            root = root->left;
        } else {
            root = st.top()->right;
            st.pop();
        }
    }
}
```

#### 中序遍历

- 对于二叉搜索树，中序遍历可以得到一个递增的有序序列

递归法：

```cpp
void inorder(TreeNode* root) {
    if (!root) return;
    inorder(root->left);
    // 处理节点值 root->val
    inorder(root->right);
}
```

非递归法：

```cpp
void inorder(TreeNode* root) {
    stack<TreeNode*> st;
    while (root || !st.empty()) {
        if (root) {
            st.push(root);
            root = root->left;
        } else {
            // 处理节点值 st.top()->val
            root = st.top()->right;
            st.pop();
        }
    }
}
```

#### 后序遍历

- 后序遍历是删除节点时的顺序
- 可以配合栈来计算表达式树

递归法：

```cpp
void postorder(TreeNode* root) {
    if (!root) return;
    postorder(root->left);
    postorder(root->right);
    // 处理节点值 root->val
}
```

非递归法：

`前序遍历的非递归方法先左后右，最后逆序即可`

```cpp
void postorder(TreeNode* root) {
    if (!root) return;
    vector<int> res;  // 保存遍历结果
    stack<TreeNode*> st;
    st.push(root);
    while (!st.empty()) {
        root = st.top();
        st.pop();
        res.push_back(root->val);  // 保存节点值
        if (root->left) st.push(st->left);
        if (root->right) st.push(st->right);
    }
    reverse(res.begin(), res.end());  // 逆序
}
```

```cpp

```

#### 层序遍历

```cpp

```

### 常见问题

## 回溯算法

### N 皇后问题

题目描述：将 N 个皇后放在 N×N 的棋盘上，需保证任意两个皇后不能处于同行、同列或同斜线上。

```cpp
vector<vector<char>> solve(int n) {

}

void dfs(vector<vector<char>>& chessboard, int rowNo, int n) {

}
```

## 贪心与分治理论

### 贪心算法

#### 贪心算法理论

严格来说，贪心算法并不是某些有明确指向的算法，而是代指一类算法思想。在有多种决策可选时，我们会选择一个最优的策略，即所谓的贪心算法。

举一个最简单的例子，田忌赛马。在对方出上等马的时候，我方没有任何一匹马能赢这一局，既然注定是输，那么我们希望**尽量减少我们的损失**。何谓损失？我们每一局都会用掉一匹马，那么对于必输的局，显然用掉最弱的马是最好的。这里就可以归类出两个名词：

- **局部目标**：在贪心问题中，总归有一个局部的目标。例如在上述场景里，我们希望减少这一轮的损失。这就是一个局部目标。和局部目标对应的是全局目标，全局上来说我们当然希望最终能赢得比赛。
- **策略**：在这个局部情景里，我们有多种可用的决策，例如我们可以挑选任意一匹马应战。

实际上，很多问题都可以拆解为若干个局部问题和局部策略。如果这一类问题满足：

- **局部问题存在最优解。**
- **局部问题最优可以保证全局问题最优。**

那么这个问题就可以通过贪心算法解决。

> - 小技巧：局部问题又称为子问题，很多复杂的原始问题都可以拆解成若干个子问题构成，例如一盘围棋就可以拆解为每次双方执子的小问题。在不同的情景下，子问题的性质是不一样的，对应的解决办法也不一样。
> - 例如：
>     - 子问题最优则原始问题最优——贪心算法或者动态规划算法。
>     - 子问题最优则原始问题最优，且子问题互相独立——分治算法。
>     - 子问题最优不能推导出原始问题最优——暴力搜索等。

#### 算法中的贪心思想

**例一：二叉搜索树找最小值**

子问题：最小值一定在根节点，左子树（如果存在），右子树（如果存在）三者之一上，因此原问题可以划分为三个子问题。

我们本来可以在左右子树上均查找一次最小值，但是根据二叉查找树的性质，**如果左子树存在，那么最小值只可能存在于左子树上**。这就是一个贪心的思想，通过只找一边的子树，我们可以将复杂度从`O(n)`降低至`O(log(h))`，其中`h`为树的高度。

**例二：二分查找问题**

同样，二分查找也存在贪心的思想。在确定`left, mid, right`后，根据`target`和`mid`的大小关系，我们同样只会继续查找左半边或者右半边，这也是因为另一边不可能有目标值了。

#### 贪心问题解决思路

那么对于原始的复杂问题，如何能够知道他是否能被贪心解决呢？

1. 首先，我们需要将原始问题拆解成子问题，明确子问题的局面以及局面中可进行的操作。实际上不止贪心问题，很多问题都需要这样的拆解过程。
2. 然后我们需要考虑子问题的最优，是否能保证全局最优。如果能的话，我们就可以只考虑如何解子问题，否则就可能需要动态规划或者搜索解了。
3. 最后，我们需要考虑如何使子问题最优，可能有些问题存在乍一看可行的策略，但是我们仍然需要仔细思考甚至证明。以保证没有漏掉什么细节情况。

> 小技巧：对于第三点，很多同学这里会经常犯错。比如以为贪心做法是对的，但是实际上有问题。例如错误地用贪心解决 01 背包问题（子问题最优，原问题不一定最优）等。所以如果我们觉得某道题目存在贪心策略，最好自己证明一下正确性再实现。

### 分治算法

复杂的原始问题可能可以拆分成若干个子问题，如果子问题之间**互相独立（一个子问题的计算结果不依赖于其他子问题）**，那么原始问题可以被分治法解决。

分治法究竟有什么作用呢？

- 简化思维逻辑：在很多情况下，原始问题是非常复杂的，例如排序问题。假设原始我们需要考虑对 1000 个数进行排序，那么利用分治思想我们可以分别对左右的 500 个数进行排序，然后考虑合并两个有序数组。当然，排序 500 个数看起来仍然不容易，但是我们可以继续分治下去，最终我们只需要考虑 1~2 个数的排序策略，这就是经典的归并排序的思想。
- 分布式算法：虽然在算法学习的过程中少有接触多进程和分布式等思想。但是随着 CPU core 越来越多，能够被分治法拆解的问题显然更方便进行并行计算，从而节省总体时间。因此分治思想在工程实现上具有重要的意义。
- 效率优化：虽然我们不常用并行解决算法问题，但是在某些情况下仍然能够帮助我们节省计算代价，代表就是快速幂算法。课程在这里不作展开，我们会在例题部分进一步详细讨论。

### 总结

对于复杂的原问题：

- 如果子问题最优则原问题最优，贪心算法。
- 如果子问题需要全部求解才能求解原问题，子问题互相独立，分治算法。
- 如果子问题最优不能保证原问题最优，但是子问题之间不会循环（所谓循环，是指从问题 A 拆解出子问题 B，然后子问题 B 又能拆解出子问题 A），考虑动态规划算法。
- 更加复杂的情况，我们总是可以考虑暴力搜索解决。

## 动态规划

考虑能否将问题规模减小
将问题规模减小的方式有很多种，一些典型的减小方式是动态规划分类的依据，例如线性，区间，树形等。这里考虑数组上常用的两种思路：

每次减少一半：如果每次将问题规模减少一半，原问题有[10,9,2,5]，和[3,7,101,18]，两个子问题的最优解分别为 [2,5] 和 [3,7,101]，但是找不到好的组合方式将两个子问题最优解组合为原问题最优解 [2,5,7,101]。

每次减少一个：记 f(n)f(n) 为以第 nn 个数结尾的最长子序列，每次减少一个，将原问题分为 f(n-1)f(n−1), f(n-2)f(n−2), ..., f(1)f(1)，共 n - 1n−1 个子问题。n - 1 = 7n−1=7 个子问题以及答案如下：

以上组合方式可以写成一个式子，即状态转移方程

总结： 解决动态规划问题最难的地方有两点：

如何定义 f(n)f(n)
如何通过 f(1)f(1), f(2)f(2), … f(n - 1)f(n−1) 推导出 f(n)f(n)，即状态转移方程

1. 递归

有了状态转移方程，实际上已经可以直接用递归进行实现了。

2. 自顶向下（记忆化）

递归的解法需要非常多的重复计算，如果有一种办法能避免这些重复计算，可以节省大量计算时间。记忆化就是基于这个思路的算法。在递归地求解子问题 f(1)f(1), f(2)f(2)... 过程中，将结果保存到一个表里，在后续求解子问题中如果遇到求过结果的子问题，直接查表去得到答案而不计算。

对于这种将问题规模不断减少的做法，我们把它称为自顶向下的方法。

3. 自底向上（迭代）

在自顶向下的算法中，由于递归的存在，程序运行时有额外的栈的消耗。

有了状态转移方程，我们就知道如何从最小的问题规模入手，然后不断地增加问题规模，直到所要求的问题规模为止。在这个过程中，我们同样地可以记忆每个问题规模的解来避免重复的计算。这种方法就是自底向上的方法，由于避免了递归，这是一种更好的办法。

但是迭代法需要有一个明确的迭代方向，例如线性，区间，树形，状态压缩等比较主流的动态规划问题中，迭代方向都有相应的模式。参考后面的例题。但是有一些问题迭代法方向是不确定的，这时可以退而求其次用记忆化来做，参考后面的例题。

