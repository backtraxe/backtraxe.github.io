# 力扣第304场周赛复盘


[第 304 场周赛](https://leetcode.cn/contest/weekly-contest-304/)

<!--more-->

## 复盘

- 全国排名：596 / 7372（8.08%）
- 全球排名：1246 / 26877（4.64%）
- 分数变化：2095 + 17 = 2112
- 完成时间：1h18s（+5m）
- 题目1
    - 难度：
    - 顺序：1
    - 用时：5m27s
    - 错误：0
- 题目2
    - 难度：
    - 顺序：4
    - 用时：6m41s
    - 错误：0
- 题目3
    - 难度：
    - 顺序：2
    - 用时：31m22s
    - 错误：0
- 题目4
    - 难度：
    - 顺序：3
    - 用时：11m48s
    - 错误：1
        - WA：把`vis`误用为`onPath`

## 使数组中所有元素都等于零

[6132. 使数组中所有元素都等于零](https://leetcode.cn/problems/make-array-zero-by-subtracting-equal-amounts/)

```java
class Solution {
    public int minimumOperations(int[] nums) {
        TreeSet<Integer> set = new TreeSet<>();
        // 去重
        for (int x : nums)
            set.add(x);
        int ans = 0;
        int sub = 0;
        for (int x : set) {
            if (x == 0) continue;
            x -= sub;
            ans++;
            sub += x;
        }
        return ans;
    }
}
```

## 分组的最大数量

[6133. 分组的最大数量](https://leetcode.cn/problems/maximum-number-of-groups-entering-a-competition/)

```java
class Solution {
    public int maximumGroups(int[] grades) {
        int n = grades.length;
        int ans = (int) Math.sqrt(n * 2);
        if (ans * (ans + 1) / 2 > n)
            ans--;
        return ans;
    }
}
```

## 找到离给定两个节点最近的节点

[6134. 找到离给定两个节点最近的节点](https://leetcode.cn/problems/find-closest-node-to-given-two-nodes/)

```java
class Solution {
    int[] edges;
    int[] dis;
    boolean[] vis;
    int ans = -1;
    int minDis = Integer.MAX_VALUE;

    public int closestMeetingNode(int[] edges, int node1, int node2) {
        this.edges = edges;
        int n = edges.length;
        dis = new int[n];
        Arrays.fill(dis, -1);
        vis = new boolean[n];
        dfs1(node1, 0);
        vis = new boolean[n];
        dfs2(node2, 0);
        return ans;
    }

    void dfs1(int node, int step) {
        vis[node] = true;
        dis[node] = step;
        if (edges[node] != -1 && !vis[edges[node]])
            dfs1(edges[node], step + 1);
    }

    void dfs2(int node, int step) {
        vis[node] = true;
        if (dis[node] != -1) {
            int d = Math.max(step, dis[node]);
            if (d < minDis) {
                minDis = d;
                ans = node;
            } else if (d == minDis && ans > node) {
                ans = node;
            }
        }
        if (edges[node] != -1 && !vis[edges[node]])
            dfs2(edges[node], step + 1);
    }
}
```

## 图中的最长环

[6135. 图中的最长环](https://leetcode.cn/problems/longest-cycle-in-a-graph/)

```java
class Solution {
    int[] edges;
    int[] vis;
    boolean[] onPath;
    int ans = -1;

    public int longestCycle(int[] edges) {
        this.edges = edges;
        int n = edges.length;
        vis = new int[n];
        onPath = new boolean[n];
        for (int i = 0; i < n; i++)
            if (vis[i] == 0)
                dfs(i, 1);
        return ans;
    }

    void dfs(int node, int step) {
        onPath[node] = true;
        vis[node] = step;
        if (edges[node] != -1) {
            if (onPath[edges[node]])
                ans = Math.max(ans, step - vis[edges[node]] + 1);
            if (vis[edges[node]] == 0)
                dfs(edges[node], step + 1);
        }
        onPath[node] = false;
    }
}
```

## 总结

- 考察图论算法：环检测。
- 变相考察 DFS 和 BFS。

