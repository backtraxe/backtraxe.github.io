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

## 实战

### 🟩二叉树的所有路径

[257. 二叉树的所有路径](https://leetcode.cn/problems/binary-tree-paths/)

```java
class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> ans = new ArrayList<>();
        char[] path = new char[512];
        dfs(root, path, 0, ans);
        return ans;
    }

    void dfs(TreeNode root, char[] path, int idx, List<String> allPath) {
        if (root == null) return;
        String s = Integer.toString(root.val);
        for (int i = 0; i < s.length(); i++) path[idx++] = s.charAt(i);
        if (root.left == null && root.right == null) {
            allPath.add(String.valueOf(path, 0, idx));
        } else {
            path[idx++] = '-';
            path[idx++] = '>';
            dfs(root.left, path, idx, allPath);
            dfs(root.right, path, idx, allPath);
        }
    }
}
```

### 🟨求和路径

[面试题 04.12. 求和路径](https://leetcode.cn/problems/paths-with-sum-lcci/)

```java
class Solution {
    public int pathSum(TreeNode root, int sum) {
        if (root == null) return 0;
        return dfs(root, sum) + pathSum(root.left, sum) + pathSum(root.right, sum);
    }

    int dfs(TreeNode root, int sum) {
        // 查找从 root 开始的路径
        if (root == null) return 0;
        sum -= root.val;
        return (sum == 0 ? 1 : 0) + dfs(root.left, sum) + dfs(root.right, sum);
    }
}
```

### 🟩路径总和

[112. 路径总和](https://leetcode.cn/problems/path-sum/)

### 🟨路径总和 II

[113. 路径总和 II](https://leetcode.cn/problems/path-sum-ii/)

### 🟨路径总和 III

[437. 路径总和 III](https://leetcode.cn/problems/path-sum-iii/)

### 🟨从叶结点开始的最小字符串

[988. 从叶结点开始的最小字符串](https://leetcode.cn/problems/smallest-string-starting-from-leaf/)

### 🟥二叉树中的最大路径和

[124. 二叉树中的最大路径和](https://leetcode.cn/problems/binary-tree-maximum-path-sum/)

### 🟨最长同值路径

[687. 最长同值路径](https://leetcode.cn/problems/longest-univalue-path/)

```java
class Solution {
    public int longestUnivaluePath(TreeNode root) {
        return dfs(root)[0];
    }

    int[] dfs(TreeNode root) {
        // { 最长同值路径的长度，以根结点为端点的同值路径的长度 }
        if (root == null) return new int[] { 0, 0 };
        int[] leftAns = dfs(root.left);
        int[] rightAns = dfs(root.right);
        // 最长同值路径可能在左子树上，也可能在右子树上，取两者的更大值
        int maxLen = Math.max(leftAns[0], rightAns[0]); // 最长同值路径的长度
        int rootLen = 0; // 以 root 结点为端点的同值路径的长度
        int leftVal = root.left == null ? Integer.MAX_VALUE : root.left.val;
        int rightVal = root.right == null ? Integer.MAX_VALUE : root.right.val;
        if (root.val == leftVal && root.val == rightVal) {
            // 当左孩子、右孩子和 root 的值相同时，可以形成一条经过根结点的同值路径
            // 此时，以根结点为端点的同值路径只能选择左右子树中的更长的路径
            maxLen = Math.max(maxLen, 2 + leftAns[1] + rightAns[1]);
            rootLen = 1 + Math.max(leftAns[1], rightAns[1]);
        } else if (root.val == leftVal) {
            // 当左孩子和 root 的值相同时，以左孩子为端点的同值路径可以添加 root 结点
            rootLen = 1 + leftAns[1];
        } else if (root.val == rightVal) {
            // 当右孩子和 root 的值相同时，以右孩子为端点的同值路径可以添加 root 结点
            rootLen = 1 + rightAns[1];
        }
        return new int[] { Math.max(maxLen, rootLen), rootLen };
    }
}
```

### 🟩二叉树的直径

[543. 二叉树的直径](https://leetcode.cn/problems/diameter-of-binary-tree/)

```java

```

## 参考

1. [一篇文章解决所有二叉树路径问题（问题分析+分类模板+题目剖析） - 最长同值路径 - 力扣（LeetCode）](https://leetcode.cn/problems/longest-univalue-path/solution/yi-pian-wen-zhang-jie-jue-suo-you-er-cha-94j7/)

