# 数据结构与算法


<!--more-->

## 1.基础

**存储结构：**

- **数组**：由于是紧凑连续存储，可以随机访问，通过索引快速找到对应元素，而且相对节约存储空间。但正因为连续存储，内存空间必须一次性分配够，所以说数组如果要扩容，需要重新分配一块更大的空间，再把数据全部复制过去，时间复杂度 O(N)；而且你如果想在数组中间进行插入和删除，每次必须搬移后面的所有数据以保持连续，时间复杂度 O(N)。
- **链表**：因为元素不连续，而是靠指针指向下一个元素的位置，所以不存在数组的扩容问题；如果知道某一元素的前驱和后驱，操作指针即可删除该元素或者插入新元素，时间复杂度 O(1)。但是正因为存储空间不连续，你无法根据一个索引算出对应元素的地址，所以不能随机访问；而且由于每个元素必须存储指向前后元素位置的指针，会消耗相对更多的储存空间。

## 2.排序

### 1.选择排序

```cpp
void selectionSort(vector<int>& nums) {
    int n = nums.size();
    for (int i = 0; i < n; i++) {
        int minIdx = i;
        for (int j = i + 1; j < n; j++) {
            minIdx = (arr[j] < arr[minIdx]) ? j : minIdx;
        }
        int temp = arr[i];
        arr[i] = arr[minIdx];
        arr[minIdx] = temp;
    }
}
```

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

