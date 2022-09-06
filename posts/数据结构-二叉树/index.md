# 数据结构-二叉树


<!--more-->

## 基础

满二叉树：一个高度为 d 的二叉树，有 $2^d-1$ 个节点。即除叶节点外，每个节点都有两个孩子，即节点的出度只为 0 或 2。

完全二叉树：只有最后一层可能未满，且节点严格从左往右排列。即出度为 1 的节点一定只有左孩子；若某节点出度小于 2，则其右边的节点出度为 0。

> - 二叉树第 $i$ 层最多有 $2^{i-1}$ 个节点。
> - 高度为 $d$ 的二叉树最多有 $2^d-1$ 个节点。

## 定义

```java
class TreeNode {
    public int val;
    public TreeNode left, right;

    public TreeNode() {}

    public TreeNode(int val) {
        this.val = val;
    }

    public TreeNode(int val, TreeNode left, TreeNode right) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}
```

```java
//       3
//     /   \
//    1     5
//     \   / \
//      2 4   6
TreeNode n2 = new TreeNode(2);
TreeNode n1 = new TreeNode(1, null, n2);
TreeNode n4 = new TreeNode(4);
TreeNode n6 = new TreeNode(6);
TreeNode n5 = new TreeNode(5, n4, n6);
TreeNode n3 = new TreeNode(3, n1, n5);
```

## 遍历

### 前序遍历

- 递归：根左右

```java
List<Integer> preorder(TreeNode root) {
    List<Integer> ans = new ArrayList<>();
    dfs(root, ans);
    return ans;
}

void dfs(TreeNode root, List<Integer> ans) {
    if (root == null) return;
    ans.add(root.val);
    dfs(root.left, ans);
    dfs(root.right, ans);
}
```

- 递归：带结点深度

```java
List<Integer> preorder(TreeNode root) {
    List<Integer> ans = new ArrayList<>();
    dfs(root, 0, ans);
    return ans;
}

void dfs(TreeNode root, int depth, List<Integer> ans) {
    if (root == null) return;
    ans.add(root.val);
    dfs(root.left, depth + 1, ans);
    dfs(root.right, depth + 1, ans);
}
```

- 非递归：栈根右左

```java
List<Integer> preorder(TreeNode root) {
    List<Integer> ans = new ArrayList<>();
    if (root == null) return ans;
    Deque<TreeNode> st = new ArrayDeque<>();
    st.push(root);
    while (!st.isEmpty()) {
        TreeNode p = st.pop();
        ans.add(p.val);
        if (p.right != null) st.push(p.right);
        if (p.left != null) st.push(p.left);
    }
    return ans;
}
```

### 中序遍历

- 递归：左根右

```java
List<Integer> inorder(TreeNode root) {
    List<Integer> ans = new ArrayList<>();
    dfs(root, ans);
    return ans;
}

void dfs(TreeNode root, List<Integer> ans) {
    if (root == null) return;
    dfs(root.left, ans);
    ans.add(root.val);
    dfs(root.right, ans);
}
```

- 递归：带结点深度

```java
List<Integer> inorder(TreeNode root) {
    List<Integer> ans = new ArrayList<>();
    dfs(root, 0, ans);
    return ans;
}

void dfs(TreeNode root, int depth, List<Integer> ans) {
    if (root == null) return;
    dfs(root.left, depth + 1, ans);
    ans.add(root.val);
    dfs(root.right, depth + 1, ans);
}
```

- 非递归：有左则左，无左则根，有右继续

```java
List<Integer> inorder(TreeNode root) {
    List<Integer> ans = new ArrayList<>();
    if (root == null) return ans;
    Deque<TreeNode> st = new ArrayDeque<>();
    while (!st.isEmpty() || root != null) {
        while (root != null) {
            st.push(root);
            root = root.left;
        }
        TreeNode p = st.pop();
        ans.add(p.val);
        root = p.right;
    }
    return ans;
}
```

### 后序遍历

- 递归：左右根

```java
List<Integer> postorder(TreeNode root) {
    List<Integer> ans = new ArrayList<>();
    dfs(root, ans);
    return ans;
}

void dfs(TreeNode root, List<Integer> ans) {
    if (root == null) return;
    dfs(root.left, ans);
    dfs(root.right, ans);
    ans.add(root.val);
}
```

- 递归：带结点深度

```java
List<Integer> postorder(TreeNode root) {
    List<Integer> ans = new ArrayList<>();
    dfs(root, 0, ans);
    return ans;
}

void dfs(TreeNode root, int depth, List<Integer> ans) {
    if (root == null) return;
    dfs(root.left, depth + 1, ans);
    dfs(root.right, depth + 1, ans);
    ans.add(root.val);
}
```

