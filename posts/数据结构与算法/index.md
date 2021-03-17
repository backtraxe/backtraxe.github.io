# 数据结构与算法


数据结构，是抽象的表示数据的方式；算法，则是计算的一系列有效、通用的步骤。算法与数据结构是程序设计中相辅相成的两个方面，是计算机学科的重要基石。

<!--more-->

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



## 排序

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