void swap(int[] nums, int a, int b) {
    int temp = nums[a];
    nums[a] = nums[b];
    nums[b] = temp;
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
int partition(vector<int> &arr, int low, int high) {
    // [low, high]
    int pivotId = low + rand() % (high - low + 1);
    swap(arr[low], arr[pivotId]);
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

void quickSort(vector<int> &arr, int low, int high) {
    if (low >= high) return;
    int mid = partition(arr, low, high);
    quickSort(arr, low, mid - 1);
    quickSort(arr, mid + 1, high);
}
```

```java

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
1. [复习基础排序算法（Java） - 排序数组 - 力扣（LeetCode）](https://leetcode-cn.com/problems/sort-an-array/solution/fu-xi-ji-chu-pai-xu-suan-fa-java-by-liweiwei1419/)

## 栈

### 单调栈

#### 返回第一个更大元素

```cpp
vector<int> nextGreaterElement(vector<int>& nums) {
    int n = nums.size();
    vector<int> ans(n);
    stack<int> st;
    for (int i = n - 1; i >= 0; i--) {
        while (!st.empty() && st.top() <= nums[i]) {
            // 栈顶元素比当前元素小，弹出
            st.pop();
        }
        // 栈顶元素即为下一个更大元素
        ans[i] = st.empty() ? -1 : st.top();
        // 当前元素入栈
        st.push(nums[i]);
    }
    return ans;
}
```

```java
int[] nextGreaterElement(int[] nums) {
    int n = nums.length;
    int[] ans = new int[n];
    Deque<Integer> stack = new LinkedList<>();
    for (int i = n - 1; i >= 0; i--) {
        while (!stack.isEmpty() && stack.peek() <= nums[i]) {
            stack.pop();
        }
        ans[i] = stack.isEmpty() ? -1 : stack.peek();
        stack.push(nums[i]);
    }
    return ans;
}
```

#### 循环数组

```java
int[] nextGreaterElement(int[] nums) {
    int n = nums.length;
    int[] ans = new int[n];
    Stack<Integer> st = new Stack<>();
    for (int i = 2 * n - 1; i >= 0; i--) {
        while (!st.isEmpty() && st.peek() <= nums[i % n]) {
            st.pop();
        }
        ans[i % n] = st.isEmpty() ? -1 : st.peek();
        st.push(nums[i % n]);
    }
    return ans;
}
```

## 数组

### 打印数组

```java
import java.utils.Arrays;
int[] arr1;
System.out.println(Arrays.toString());
int[][] arr2;
System.out.println(Arrays.deepToString());
```

### 转换

```java
// int[] 转 List<Integer>
// 不可添加、删除、修改
int[] arr;
List<Integer> list = List.of(arr);
```

```java
// int[] 转 Integer[]
int[] arr;
Integer[] arr2 = IntStream.of(arr).boxed()
```

```java
// Integer[] 转 List<Integer>
// 不可添加、删除、修改
Integer[] arr;
List<Integer> list = List.of(arr);
```

```java
// List<Integer> 转 Integer[]
List<Integer> list;
int[] arr2 = list.stream().mapToInt().toArray(Integer::new);
```

## 链表

```java
class ListNode {
    int val;
    ListNode next;
    // 双向链表
    ListNode prev;
}
```

**trick:**

- 双指针（快慢指针）
- 虚拟头结点

### 21. 合并两个有序链表

双指针

### 23. 合并K个升序链表

优先队列

### 返回链表的倒数第 k 个节点

```java
public static ListNode findFromEnd1(ListNode head, int k) {
    // 要求 1 <= k <= n
    ListNode p1 = head, p2 = head;
    // p1 先走 k 步
    for (int i = 0; i < k && p1 != null; i++) {
        p1 = p1.next;
    }
    // 然后 p1 和 p2 同步走，走到头
    // 即 p2 走了 n - k 步，也就是倒数第 k 个
    while (p1 != null) {
        p1 = p1.next;
        p2 = p2.next;
    }
    return p2;
}
```

```java
// 不适用于多测试用例，因为 count 是静态的
// 或者每次调用前将 count 归零
public static count = 0;

public static ListNode findFromEnd2(ListNode head, int k) {
    // 要求 1 <= k <= n
    if (head == null) {
        return null;
    }
    ListNode node = findFromEnd2(head.next, k);
    // 从尾部向前计数
    count++;
    if (count == k) {
        return head;
    }
    return node;
}
```

### 删除链表的倒数第 N 个结点

1. 添加虚表头结点。
2. 返回链表的倒数第 k+1 个节点，删除后继结点。

### 单链表的中点

快慢指针，慢走一步，快走两步。

### 判断链表是否包含环

```java
boolean hasCycle(ListNode head) {
    // 快慢指针初始化指向 head
    ListNode slow = head, fast = head;
    // 快指针走到末尾时停止
    while (fast != null && fast.next != null) {
        // 慢指针走一步，快指针走两步
        slow = slow.next;
        fast = fast.next.next;
        // 快慢指针相遇，说明含有环
        if (slow == fast) {
            return true;
        }
    }
    // 不包含环
    return false;
}
```

### 如果链表中含有环，如何计算这个环的起点？

```java
ListNode detectCycle(ListNode head) {
    ListNode fast = head, slow = head;
    while (fast != null && fast.next != null) {
        fast = fast.next.next;
        slow = slow.next;
        if (fast == slow) {
            break;
        }
    }
    if (fast == null || fast.next == null) {
        // 无环
        return null;
    }
    /*
       相遇时，slow 走了 k 步，fast 走了 2k 步
       则 k 为环的长度，设相遇点距环起点距离为 m
       则表头到环起点距离为 k - m，相遇点向前走到环起点距离为 k - m
       则将 slow 放到表头，然后 slow 和 fast 同步走
       相遇即为环起点
    */
    // 重新指向头结点
    slow = head;
    // 快慢指针同步前进，相交点就是环起点
    while (slow != fast) {
        fast = fast.next;
        slow = slow.next;
    }
    return slow;
}
```

### 两个链表是否相交

1. 让 p1 遍历完链表 A 之后开始遍历链表 B，让 p2 遍历完链表 B 之后开始遍历链表 A，这样相当于「逻辑上」两条链表接在了一起。

```java
ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    // p1 指向 A 链表头结点，p2 指向 B 链表头结点
    ListNode p1 = headA, p2 = headB;
    while (p1 != p2) {
        // p1 走一步，如果走到 A 链表末尾，转到 B 链表
        if (p1 == null) {
            p1 = headB;
        } else {
            p1 = p1.next;
        }
        // p2 走一步，如果走到 B 链表末尾，转到 A 链表
        if (p2 == null) {
            p2 = headA;
        } else {
            p2 = p2.next;
        }
    }
    return p1;
}
```

2. 如果把两条链表首尾相连，那么「寻找两条链表的交点」的问题转换成了前面讲的「寻找环起点」的问题。

3. 预先计算两条链表的长度。

```java
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    int lenA = 0, lenB = 0;
    // 计算两条链表的长度
    for (ListNode p1 = headA; p1 != null; p1 = p1.next) {
        lenA++;
    }
    for (ListNode p2 = headB; p2 != null; p2 = p2.next) {
        lenB++;
    }
    // 让 p1 和 p2 到达尾部的距离相同
    ListNode p1 = headA, p2 = headB;
    if (lenA > lenB) {
        for (int i = 0; i < lenA - lenB; i++) {
            p1 = p1.next;
        }
    } else {
        for (int i = 0; i < lenB - lenA; i++) {
            p2 = p2.next;
        }
    }
    // 看两个指针是否会相同，p1 == p2 时有两种情况：
    // 1、要么是两条链表不相交，他俩同时走到尾部空指针
    // 2、要么是两条链表相交，他俩走到两条链表的相交点
    while (p1 != p2) {
        p1 = p1.next;
        p2 = p2.next;
    }
    return p1;
}
```

4. 哈希表存储结点。

### 递归反转整个链表

```java
ListNode reverse(ListNode head) {
    if (head == null || head.next == null) {
        return head;
    }
    // 返回从 head.next 开始的链表的逆序的头结点
    // newHead 即为原链表尾结点
    ListNode newHead = reverse(head.next);
    // head.next 原来是待逆序链表的头结点
    // 逆序后变为尾结点
    // 将 head 作为新的尾结点，实现逆序
    head.next.next = head;
    head.next = null;
    // 返回逆序后链表的头结点
    return newHead;
}
```

### 递归反转链表前 N 个节点

```java
// 后继结点
ListNode successor = null;

ListNode reverseN(ListNode head, int n) {
    if (n == 1) {
        successor = head.next;
        return head;
    }
    ListNode newHead = reverseN(head.next, n - 1);
    head.next.next = head;
    // 将尾结点连接到后继结点上
    head.next = successor;
    return newHead;
}
```

### 递归反转链表第 M 个节点到第 N 个节点

```java
ListNode reverseBetween(ListNode head, int m, int n) {
    if (m == 1) {
        return reverseN(head, n);
    }
    head.next = reverseBetween(head.next, m - 1, n - 1);
    return head;
}
```

### 25. K 个一组翻转链表

```java

```

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

```java
class TreeNode {
    public int val;
    public TreeNode[] children;
    TreeNode() {
        this.val = 0;
    }
}
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

```python
结果集合

def backtrack(当前路径, 可选择列表):
    if 可选择列表为空:
        结果集合添加当前路径
        return

    for 当前选择 in 可选择列表:
        当前路径添加当前选择
        可选择列表删除当前选择
        backtrack(当前路径, 可选择列表)
        当前路径撤销当前选择
        可选择列表添加当前选择
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

**定义：**

- 存在「重叠子问题」
- 具备「最优子结构」

**步骤：**

- 确定「初始条件」
- 确定「状态」
- 确定「可选择列表」
- 确定「状态转移方程」
- 数组优化时间
- 降维优化空间

**思路：**

- 自顶向下

```python
def fib(n):
    return fib(n - 1) + fib(n - 2)
```

- 自底向上

```python
def fib(n):
    for i in range(n + 1):
        dp[i] = dp[i - 1] + dp[i - 2]
    return dp[n]
```

## 图论

### 定义

```java
// 结点
class Vertex {
    int id;
    List<Vertex> neighbors;
}
// 邻接表 无边权
List<Integer>[] adjList1;
// 邻接表 有边权
List<int[]>[] adjList2;
// 邻接表，查询效率更高
TreeSet<Integer>[] adjSet;
// 邻接矩阵
int[][] adjMat;
```

- 邻接表
    - 优点：占用的空间少
    - 缺点：无法快速判断两个节点是否相邻
    - 适用于稀疏图
- 邻接矩阵
    - 优点：占用的空间多
    - 缺点：可以快速判断两个节点是否相邻
    - 适用于稠密图

### 遍历

#### DFS

```java
boolean[] vis;
Deque<Integer> trace;                            // 记录当前路径
boolean[] onPath;                                // 判断某顶点是否在当前路径上
List<List<Integer>> traces = new LinkedList<>(); // 记录所有路径

// 邻接表
void dfs(List<Integer>[] graph, int begin, int end) {
    if (vis[begin]) {
        return;
    }
    // 访问当前顶点
    vis[begin] = true;
    onPath[begin] = true;
    trace.offerLast(begin);
    // 到达终点
    if (begin == end) {
        // 防止已添加路径被修改
        traces.add(new LinkedList<Integer>(trace));
        // 回溯
        vis[begin] = false;
        onPath[begin] = false;
        trace.pollLast();
        return;
    }
    // 下个顶点
    for (int next : graph[begin]) {
        if (!vis[next]) {
            dfs(graph, next, end);
        }
    }
    // 回溯
    vis[begin] = false;
    onPath[begin] = false;
    trace.pollLast();
}
```

```java
boolean[] vis;
Deque<Integer> trace;                            // 记录当前路径
boolean[] onPath;                                // 判断某顶点是否在当前路径上
List<List<Integer>> traces = new LinkedList<>(); // 记录所有路径

// 邻接矩阵
void dfs(int[][] graph, int begin) {
    if (vis[begin]) {
        return;
    }
    // 访问当前顶点
    vis[begin] = true;
    onPath[begin] = true;
    trace.offerLast(begin);
    // 到达终点
    if (begin == end) {
        // 防止已添加路径被修改
        traces.add(new LinkedList<Integer>(trace));
        // 回溯
        vis[begin] = false;
        onPath[begin] = false;
        trace.pollLast();
        return;
    }
    // 下个顶点
    for (int next = 0; next < graph.length; next++) {
        if (!vis[next] && graph[begin][next] != 0) {
            dfs(graph, next);
        }
    }
    // 回溯
    vis[begin] = false;
    onPath[begin] = false;
    trace.pollLast();
}
```

#### BFS

```python
def BFS(开始节点 s):
    q = 队列
    vis = 集合/哈希表/布尔数组
    将s添加到q中
    将s添加到vis中/标记s为已访问
    步数 = 0
    while(q非空):
        for q的每一个节点，记为h # 每次清空队列
            if 满足结束条件:
                return
            for h的每个相邻节点x:
                if x未在vis中/x未访问:
                    将x添加到q中
                    将x添加到vis中/标记x为已访问
        步数 += 1
```

```python
# 双向BFS
def BiBFS(开始节点 s，结束节点 e):
    s1 = 从s开始的搜索集合
    s2 = 从e开始的搜索集合
    vis = 集合/哈希表/布尔数组
    将s添加到s1中
    将s添加到vis中/标记s为已访问
    将e添加到s2中
    将e添加到vis中/标记e为已访问
    步数 = 0
    while(s1非空 and s2非空):
        if s1中元素比s2多:
            # 交换s1和s2
            s1, s2 = s2, s1
        s3 = 过渡集合
        for s1的每一个节点，记为h # 每次清空队列
            if h在s2中存在:
                return
            for h的每个相邻节点x:
                if x未在vis中/x未访问:
                    将x添加到s3中
                    将x添加到vis中/标记x为已访问
        # 交换q1和q2（一边一轮）
        s1 = s2
        s2 = s3
        步数 += 1
```

### Flood Fill

#### 岛屿数量

```java
int[] dx = {0, 1, 0, -1};
int[] dy = {1, 0, -1, 0};
int m, n;

public int numIslands(char[][] grid) {
    m = grid.length;
    n = grid[0].length;
    boolean[][] visited = new boolean[m][n];
    int res = 0;
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (grid[i][j] == '1' && !visited[i][j]) {
                res++;
                dfs(grid, i, j, visited);
            }
        }
    }
    return res;
}

void dfs(char[][] grid, int x, int y, boolean[][] visited) {
    if (x < 0 || x >= m || y < 0 || y >= n || visited[x][y] || grid[x][y] == '0') {
        return;
    }
    visited[x][y] = true;
    for (int i = 0; i < 4; i++) {
        int nextX = x + dx[i];
        int nextY = y + dy[i];
        dfs(grid, nextX, nextY, visited);
    }
}
```

#### 封闭岛屿的数量

将靠边的岛屿变为水，剩下的就是「封闭岛屿」。

```java
void dfs(int[][] grid, int x, int y) {
    if (x < 0 || x >= m || y < 0 || y >= n || grid[x][y] == 0) {
        return;
    }
    grid[x][y] = 0; // 淹没
    for (int i = 0; i < 4; i++) {
        int nextX = x + dx[i];
        int nextY = y + dy[i];
        dfs(grid, nextX, nextY);
    }
}
```

#### 1020. 飞地的数量

先把靠边的陆地淹掉，然后去数剩下的陆地数量。

#### 695. 岛屿的最大面积

淹没岛屿的同时，记录这个岛屿的面积。

#### 1905. 统计子岛屿

岛屿 B 中存在一片陆地，在岛屿 A 的对应位置是海水，那么岛屿 B 就不是岛屿 A 的子岛。

#### 694. 不同岛屿的数量

对于形状相同的岛屿，如果从同一起点出发，dfs 函数遍历的顺序肯定是一样的。

分别用 1, 2, 3, 4 代表上下左右，用 -1, -2, -3, -4 代表上下左右的撤销。

把二维矩阵中的「岛屿」进行转化，变成比如字符串这样的类型，然后利用 HashSet 这样的数据结构去重，最终得到不同的岛屿的个数。

## 字符串

### 1 KMP 算法

Knuth-Morris-Pratt 算法。

#### 原理

主串`text`长度为`n`，匹配串`pattern`长度为`m`。

KMP 算法首先算出一个`next`数组，匹配串每轮匹配在`j`位置失配时，匹配串向右滑动的距离为`j - next[j]`。

- `next[0] = -1`
- `j > 0`时`next[j]`为匹配串中区间`[0, j - 1]`的严格前缀子串和严格后缀子串中最长公共子串的长度。

设匹配串为`abcdabd`。

| `j` | 子串 | 严格前缀子串 | 严格后缀子串 | 最长公共子串 | `next[j]` |
|:---:|:---:|:---:|:---:|:---:|:---:|
| 0 |  |  |  |  | -1 |
| 1 | `a` |  |  |  | 0 |
| 2 | `ab` | `a` | `b` |  | 0 |
| 3 | `abc` | `a`、`ab` | `bc`、`c` |  | 0 |
| 4 | `abcd` | `a` 、 `ab`、`abc` | `bcd`、`cd` 、 `d` |  | 0 |
| 5 | `abcda` | `a` 、 `ab` 、 `abc`、`abcd` | `bcda`、`cda` 、 `da` 、 `a` | `a` | 1 |
| 6 | `abcdab` | `a` 、 `ab` 、 `abc` 、 `abcd`、`abcda` | `bcdab`、`cdab`、`dab`、`ab`、`b` | `ab` | 2 |

- 时间复杂度：`O(n + m)`

#### 代码

```java
int[] getNext(char[] pattern) {
    int m = pattern.length;
    int[] next = new int[m];
    next[0] = -1; // 特殊情况
    int i = 0;    // [0, i - 1] 区间的最长公共子串
    int j = -1;
    while (i < m - 1) {
        if (j == -1 || pattern[i] == pattern[j]) {
            i++;
            j++;
            next[i] = j;
        } else {
            j = next[j];
        }
    }
    return next;
}

int kmpSearch(char[] text, char[] pattern) {
    int n = text.length;
    int m = pattern.length;
    if (m == 0) {
        return 0;
    }
    int[] next = getNext(pattern);
    int i = 0; // 主串指针
    int j = 0; // 匹配串指针
    while (i < n && j < m) {
        if (j == -1 || text[i] == pattern[j]) {
            i++;
            j++;
        } else {
            j = next[j];
        }
    }
    return j == m ? i - j : -1;
}
```

#### 参考

1. [字符串匹配的KMP算法 - 阮一峰的网络日志](https://www.ruanyifeng.com/blog/2013/05/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm.html)
1. [如何更好地理解和掌握 KMP 算法? - 知乎](https://www.zhihu.com/question/21923021/answer/281346746)
1. [字符串匹配算法详解 - 云+社区 - 腾讯云](https://cloud.tencent.com/developer/article/1770486)

### 2 BM 算法

Boyer-Moore 算法。

#### 原理

主串`text`长度为`n`，匹配串`pattern`长度为`m`。

1. 匹配串从后往前匹配

2. 坏字符：将主串中未与匹配串匹配的第一个字符`pattern[j]`称为坏字符，然后匹配串向右滑动的距离为`j - 匹配串中该字符上次出现的位置（未出现返回-1）`。

{{< admonition tip "示例" false >}}
设匹配串为`abcdabc`。

| 字符 | 匹配串中该字符上次出现的位置 |
|:---:|:---:|
| `a` | 4 |
| `b` | 5 |
| `c` | 6 |
| `d` | 3 |
| 其他 | -1 |
{{< /admonition >}}


3. 好后缀：匹配串中已匹配的后缀子串称为好后缀，然后然后匹配串向右滑动的距离为`m - 好后缀和匹配串前缀子串的最长公共子串长度`。特殊地，当`j == m - 1`时无已匹配部分，定义`goodSuffix[m - 1] = m - 1`。

{{< admonition tip "示例" false >}}
设匹配串为`abcdabc`。

| `j` | 好后缀 | 最长公共子串 | `goodSuffix[j]` |
|:---:|:---:|:---:|:---:|
| 6 |  |  | 6 |
| 5 | `c` |  | 0 |
| 4 | `bc`、`c` |  | 0 |
| 3 | `abc`、`bc`、`c` | `abc` | 3 |
| 2 | `dabc`、`abc`、`bc`、`c` | `abc` | 3 |
| 1 | `cdabc`、`dabc`、`abc`、`bc`、`c` | `abc` | 3 |
| 0 | `bcdabc`、`cdabc` 、 `dabc` 、 `abc` 、 `bc` 、 `c` | `abc` | 3 |
{{< /admonition >}}

4. 每次匹配串向右滑动这两个规则之中的较大值。可以预处理出`badChar<char, int>`和`goodSuffix[]`。

#### 代码

```java
HashMap<Character, Integer> getBadChar(String pattern) {
    int m = pattern.length();
    // 坏字符
    HashMap<Character, Integer> badChar = new HashMap<>();
    for (int i = 0; i < m; i++) {
        badChar.put(pattern.charAt(i), i);
    }
    return badChar;
}

int[] getGoodSuffix(String pattern) {
    int m = pattern.length();
    // 好后缀
    int[] goodSuffix = new int[m];
    goodSuffix[m - 1] = m - 1;
    int maxLen = 0;
    for (int i = m - 2; i >= 0; i--) {
        int j = 0;
        // 查找公共子串
        while (i + j + 1 < m && pattern.charAt(j) == pattern.charAt(i + j + 1)) {
            j++;
        }
        if (i + j + 1 < m) {
            // 不存在公共子串
            goodSuffix[i] = maxLen;
        } else {
            goodSuffix[i] = j;
            maxLen = j;
        }
    }
    return goodSuffix;
}

int bmSearch(String text, String pattern) {
    int n = text.length();
    int m = pattern.length();
    if (n == 0 || m == 0) {
        return -1;
    }
    HashMap<Character, Integer> badChar = getBadChar(pattern);
    int[] goodSuffix = getGoodSuffix(pattern);
    int i = m - 1;
    while (i < n) {
        // 匹配串从后往前匹配
        int j = m - 1;
        while (j >= 0) {
            char c = text.charAt(i + j - m + 1);
            if (c != pattern.charAt(j)) {
                int badCharDis = j - badChar.getOrDefault(c, -1);
                int goodSuffixDis = m - goodSuffix[j];
                i += Math.max(badCharDis, goodSuffixDis);
                break;
            }
            j--;
        }
        if (j == -1) {
            return i - m + 1;
        }
    }
    return -1;
}
```

#### 参考

1. [字符串匹配的Boyer-Moore算法 - 阮一峰的网络日志](https://www.ruanyifeng.com/blog/2013/05/boyer-moore_string_search_algorithm.html)
1. [字符串匹配算法详解 - 云+社区 - 腾讯云](https://cloud.tencent.com/developer/article/1770486)

### 3 Sunday 算法

#### 原理

- 主串`text`长度为`n`，匹配串`pattern`长度为`m`。

- 当`text[i + j] != pattern[j]`时，观察主串中匹配串的下一个字符`text[i + m]`：
    - 若`text[i + m]`在`pattern`中存在，则`i += m - c最后出现的位置`
    - 若`text[i + m]`在`pattern`中不存在，则`i += m + 1`

- 时间复杂度
    - 平均：`O(n)`
    - 最坏：`O(n * m)`

#### 代码

```java
int sundaySearch(char[] text, char[] pattern) {
    int n = text.length;
    int m = pattern.length;
    // 字符最后出现的位置
    HashMap<Character, Integer> pos = new HashMap<>();
    for (int i = 0; i < m; i++) {
        pos.put(pattern[i], i);
    }
    int i = 0;
    while (i + m <= n) {
        int j = 0;
        while (j < m) {
            if (text[i + j] != pattern[j]) {
                if (i + m < n && pos.containsKey(text[i + m])) {
                    i += m - pos.get(text[i + m]);
                } else {
                    i += m + 1;
                }
                break;
            }
            j++;
        }
        if (j == m) {
            return i;
        }
    }
    return -1;
}
```

#### 参考

1. [Sunday 解法 - 实现 strStr() - 力扣（LeetCode）](https://leetcode-cn.com/problems/implement-strstr/solution/python3-sundayjie-fa-9996-by-tes/)

### 4. Rabin Karp 算法

#### 原理

- 主串`text`长度为`n`，匹配串`pattern`长度为`m`。

- 使用字符串哈希算法将字符串比较转化为整数比较。然后通过滚动计算哈希来降低时间复杂度。最后防止出现哈希冲突，再朴素比较一遍。

- 区间`[a,b]`的哈希值为

$$hash1=text[a] \times k^{b-a} + \cdots + text[b] \times k^{0}$$

- 区间`[a+1,b+1]`的哈希值为

$$hash2=text[a+1] \times k^{b-a} + \cdots + text[b+1] \times k^{0}$$

$$hash2=(hash1-text[a] \times k^{b-a}) \times k + text[b + 1] \times k^{0}$$

- 如果字符串过长，最后计算哈希可能会溢出。为了解决这个问题，使用取余。

$$hash2=((hash1-text[a] \times k^{b-a} \mod q) \times k + text[b + 1] \times k^{0}) \mod q$$

- 最后，`k`取一个大于`text[i]`取值范围的质数即可。

- 时间复杂度：`O(n + m)`

#### 代码

```java
int rkSearch(char[] text, char[] pattern) {
    int n = text.length;
    int m = pattern.length;
    final int MOD = (int) 1e7 + 7; // 取余
    final int K = 31; // 任意数字即可，一般为质数
    final int POWER = (int) Math.pow(K, m - 1) % MOD;
    int pHash = 0;
    int tHash = 0;
    for (int i = 0; i < m; i++) {
        pHash = (pHash * K + pattern[i]) % MOD;
        tHash = (tHash * K + text[i]) % MOD;
    }
    for (int i = 0; i + m <= n; i++) {
        if (pHash == tHash) {
            boolean equal = true;
            // 二次判断，防止哈希冲突
            for (int j = 0; j < m; j++) {
                if (text[i + j] != pattern[j]) {
                    equal = false;
                    break;
                }
            }
            if (equal) {
                return i;
            }
        }
        if (i + m >= n) {
            break;
        }
        // 滚动计算哈希
        tHash = ((tHash - text[i] * POWER % MOD) * K + text[i + m]) % MOD;
        if (tHash < 0) {
            tHash += MOD;
        }
    }
    return -1;
}
```

#### 参考

1. [字符串匹配算法-Rabin Karp算法 | coolcao的小站](https://coolcao.com/2020/08/20/rabin-karp/)
1. [简单易懂的Rabin Karp算法详解！ - 实现 strStr() - 力扣（LeetCode）](https://leetcode-cn.com/problems/implement-strstr/solution/yi-dong-de-rabin-karpsuan-fa-hao-xiang-mei-ren-xie/)

## 并查集 Union Find

## 二分查找

### 闭区间的二分查找

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

### 左闭右开区间的二分查找

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

### 左侧边界的二分查找 lower_bound

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

### 右侧边界的二分查找 upper_bound

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

## 滑动窗口

### 窗口大小可任意调整

```java
// 窗口大小可任意调整
int left = 0;
int right = 0;
int ans = 0;
// [left, right]
while (right < n) {
    // 增大窗口
    // 右端点 right 操作
    // 修改约束值
    while (condition) { // 约束值满足调整窗口的条件
        // 缩小窗口
        // 左端点 left 操作
        // 修改约束值
        left++;
    }
    ans = Math.max(ans, right - left + 1);
    right++;
}
```

### 窗口大小单调递增

```java
// 窗口大小单调递增
int left = 0;
int right = 0;
// [left, right]
while (right < n) {
    // 增大窗口
    // 右端点 right 操作
    // 修改约束值
    if (condition) { // 约束值满足调整窗口的条件
        // 此时左端点移动最多使窗口大小不变
        // 左端点 left 操作
        // 修改约束值
        left++;
    }
    right++;
}
int ans = right - left;
```

