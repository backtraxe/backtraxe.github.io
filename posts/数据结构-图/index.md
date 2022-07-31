# 数据结构-图


<!--more-->

## 1.术语表

图（graph）

**边（edge）**：连接两个顶点。
- 自环：起点和终点是同一个顶点的边。
- 平行边：连接同一对顶点的两条边。

边是否存在方向：
- 有向图：边有方向。
- 无向图：边无方向。

是否有环：
- 无环图：不存在环的图。

顶点（vertex）

**度（degree）**：顶点连接的边的数量。
- 入度（indegree）：**终点**是当前顶点的**有向边**的数量（到达当前顶点）。
- 出度（outdegree）：**起点**是当前顶点的**有向边**的数量（从当前顶点出发）。

**路径（path）**：由边连接的一系列顶点。
- 简单路径：无重复顶点的路径。

**环（loop）**：起点和终点相同的路径。
- 简单环：无重复顶点的环。

是否存在平行边：
- 简单图：无平行边的图。
- 多重图：存在平行边的图。

子图：边和连接的顶点的子集。

**连通图**：任意一个顶点都存在路径到达另一个任意顶点。
- 连通子图：
- 极大连通子图：

**二分图**：能将所有顶点分为两个部分的图，每条边的两个顶点分别属于不同的部分。

<br>

## 2.图的存储

### 2.1 邻接矩阵

```java
// mat[u][v] == true 表示顶点 u 到顶点 v 存在一条有向边，u 是起点，v 是终点
boolean[][] mat = new boolean[n][n];

// mat[u][v] == w 表示顶点 u 到顶点 v 的有向边的边权 w
// 可将 mat[u][v] 赋值为 0 或 Integer.MAX_VALUE 表示 u 和 v 之间无边
int[][] mat = new int[n][n];
```

- 优点：可快速判断两个顶点是否相邻
- 缺点：占用空间大

<br>

### 2.2 邻接表

```java
// graph[u] 存储从顶点 u 开始的所有边的终点集合
// graph[u].get(i) == v 表示顶点 u 开始的第 i 条边的终点 v
List<Integer>[] graph = new ArrayList[n];
// graph[u].get(i)[0] == v, graph[u].get(i)[1] == w 表示顶点 u 开始的第 i 条边的终点 v，边权 w
List<int[]>[] graph = new ArrayList[n];
for (int i = 0; i < n; i++)
    graph[i] = new ArrayList<>();

// 快速判断两个顶点是否相邻
Set<Integer>[] graph = new HashSet[n];
Set<int[]>[] graph = new HashSet[n];
for (int i = 0; i < n; i++)
    graph[i] = new HashSet<>();

// 快速判断两个顶点是否相邻，从顶点 u 开始的所有边的终点集合有序
Set<Integer>[] graph = new TreeSet[n];
Set<int[]>[] graph = new TreeSet[n];
for (int i = 0; i < n; i++)
    graph[i] = new TreeSet<>();
```

- 优点：占用空间小
- 缺点：无法快速判断两个顶点是否相邻（用 Set 存储可解决）

<br>

### 2.3 其他

```java
// 1.邻接表（无边权）
// adjList1[u] 表示顶点 u 的相邻顶点
List<Integer>[] adjList1;

// 2.邻接表 有边权
// adjList2[u] = { {v1, w1}, {v2, w2} ... } 表示顶点 u 的相邻顶点
List<int[]>[] adjList2;

// 3.邻接表（无边权，可快速查询某顶点是否相邻）
// adjSet[u] 表示顶点 u 的相邻顶点
TreeSet<Integer>[] adjSet;

// 4.邻接矩阵
// adjMat[u][v] 表示顶点 u 到 顶点 v 的边的权值。
int[][] adjMat;

// 5.边集数组
// edges[i] = { u, v, w } 表示第 i 条边，从顶点 u 到 顶点 v，权值为 w。
int[][] edges;
```

- 邻接表
    - 优点：占用的空间少
    - 缺点：无法快速判断两个节点是否相邻
    - 适用于稀疏图
