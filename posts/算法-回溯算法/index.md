# 算法-回溯算法


<!--more-->

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

### 排列&组合&子集问题

#### 无重复元素&元素不可重复选择

子集

```java
List<List<Integer>> ans = new ArrayList<>();
LinkedList<Integer> set = new LinkedList<>();

/**
 * @param nums 元素唯一的数组
 * @return 所有子集的集合
 */
List<List<Integer>> subsets(int[] nums) {
    dfs(nums, 0);
    return ans;
}

/**
 * @param nums 元素唯一的数组
 * @param start 可选择元素范围[start, nums.length)
 */
void dfs(int[] nums, int start) {
    // 前序添加
    ans.add(new LinkedList<>(set));
    for (int i = start; i < nums.length; i++) {
        set.add(nums[index]);
        dfs(nums, i + 1);
        // 回溯
        set.pollLast();
    }
}
```

组合

```java
List<List<Integer>> ans = new ArrayList<>();
LinkedList<Integer> set = new LinkedList<>();

/**
 * @param n 元素范围[1, n]
 * @param k 组合中元素个数
 * @return 所有长度为 k 的组合的集合
 */
List<List<Integer>> combine(int n, int k) {
    dfs(n, k, 0);
    return ans;
}

/**
 * @param n     元素范围[1, n]
 * @param k     组合中元素个数
 * @param start 可选择元素范围[start, n]
 */
void dfs(int n, int k, int start) {
    if (set.size() == k) {
        ans.add(new LinkedList<>(set));
        return;
    }
    for (int i = start; i <= n; i++) {
        set.add(i);
        dfs(n, k, i + 1);
        // 回溯
        set.pollLast();
    }
}
```

排列

```java {hl_lines=[26]}
List<List<Integer>> ans = new ArrayList<>();
LinkedList<Integer> set = new LinkedList<>();
boolean[] vis;

/**
 * @param nums 元素唯一的数组
 * @return 所有排列的集合
 */
List<List<Integer>> permute(int[] nums) {
    vis = new boolean[nums.length];
    dfs(nums);
    return ans;
}

/**
 * @param nums 元素唯一的数组
 */
void dfs(int[] nums) {
    int n = nums.length;
    if (set.size() == n) {
        ans.add(new LinkedList<>(set));
        return;
    }
    for (int i = 0; i < n; i++) {
        // 防止重复选
        if (!vis[i]) {
            vis[i] = true;
            set.add(nums[i]);
            dfs(nums);
            // 回溯
            set.pollLast();
            vis[i] = false;
        }
    }
}
```

#### 无重复元素&元素可以重复选择

组合

```java
List<List<Integer>> ans = new ArrayList<>();
LinkedList<Integer> set = new LinkedList<>();

/**
 * @param candidates 无重复元素的数组
 * @param target     目标和
 * @return 所有和为 target 的组合的集合（元素可重复）
 */
List<List<Integer>> combinationSum(int[] candidates, int target) {
    Arrays.sort(candidates);
    dfs(candidates, target, 0, 0);
    return ans;
}

/**
 * @param candidates 有序数组
 * @param target     目标和
 * @param start      可选择元素范围[start, candidates.length)
 * @param sum        当前元素和
 */
void dfs(int[] candidates, int target, int start, int sum) {
    if (sum == target) {
        ans.add(new LinkedList<>(set));
        return;
    }
    for (int i = start; i < candidates.length; i++) {
        // 剪枝
        if (sum + candidates[i] <= target) {
            set.add(candidates[i]);
            // 可选范围不变
            dfs(candidates, target, i, sum + candidates[i]);
            set.pollLast();
        }
    }
}
```

排列

```java
List<List<Integer>> ans = new ArrayList<>();
LinkedList<Integer> set = new LinkedList<>();

/**
 * @param nums 无重复元素的数组
 * @return 所有排列的集合（可重复选择元素）
 */
List<List<Integer>> permuteDup(int[] nums) {
    dfs(nums);
    return ans;
}

/**
 * @param nums 无重复元素的数组
 */
void dfs(int[] nums) {
    int n = nums.length;
    if (set.size() == n) {
        ans.add(new LinkedList<>(set));
        return;
    }
    for (int i = 0; i < n; i++) {
        set.add(nums[i]);
        dfs(nums);
        // 回溯
        set.pollLast();
    }
}
```

