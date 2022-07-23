# 力扣题解


<!--more-->

## 图论

### 拓扑排序

- [207. 课程表](https://leetcode.cn/problems/course-schedule/)

```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        List<Integer>[] graph = new ArrayList[numCourses];
        boolean[] vis = new boolean[numCourses];
        boolean[] onPath = new boolean[numCourses];
        for (int i = 0; i < numCourses; i++)
            graph[i] = new ArrayList<>();
        for (int[] course : prerequisites) // 建图
            graph[course[1]].add(course[0]);
        for (int u = 0; u < numCourses; u++) // DFS 遍历
            if (!vis[u] && dfs(graph, u, vis, onPath))
                return false;
        return true;
    }

    boolean dfs(List<Integer>[] graph, int u, boolean[] vis, boolean[] onPath) {
        // 返回是否存在环
        vis[u] = true;
        onPath[u] = true;
        for (int v : graph[u]) {
            if (onPath[v]) // 找到环
                return true;
            if (!vis[v] && dfs(graph, v, vis, onPath)) // 已找到环，剪枝
                return true;
        }
        onPath[u] = false; // 回溯
        return false;
    }
}
```

- []()

```java

```