- 邻接矩阵
    - 优点：占用的空间多
    - 缺点：可以快速判断两个节点是否相邻
    - 适用于稠密图

#### 链式邻接表

**实现**

```java
// 边的定义
class Edge {
    // 起点，终点，边权
    int u, v, w;
}

// 边集数组
// edges[j] 存储第 j 条边的 { 起点 u，终点 v，边权 w }
Edge[] edges;
// 或者 int[][] edges;

// 表头数组
// adjEdges[u] 存储 u 点的所有出边的编号
List<Integer>[] adjEdges;
```

**可视化**

```text
输入：
6 5
4 3 90
1 4 30
5 6 60
1 5 20
5 2 70
```

```text
边集数组：
0 { 4, 3, 90 }
1 { 3, 4, 90 }
2 { 1, 4, 30 }
3 { 4, 1, 30 }
4 { 5, 6, 60 }
5 { 6, 5, 60 }
6 { 1, 5, 20 }
7 { 5, 1, 20 }
8 { 5, 2, 70 }
9 { 2, 5, 70 }
```

```text
表头数组：
1 { 2, 6 }
2 { 9 }
3 { 1 }
4 { 0, 3 }
5 { 4, 7, 8 }
6 { 5 }
```

**特点**

- 空间复杂度：$O(n+m)$
- 适用于各种图。
- 能够处理反向边，边的编号与 1 异或得到反向边。

#### 链式前向星

**实现**

```java
// 边的定义
class Edge {
    // 终点，边权，下一条边的编号
    int v, w, ne;
}

// 边集数组
// edges[j] 存储第 j 条边的 { 终点 v，边权 w，下一条边的编号 ne }
Edge[] edges;
// 或者 int[][] edges;

// 表头数组
// firstEdge[u] 存储 u 点的第一条出边的编号
int[] firstEdge;

// 添加边
void addEdge(int i, int u, int v, int w) {
    Edge e = new Edge();
    e.v = v;
    e.w = w;
    // 头插法
    e.ne = firstEdge[u];
    edges[i] = e;
    firstEdge[u] = i;
}
```

**可视化**

```text
输入：
6 5
4 3 90
1 4 30
5 6 60
1 5 20
5 2 70
```

```text
边集数组：
0 { 3, 90 }
1 { 3, 4, 90 }
2 { 1, 4, 30 }
3 { 4, 1, 30 }
4 { 5, 6, 60 }
5 { 6, 5, 60 }
6 { 1, 5, 20 }
7 { 5, 1, 20 }
8 { 5, 2, 70 }
9 { 2, 5, 70 }
```

```text
表头数组：
1 { 2, 6 }
2 { 9 }
3 { 1 }
4 { 0, 3 }
5 { 4, 7, 8 }
6 { 5 }
```

**特点**

- 空间复杂度：$O(n+m)$

<br>

## 3.图的遍历

#### 3.1 深度优先搜索（DFS）

Depth First Search

```java
void traverse(Graph graph) {
    int n = graph.length;
    boolean[] vis = new boolean[n];
    for (int u = 0; u < n; u++)
        dfs(graph, u, vis);
}

void dfs(Graph graph, int u, boolean[] vis) {
    vis[u] = true;
    // 前序
    for (int v : graph[u]) {
        // 树枝
        if (!vis[v])
            dfs(graph, v, vis);
    }
    // 后序
}
```

<br>

#### 3.2 广度优先搜索（BFS）

Breadth First Search

- BFS

```java
void traverse(Graph graph) {
    int n = graph.length;
    boolean[] vis = new boolean[n];
    for (int u = 0; u < n; u++)
        bfs(graph, u, vis);
}

void bfs(Graph graph, int start, boolean[] vis) {
    Queue<Integer> queue = new Queue<>();
    queue.offer(start);
    vis[start] = true;
    int step = 0;
    while (!queue.isEmpty()) {
        int size = queue.size();
        while (size-- > 0) {
            int u = queue.poll();
            for (int v : graph[u]) {
                // 此处可添加结束条件
                if (!vis[v]) {
                    vis[v] = true;
                    queue.offer(v);
                }
            }
        }
        step++;
    }
}
```