#### 有重复元素&元素不可重复选择

子集

```java
List<List<Integer>> ans = new ArrayList<>();
LinkedList<Integer> set = new LinkedList<>();

/**
 * @param nums 可能存在重复元素的数组
 * @return 所有非重复子集的集合
 */
List<List<Integer>> subsetsWithDup(int[] nums) {
    Arrays.sort(nums);
    dfs(nums, 0);
    return ans;
}

/**
 * @param nums  有序数组
 * @param start 可选择元素范围[start, nums.length)
 */
void dfs(int[] nums, int start) {
    ans.add(new LinkedList<>(set));
    for (int i = start; i < nums.length; i++) {
        // 剪枝：相同元素已经选过就不再重复选择
        if (i > start && nums[i - 1] == nums[i]) {
            continue;
        }
        set.add(nums[i]);
        dfs(nums, i + 1);
        // 回溯
        set.pollLast();
    }
}
```

组合

```java {hl_lines=[29]}
List<List<Integer>> ans = new ArrayList<>();
LinkedList<Integer> set = new LinkedList<>();

/**
 * @param candidates 可能存在重复元素的正数数组
 * @param target     目标和
 * @return 所有和为 target 的组合的集合
 */
List<List<Integer>> combinationSum2(int[] candidates, int target) {
    Arrays.sort(candidates);
    dfs(candidates, target, 0, 0);
    return ans;
}

/**
 * @param candidates 有序数组
 * @param target     目标和
 * @param start      可选择元素范围[start, candidates.length)
 * @param sum        当前元素和
 */
void dfs(int[] candidates, int target, int start, int sum) {
    if (sum == target) {
        ans.add(new LinkedList<>(set));
        return;
    }
    for (int i = start; i < candidates.length; i++) {
        // 剪枝1：相同元素已经选过就不再重复选择
        // 即相同元素只能连续选择，不能间隔选择
        if (i > start && candidates[i - 1] == candidates[i]) {
            continue;
        }
        // 剪枝2：只有加上当前元素后元素和不超过 target 才选择
        if (sum + candidates[i] <= target) {
            set.add(candidates[i]);
            dfs(candidates, target, i + 1, sum + candidates[i]);
            set.pollLast();
        }
    }
}
```

排列

```java {hl_lines=[29]}
List<List<Integer>> ans = new ArrayList<>();
LinkedList<Integer> set = new LinkedList<>();
boolean[] vis;

/**
 * @param nums 可能存在重复元素的数组
 * @return 所有排列的集合
 */
List<List<Integer>> permuteUnique(int[] nums) {
    vis = new boolean[nums.length];
    Arrays.sort(nums);
    dfs(nums);
    return ans;
}

/**
 * @param nums 有序数组
 */
void dfs(int[] nums) {
    int n = nums.length;
    if (set.size() == n) {
        ans.add(new LinkedList<>(set));
        return;
    }
    for (int i = 0; i < n; i++) {
        if (!vis[i]) {
            // 剪枝：相同元素已经选过就不再重复选择
            // 即相同元素只能连续选择，不能间隔选择
            if (i > 0 && !vis[i - 1] && nums[i - 1] == nums[i]) {
                continue;
            }
            vis[i] = true;
            set.add(nums[i]);
            dfs(nums);
            // 回溯
            set.pollLast();
            vis[i] = false;
        }
    }
}
```

## 实战

### 🟨所有可能的路径

[797. 所有可能的路径](https://leetcode.cn/problems/all-paths-from-source-to-target/)

```java
class Solution {
    public List<List<Integer>> allPathsSourceTarget(int[][] graph) {
        List<List<Integer>> ans = new ArrayList<>();
        dfs(graph, 0, new ArrayList<>(), ans);
        return ans;
    }

    void dfs(int[][] graph, int u, List<Integer> path, List<List<Integer>> ans) {
        path.add(u);
        if (u == graph.length - 1) ans.add(new ArrayList<>(path));
        else for (int v : graph[u]) dfs(graph, v, path, ans);
        path.remove(path.size() - 1);
    }
}
```

