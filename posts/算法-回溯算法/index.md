# 算法-回溯算法


<!--more-->

## 排列 & 组合 & 子集

### 无重 & 不放回

- 子集

```java
static List<List<Integer>> subsets(int[] nums) {
    // 返回无重数组的所有子集
    List<List<Integer>> ans = new ArrayList<>();
    LinkedList<Integer> set = new LinkedList<>();
    dfs(nums, 0, set, ans);
    return ans;
}

static void dfs(int[] nums, int start, List<List<Integer>> ans, LinkedList<Integer> set) {
    ans.add(new LinkedList<>(set));
    for (int i = start; i < nums.length; i++) {
        set.add(nums[i]);
        dfs(nums, i + 1);
        set.pollLast();
    }
}
```

- 组合

```java
static List<List<Integer>> combinations(int n, int k) {
    // 返回 0-n 中所有 k 个数构成的组合
    List<List<Integer>> ans = new ArrayList<>();
    LinkedList<Integer> set = new LinkedList<>();
    dfs(n, k, 0, ans, set);
    return ans;
}

static void dfs(int n, int k, int start, List<List<Integer>> ans, LinkedList<Integer> set) {
    if (set.size() == k) {
        ans.add(new LinkedList<>(set));
        return;
    }
    for (int i = start; i <= n; i++) {
        set.add(i);
        dfs(n, k, i + 1, ans, set);
        set.pollLast();
    }
}
```

- 排列

```java
static List<List<Integer>> permutations(int[] nums) {
    // 返回无重数组的全部排列
    List<List<Integer>> ans = new ArrayList<>();
    dfs(nums, ans, new LinkedList<>(), new boolean[nums.length]);
    return ans;
}

static void dfs(int[] nums, List<List<Integer>> ans, LinkedList<Integer> set, boolean[] vis) {
    int n = nums.length;
    if (set.size() == n) {
        ans.add(new LinkedList<>(set));
        return;
    }
    for (int i = 0; i < n; i++) {
        // 防止重复选
        if (vis[i]) continue;
        vis[i] = true;
        set.add(nums[i]);
        dfs(nums, ans, set, vis);
        set.pollLast();
        vis[i] = false;
    }
}
```

### 无重 & 有放回

- 组合

```java
List<List<Integer>> ans = new ArrayList<>();
LinkedList<Integer> set = new LinkedList<>();

List<List<Integer>> combinationSum(int[] candidates, int target) {
    Arrays.sort(candidates);
    dfs(candidates, target, 0, 0);
    return ans;
}

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

- 排列

```java
List<List<Integer>> ans = new ArrayList<>();
LinkedList<Integer> set = new LinkedList<>();

List<List<Integer>> permuteDup(int[] nums) {
    dfs(nums);
    return ans;
}

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

### 有重 & 不放回

- 子集

```java
List<List<Integer>> ans = new ArrayList<>();
LinkedList<Integer> set = new LinkedList<>();

List<List<Integer>> subsetsWithDup(int[] nums) {
    Arrays.sort(nums);
    dfs(nums, 0);
    return ans;
}

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

- 组合

```java
List<List<Integer>> ans = new ArrayList<>();
LinkedList<Integer> set = new LinkedList<>();

List<List<Integer>> combinationSum2(int[] candidates, int target) {
    Arrays.sort(candidates);
    dfs(candidates, target, 0, 0);
    return ans;
}

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

- 排列

```java
List<List<Integer>> ans = new ArrayList<>();
LinkedList<Integer> set = new LinkedList<>();
boolean[] vis;

List<List<Integer>> permuteUnique(int[] nums) {
    vis = new boolean[nums.length];
    Arrays.sort(nums);
    dfs(nums);
    return ans;
}

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

