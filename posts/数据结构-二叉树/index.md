# 数据结构-二叉树


<!--more-->

### 基础

满二叉树：一个高度为 d 的二叉树，有 $2^d-1$ 个节点。即除叶节点外，每个节点都有两个孩子，即节点的出度只为 0 或 2。

完全二叉树：只有最后一层可能未满，且节点严格从左往右排列。即出度为 1 的节点一定只有左孩子；若某节点出度小于 2，则其右边的节点出度为 0。

> - 二叉树第 $i$ 层最多有 $2^{i-1}$ 个节点。
> - 高度为 $d$ 的二叉树最多有 $2^d-1$ 个节点。

### 定义

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

### 二叉搜索树

Binary Search Tree，BST

#### 性质

- 左子树结点均小于根结点，右子树结点均大于根结点
- 左右子树均为二叉搜索树
- 中序遍历结果为升序

#### 插入

小左大右

{{< admonition tip "递归" false >}}
```java
TreeNode insert(TreeNode root, int val) {
    if (root == null) {
        return new TreeNode(val);
    } else if (root.val > val) {
        root.left = insert(root.left, val);
    } else if (root.val < val) {
        root.right = insert(root.right, val);
    }
    // 跳过相同值
    return root;
}
```
{{< /admonition >}}

{{< admonition tip "迭代" false >}}
```java
TreeNode insert(TreeNode root, int val) {
    if (root == null) {
        return new TreeNode(val);
    }
    TreeNode p = root;
    while (p != null) {
        if (p.val > val) {
            if (p.left == null) {
                p.left = new TreeNode(val);
                break;
            }
            p = p.left;
        } else if (p.val < val) {
            if (p.right == null) {
                p.right = new TreeNode(val);
                break;
            }
            p = p.right;
        } else {
            // 跳过相同值
            break;
        }
    }
    return root;
}
```
{{< /admonition >}}

#### 删除

- 若删除结点为叶结点，直接删除。
- 若删除结点为非叶结点，将左子树最大结点或者右子树最小结点移至当前位置。

{{< admonition tip "递归" false >}}
```java

```
{{< /admonition >}}

{{< admonition tip "迭代" false >}}
```java

```
{{< /admonition >}}