- 非递归：栈根左右，最后逆序

```java
List<Integer> postorder(TreeNode root) {
    List<Integer> ans = new ArrayList<>();
    if (root == null) return ans;
    Deque<TreeNode> st = new ArrayDeque<>();
    st.push(root);
    while (!st.isEmpty()) {
        TreeNode p = st.pop();
        ans.add(p.val);
        if (p.left != null) st.push(p.left);
        if (p.right != null) st.push(p.right);
    }
    // Collections.reverse(ans);
    for (int i = 0, j = ans.size() - 1; i < j; i++, j--) {
        // Collections.swap(ans, i, j);
        int temp = ans.get(i);
        ans.set(i, ans.get(j));
        ans.set(j, temp);
    }
    return ans;
}
```

- 非递归：栈根左右，首部添加，无需逆序

```java
List<Integer> postorder(TreeNode root) {
    List<Integer> ans = new LinkedList<>();
    if (root == null) return ans;
    Deque<TreeNode> st = new ArrayDeque<>();
    st.push(root);
    while (!st.isEmpty()) {
        TreeNode p = st.pop();
        ans.add(0, p.val);
        if (p.left != null) st.push(p.left);
        if (p.right != null) st.push(p.right);
    }
    return ans;
}
```

- 非递归：记录上个结点

```java
List<Integer> postorder(TreeNode root) {
    List<Integer> ans = new ArrayList<>();
    if (root == null) return ans;
    Deque<TreeNode> st = new ArrayDeque<>();
    TreeNode prev = null;
    while (!st.isEmpty() || root != null) {
        while (root != null) {
            st.push(root);
            root = root.left;
        }
        TreeNode p = st.peek();
        if (p.right != null && p.right != prev) {
            root = p.right;
        } else {
            ans.add(p.val);
            st.pop();
        }
        prev = p;
    }
    return ans;
}
```

### 层序遍历

- BFS

```java
List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> ans = new ArrayList<>();
    if (root == null) return ans;
    Queue<TreeNode> que = new ArrayDeque<>();
    que.offer(root);
    int depth = 0;
    while (!que.isEmpty()) {
        int size = que.size();
        List<Integer> level = new ArrayList<>();
        while (size-- > 0) {
            root = que.poll();
            level.add(root.val);
            if (root.left != null) que.offer(root.left);
            if (root.right != null) que.offer(root.right);
        }
        depth++;
        ans.add(level);
    }
    return ans;
}
```

- DFS

```java
List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> ans = new ArrayList<>();
    dfs(root, 0, ans);
    return ans;
}

void dfs(TreeNode root, int depth, List<List<Integer>> ans) {
    if (root == null) return;
    if (depth == ans.size()) ans.add(new ArrayList<>());
    ans.get(depth).add(root.val);
    dfs(root.left, depth + 1, ans);
    dfs(root.right, depth + 1, ans);
}
```

## 二叉搜索树

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

### 🟨寻找重复的子树

- 哈希表
- 二叉树序列化

[652. 寻找重复的子树](https://leetcode.cn/problems/find-duplicate-subtrees/)

```java
class Solution {
    int id = 1; // 自增 id
    HashMap<String, Integer> map = new HashMap<>(); // 二叉树序列号 -> 自增 id
    HashMap<String, Integer> cnt = new HashMap<>(); // 二叉树序列号 -> 二叉树数量
    List<TreeNode> ans = new ArrayList<>();

    public List<TreeNode> findDuplicateSubtrees(TreeNode root) {
        dfs(root);
        return ans;
    }

    int dfs(TreeNode root) {
        if (root == null) return 0;
        // 二叉树序列化值
        String code = root.val + "/" + dfs(root.left) + "/" + dfs(root.right);
        // 将相同结构的二叉树映射为相同值，用于减小 code 的长度
        int nodeId = map.computeIfAbsent(code, k -> id++);
        cnt.put(code, cnt.getOrDefault(code, 0) + 1);
        // 第二次出现
        if (cnt.get(code) == 2) ans.add(root);
        return nodeId;
    }
}
```

## 参考

1. [「代码随想录」帮你对二叉树不再迷茫，彻底吃透前中后序递归法（递归三部曲）和迭代法（不统一写法与统一写法） - 二叉树的后序遍历 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-postorder-traversal/solution/bang-ni-dui-er-cha-shu-bu-zai-mi-mang-che-di-chi-t/)
1. [一篇文章解决所有二叉树路径问题（问题分析+分类模板+题目剖析） - 最长同值路径 - 力扣（LeetCode）](https://leetcode.cn/problems/longest-univalue-path/solution/yi-pian-wen-zhang-jie-jue-suo-you-er-cha-94j7/)

