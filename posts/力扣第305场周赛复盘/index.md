# 力扣第305场周赛复盘


[第 304 场周赛](https://leetcode.cn/contest/weekly-contest-305/)

<!--more-->

## 复盘

- 全国排名：556 / 7426（8.08%）
- 全球排名：1309 / 26877（4.64%）
- 分数变化：2095 + 17 = 2112
- 完成时间：42m20s（+5m）
- 题目1
    - 难度：
    - 顺序：1
    - 用时：5m46s
    - 错误：0
- 题目2
    - 难度：
    - 顺序：2
    - 用时：7m28s
    - 错误：0
- 题目3
    - 难度：
    - 顺序：3
    - 用时：16m28s
    - 错误：1
        - dp 初始化错误
- 题目4
    - 难度：
    - 顺序：4
    - 用时：17m38s
    - 错误：0

## 算术三元组的数目

[6136. 算术三元组的数目]()

```java
class Solution {
    public int arithmeticTriplets(int[] nums, int diff) {
        HashMap<Integer, Integer> map = new HashMap<>();
        int n = nums.length;
        for (int i = 0; i < n; i++) map.put(nums[i], i);
        int ans = 0;
        for (int i = 1; i + 1 < n; i++) {
            int prev = map.getOrDefault(nums[i] - diff, n);
            int next = map.getOrDefault(nums[i] + diff, -1);
            if (prev < i && next > i) ans++;
        }
        return ans;
    }
}
```

## 受限条件下可到达节点的数目

[6139. 受限条件下可到达节点的数目]()

```java
class Solution {
    int ans = 0;

    public int reachableNodes(int n, int[][] edges, int[] restricted) {
        List<Integer>[] graph = new ArrayList[n];
        for (int i = 0; i < n; i++)
            graph[i] = new ArrayList<>();
        for (int[] edge : edges) {
            graph[edge[0]].add(edge[1]);
            graph[edge[1]].add(edge[0]);
        }
        HashSet<Integer> res = new HashSet<>();
        for (int x : restricted) res.add(x);
        boolean[] vis = new boolean[n];
        dfs(graph, 0, res, vis);
        return ans;
    }

    void dfs(List<Integer>[] graph, int u, HashSet<Integer> res, boolean[] vis) {
        vis[u] = true;
        ans++;
        for (int v : graph[u]) {
            if (!res.contains(v) && !vis[v])
                dfs(graph, v, res, vis);
        }
    }
}
```

## 检查数组是否存在有效划分

[6137. 检查数组是否存在有效划分]()

```java
class Solution {
    public boolean validPartition(int[] nums) {
        int n = nums.length;
        // dp[i] 表示前 i 个元素是否存在有效划分
        boolean[] dp = new boolean[n + 1];
        dp[0] = true;
        dp[2] = nums[0] == nums[1];
        for (int i = 2; i < n; i++) {
            dp[i + 1] = dp[i - 1] && nums[i] == nums[i - 1] ||
                        dp[i - 2] && nums[i] == nums[i - 1] && nums[i] == nums[i - 2] ||
                        dp[i - 2] && nums[i] - 1 == nums[i - 1] && nums[i] - 2 == nums[i - 2];
        }
        return dp[n];
    }
}
```

## 最长理想子序列

[6138. 最长理想子序列]()

```java
class Solution {
    public int longestIdealString(String s, int k) {
        int[] dp = new int[26];
        for (char c : s.toCharArray()) {
            int x = c - 'a';
            int maxLen = 0;
            for (int i = Math.max(0, x - k); i <= Math.min(25, x + k); i++)
                maxLen = Math.max(maxLen, dp[i]);
            d[x] = maxLen + 1;
        }
        int ans = 0;
        for (int x : dp) ans = Math.max(ans, x);
        return ans;
    }
}
```

## 总结

- 考察 dp