- 双向 BFS

```java
void bbfs(Graph graph, int start, int end, boolean[] vis) {
    Set<Integer> que1 = new HashSet<>();
    Set<Integer> que2 = new HashSet<>();
    que1.add(start);
    vis[start] = true;
    que2.add(end);
    vis[end] = true;
    int step = 0;
    while (!que1.isEmpty() && !que2.isEmpty()) {
        if (que1.size() > que2.size()) {
            // 优先遍历顶点多的队列
            Set<Integer> que = que1;
            que1 = que2;
            que2 = que;
        }
        Set<Integer> que = new HashSet<>();
        int size = que1.size();
        while (size-- > 0) {
            int u = que1.poll();
            if ()
            for (int v : graph[u]) {
                if (!vis[v]) {
                    vis[v] = true;
                }
            }
        }
    }
}
```

<br>

## 4.环检测

若下个待访问的顶点已在当前路径上，则说明存在环。

[207. 课程表](https://leetcode.cn/problems/course-schedule/)

### 4.1 DFS：布尔数组表示顶点是否在当前路径上

```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        List<Integer>[] graph = new ArrayList[numCourses];
        for (int i = 0; i < numCourses; i++)
            graph[i] = new ArrayList<>();
        boolean[] vis = new boolean[numCourses];
        boolean[] onPath = new boolean[numCourses];
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

### 4.2 DFS：vis 数组中的不同值表示顶点的不同状态（未访问、访问中、已访问）。

```java
class Solution {
    List<List<Integer>> edges;
    int[] visited;
    boolean valid = true;

    public boolean canFinish(int numCourses, int[][] prerequisites) {
        edges = new ArrayList<>();
        for (int i = 0; i < numCourses; ++i)
            edges.add(new ArrayList<>());
        visited = new int[numCourses];
        for (int[] info : prerequisites)
            edges.get(info[1]).add(info[0]);
        for (int i = 0; i < numCourses && valid; ++i)
            if (visited[i] == 0)
                dfs(i);
        return valid;
    }

    public void dfs(int u) {
        visited[u] = 1; // 当前路径上
        for (int v: edges.get(u)) {
            if (visited[v] == 0) {
                dfs(v);
                if (!valid) return;
            } else if (visited[v] == 1) {
                valid = false;
                return;
            }
        }
        visited[u] = 2; // 已访问
    }
}
```

### 4.3 BFS：每次删除入度为 0 的顶点

```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        List<List<Integer>> graph = new ArrayList<>();
        for (int i = 0; i < numCourses; i++)
            graph.add(new ArrayList<>());
        int[] indegree = new int[numCourses]; // 入度
        for (int[] edge : prerequisites) {
            graph.get(edge[1]).add(edge[0]); // 方向无所谓
            indegree[edge[0]]++;
        }
        Queue<Integer> queue = new ArrayDeque<>();
        for (int i = 0; i < numCourses; i++)
            if (indegree[i] == 0) queue.offer(i);
        int visited = 0;
        while (!queue.isEmpty()) {
            visited++;
            int u = queue.poll();
            for (int v : edges.get(u)) {
                --indeg[v];
                if (indeg[v] == 0)
                    queue.offer(v);
            }
        }
        return visited == numCourses;
    }
}
```

<br>

## 5.拓扑排序

拓扑排序（Topological Sort）：得到有向图 $G$ 的顶点的一个排列，满足任意一条有向边 $(u,v)$，$u$ 在排列中都在 $v$ 前面，即相对顺序不变。

[210. 课程表 II](https://leetcode.cn/problems/course-schedule-ii/)

- DFS：

```java

```

- BFS：

```java

```

<br>

## 最小生成树

### 克鲁斯卡尔

```java
int[][] kruskal() {

}
```

<br>

### 普里姆

```java

```

<br>

## Flood Fill

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

