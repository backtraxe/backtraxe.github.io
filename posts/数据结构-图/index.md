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

#### 3.2 广度优先搜索（BFS）

Breadth First Search

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

#### 3.3 双向 BFS

```java

```

## 4.环检测

- DFS：若下个待访问的顶点在当前访问路径上，则说明存在环。
- BFS：若存在顶点未被访问，则说明存在环。

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

### 4.2 DFS：vis 数组表示顶点的不同状态（未访问、访问中、已访问）

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
            if (visited[v] == 0) { // 未访问
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
        // bfs
        Queue<Integer> queue = new ArrayDeque<>();
        for (int i = 0; i < numCourses; i++)
            if (indegree[i] == 0) // 入度为 0
                queue.offer(i);
        int visited = 0; // 已访问顶点数量
        while (!queue.isEmpty()) {
            visited++;
            int u = queue.poll();
            for (int v : graph.get(u)) {
                indegree[v]--;
                if (indegree[v] == 0) // 入度为 0
                    queue.offer(v);
            }
        }
        return visited == numCourses; // 若有顶点未访问则存在环
    }
}
```

## 5.拓扑排序

拓扑排序（Topological Sort）：得到有向图 $G$ 的顶点的一个排列，满足任意一条有向边 $(u,v)$，$u$ 在排列中都在 $v$ 前面，即相对顺序不变。

- DFS：后序添加顶点，然后逆序即可得到拓扑排序序列。（或者建图时颠倒每条边的起始顶点和结束顶点，则最后无需逆序）

```java
List<Integer> topoSort(List<Set<Integer>> graph) {
    // 存在环则返回 null
    int n = graph.size();
    List<Integer> topo = new ArrayList<>();
    int[] vis = new int[n];
    for (int u = 0; u < n; u++)
        if (vis[u] == 0 && dfs(graph, u, topo, vis))
            return null;
    return topo;
}

boolean dfs(List<Set<Integer>> graph, int u, List<Integer> topo, int[] vis) {
    // 返回有向图中是否存在环
    vis[u] = 1;
    for (int v : graph.get(u)) {
        if (vis[v] == 1) return true;
        if (vis[v] == 0 && dfs(graph, v, topo, vis)) return true;
    }
    topo.add(u);
    vis[u] = 2;
    return false;
}
```

- BFS：每次添加入度为 0 的顶点

```java
List<Integer> topoSort(List<Set<Integer>> graph) {
    // 存在环则返回 null
    int n = graph.size();
    int[] ind = new int[n];
    for (Set<Integer> u : graph)
        for (int v : u) ind[v]++;
    List<Integer> topo = new ArrayList<>();
    Queue<Integer> que = new ArrayDeque<>();
    for (int u = 0; u < n; u++)
        if (ind[u] == 0) que.offer(u);
    while (!que.isEmpty()) {
        int u = que.poll();
        topo.add(u);
        for (int v : graph.get(u))
            if (--ind[v] == 0) que.offer(v);
    }
    return topo.size() == n ? topo : null;
}
```

## 6.二分图

二分图：一个图的顶点可分割为两个互不相交的子集，且每条边的两个顶点都分属于这两个子集。

可以用两种颜色给顶点染色使得相邻顶点的颜色均不相同。

[785. 判断二分图](https://leetcode.cn/problems/is-graph-bipartite/submissions/)

### 6.1 DFS

```java
class Solution {
    public boolean isBipartite(int[][] graph) {
        int n = graph.length;
        int[] vis = new int[n];
        for (int u = 0; u < n; u++) // 非连通图
            if (vis[u] == 0 && !dfs(graph, u, vis, 1))
                return false;
        return true;
    }

    boolean dfs(int[][] graph, int u, int[] vis, int color) {
        vis[u] = color;
        for (int v : graph[u]) {
            if (vis[u] == vis[v]) return false; // 相邻结点颜色相同
            if (vis[v] == 0 && !dfs(graph, v, vis, -color)) // 两种颜色 1 -1
                return false; // 剪枝
        }
        return true;
    }
}
```

### 6.2 BFS

```java
class Solution {
    public boolean isBipartite(int[][] graph) {
        int n = graph.length;
        int[] vis = new int[n];
        for (int u = 0; u < n; u++) // 非连通图
            if (vis[u] == 0 && !bfs(graph, u, vis))
                return false;
        return true;
    }

    boolean bfs(int[][] graph, int i, int[] vis) {
        Queue<Integer> que = new ArrayDeque<>();
        que.offer(i);
        vis[i] = 1;
        while (!que.isEmpty()) {
            int size = que.size();
            while (size-- > 0) {
                int u = que.poll();
                for (int v : graph[u]) {
                    if (vis[u] == vis[v]) return false; // 相邻结点颜色相同
                    if (vis[v] == 0) {
                        vis[v] = -vis[u]; // 两种颜色 1 -1
                        que.offer(v);
                    }
                }
            }
        }
        return true;
    }
}
```

## 7.Flood Fill 算法

也称洪水扩散算法，从一个单元格向四周扩散。

[200. 岛屿数量](https://leetcode.cn/problems/number-of-islands/)

### 7.1 DFS

```java
class Solution {
    public int numIslands(char[][] grid) {
        int ans = 0;
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j] == '1') {
                    ans++;
                    dfs(grid, i, j, '1', '2');
                }
            }
        }
        return ans;
    }

    void dfs(char[][] mat, int x, int y, char v1, char v2) {
        if (x < 0 || x >= mat.length || y < 0 || y >= mat[0].length || mat[x][y] != v1) return;
        mat[x][y] = v2;
        dfs(mat, x - 1, y, v1, v2);
        dfs(mat, x, y + 1, v1, v2);
        dfs(mat, x + 1, y, v1, v2);
        dfs(mat, x, y - 1, v1, v2);
    }
}
```

### 7.2 BFS

```java
class Solution {
    int m, n;
    Queue<Integer> que = new ArrayDeque<>();
    int[][] dir = { { 0, 1 }, { 0, -1 }, { 1, 0 }, { -1, 0 } };

    public int numIslands(char[][] grid) {
        m = grid.length;
        n = grid[0].length;
        int ans = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '1') {
                    ans++;
                    bfs(grid, i, j, '1', '2');
                }
            }
        }
        return ans;
    }

    void bfs(char[][] mat, int x, int y, char v1, char v2) {
        if (mat[x][y] != v1) return;
        que.offer(x);
        que.offer(y);
        mat[x][y] = v2;
        while (!que.isEmpty()) {
            x = que.poll();
            y = que.poll();
            for (int[] d : dir) {
                int xx = x + d[0];
                int yy = y + d[1];
                if (xx >= 0 && xx < m && yy >= 0 && yy < n && mat[xx][yy] == v1) {
                    que.offer(xx);
                    que.offer(yy);
                    mat[xx][yy] = v2;
                }
            }
        }
    }
}
```

#### 保存坐标的几种方式

1. 数组保存。

```java
que.offer(new int[] { r, c });
int[] p = que.poll();
p[0];
p[1];
```

2. 分开保存。

```java
que.offer(r);
que.offer(c);
int x = que.poll();
int y = que.poll();
```

3. 数学计算。注意防止溢出。

```java
que.offer(r * n + c); // n 是列宽
int x = que.poll();
int y = x % n;
x /= n;
```

## 8.最小生成树

从一个 n 个顶点的连通图中生成包含 n 个顶点和 n - 1 条边，且边权之和最小的树。

### 8.1 Kruskal 算法

**步骤：**

1. 将图 $G=(V,E)$ 中的所有边按照长度由小到大进行排序，等长的边可以按任意顺序。
2. 初始化图 $G'=(V,\varnothing)$，从前向后扫描排序后的边，如果扫描到的边 $e$ 在 $G'$ 中连接了两个相异的连通块,则将它插入 $G'$ 中。
3. 最后得到的图 $G'$ 就是图 $G$ 的最小生成树。

**复杂度：**

- 时间复杂度：$O(E \log E)$
- 空间复杂度：$O(E)$

```java
int kruskal(Edge[] edges, int n) {
    Arrays.sort(edges, (a, b) -> a.w - b.w);
    UnionFind uf = new UnionFind(n);
    int sum = 0;   // 边权和
    int count = 0; // 已选边的数量
    for (Edge e : edges) {
        if (uf.isConnected(e.u, e.v)) continue;
        uf.union(e.u, e.v);
        sum += e.w;
        if (++count == n - 1) break;
    }
    return sum;
}

class Edge {
    int u, v, w; // 起点、终点、边权

    public Edge(int u, int v, int w) {
        this.u = u;
        this.v = v;
        this.w = w;
    }
}

class UnionFind {
    private int[] parent; // 指向父结点
    private int size;     // 连通块的数量

    public UnionFind(int n) {
        parent = new int[n];
        for (int i = 0; i < n; i++) parent[i] = i;
        size = n;
    }

    public int find(int i) { // 寻找根结点
        if (parent[i] != i)  // 路径压缩
            parent[i] = find(parent[i]);
        return parent[i];
    }

    public boolean isConnected(int i, int j) { // 判断是否连通
        return find(i) == find(j);
    }

    public void union(int i, int j) { // 连通
        if (find(i) != find(j)) size--;
        parent[find(j)] = find(i);    // j 加入 i
    }

    public int size() {
        return size;
    }
}
```

### 8.2 Prim 算法

**步骤：**

1. 初始化图 $G'=(\varnothing,\varnothing)$，随机选择一个顶点加入，得到 $G'=(V',\varnothing)$。
2. 选择 $G'$ 中顶点到其他顶点边权最小的边，将边及其对应顶点加入 $G'$。
3. 重复步骤 2 直到全部顶点都加入 $G'$，即 $G$ 的最小生成树。

**复杂度：**

- 时间复杂度：$O(E \log V)$
- 空间复杂度：$O(E)$

```java
int prim(List<Edge>[] graph) {
    int n = graph.length;
    boolean[] vis = new boolean[n]; // 顶点是否已选择
    int sum = 0;                    // 最小边权和
    int u = 0;                      // 随机选择一个顶点
    int count = 1;                  // 已选择顶点数量
    PriorityQueue<Edge> pq = new PriorityQueue<>((a, b) -> a.w - b.w); // 按边权升序
    vis[u] = true;
    for (Edge e : graph[u])         // 连接未选择顶点的边才会添加
        if (!vis[e.v]) pq.offer(e);
    while (!pq.isEmpty()) {
        Edge edge = pq.poll();
        if (vis[edge.v]) continue;  // 跳过连接已选择顶点的边
        u = edge.v;
        sum += edge.w;
        count++;
        vis[u] = true;
        for (Edge e : graph[u])
            if (!vis[e.v]) pq.offer(e);
        if (count == n) break;
    }
    return sum;
}

class Edge {
    int u, v, w; // 起点、终点、边权

    public Edge(int u, int v, int w) {
        this.u = u;
        this.v = v;
        this.w = w;
    }
}
```

## 9.最短路径

### 9.1 Dijkstra 算法

- 单源最短路径
- 不能处理负边权

**步骤：**

1. 从起点开始，将起点加入小根堆。
2. 若从当前顶点到相邻顶点的距离小于“到相邻顶点的最短距离”，则更新到相邻顶点的最短距离，同时将相邻顶点加入小根堆。
3. 重复步骤 2，直到没有顶点的最短距离得到更新。

**复杂度：**

- 时间复杂度：$ O(E \log V) $
- 空间复杂度：$ O(V+E) $

```java
int[] dijkstra(List<Edge>[] graph, int start) {
    int n = graph.length;
    int[] dis = new int[n]; // start 到所有顶点的最短距离
    Arrays.fill(dis, 0x3f3f3f3f); // 防止溢出
    PriorityQueue<Vertex> pq = new PriorityQueue<>((a, b) -> a.dis - b.dis);
    dis[start] = 0;
    pq.offer(new Vertex(start, 0));
    while (!pq.isEmpty()) {
        Vertex u = pq.poll();
        if (dis[u.id] < u.dis) continue;
        for (Edge e : graph[u.id]) {
            if (dis[e.v] > dis[u.id] + e.w) { // 更新最短路径
                dis[e.v] = dis[u.id] + e.w;
                pq.offer(new Vertex(e.v, dis[e.v]));
            }
        }
    }
    return dis;
}

class Edge {
    public int u, v, w; // 起点、终点、边权

    public Edge(int u, int v, int w) {
        this.u = u;
        this.v = v;
        this.w = w;
    }
}

class Vertex {
    public int id, dis; // 顶点编号、start 到该点的距离

    public Vertex(int id, int dis) {
        this.id = id;
        this.dis = dis;
    }
}
```

### 9.2 SPFA 算法

- 能够处理负边权
- 不能处理负环，能够判断负环

```java
int[] spfa(List<Edge>[] graph, int start) {

}
```

### 9.3 Bellman-Ford 算法

- 单源最短路径
- 能够处理负边权
- 能够检测负环
- 时间复杂度：$ O(V*E) $

```java
int[] bellmanFord(List<Edge>[] graph, Edge[] edges, int start) {
    int n = graph.length;
    int[] dis = new int[n];
    Arrays.fill(dis, 0x3f3f3f3f);
    dis[start] = 0;
    for (int k = 1; k < n; k++)
        for (Edge e : edges)
            dis[e.v] = Math.min(dis[e.v], dis[e.u] + e.w);
    for (Edge e : edges)
        if (dis[e.v] > dis[e.u] + e.w) // 存在负环
            return null;
    return dis;
}

class Edge {
    public int u, v, w; // 起点、终点、边权

    public Edge(int u, int v, int w) {
        this.u = u;
        this.v = v;
        this.w = w;
    }
}
```

### 9.4 Floyd 算法

- 多源最短路径
- 可处理负边权
- 时间复杂度：$ O(V^3) $

```java
void floyd(List<Edge>[] graph) {
    // 转为邻接矩阵
    int n = graph.length;
    int[][] dis = new int[n][n];
    for (int i = 0; i < n; i++)
        for (Edge e : graph[u])
            dis[e.u][e.v] = e.w;
    // 矩阵 n 次方，将每个顶点都作为中间顶点
    for (int k = 0; k < n; k++)
        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                dis[i][j] = Math.max(dis[i][k] + dis[k][j]);
}

class Edge {
    public int u, v, w; // 起点、终点、边权

    public Edge(int u, int v, int w) {
        this.u = u;
        this.v = v;
        this.w = w;
    }
}
```

### 9.5 A* 算法

## 实战

### 拓扑排序

#### 🟨课程表

[207. 课程表](https://leetcode.cn/problems/course-schedule/)

#### 🟨课程表 II

[210. 课程表 II](https://leetcode.cn/problems/course-schedule-ii/)

- DFS

```java
class Solution {
    List<List<Integer>> graph;
    int[] vis;
    int[] ans;
    int index;

    public int[] findOrder(int numCourses, int[][] prerequisites) {
        graph = new ArrayList<>(numCourses);
        for (int i = 0; i < numCourses; i++)
            graph.add(new ArrayList<>());
        for (int[] pre : prerequisites)
            graph.get(pre[0]).add(pre[1]);    // 逆序建图
            // graph.get(pre[1]).add(pre[0]); // 顺序建图
        // index = numCourses - 1; // 顺序建图
        vis = new int[numCourses];
        ans = new int[numCourses];
        for (int i = 0; i < numCourses; i++)
            if (vis[i] == 0 && dfs(i)) // 存在环
                return new int[0];
        return ans;
    }

    boolean dfs(int u) {
        vis[u] = 1;
        for (int v : graph.get(u)) {
            if (vis[v] == 0 && dfs(v)) return true; // 剪枝
            else if (vis[v] == 1) return true;      // 存在环
        }
        vis[u] = 2;
        ans[index++] = u;
        // ans[index--] = u; // 顺序建图
        return false;
    }
}
```

- BFS

```java
class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        List<List<Integer>> graph = new ArrayList<>(numCourses);
        for (int i = 0; i < numCourses; i++)
            graph.add(new ArrayList<>());
        int[] indegree = new int[numCourses];
        for (int[] pre : prerequisites) {
            indegree[pre[0]]++;
            graph.get(pre[1]).add(pre[0]);
        }
        int[] ans = new int[numCourses];
        int index = 0;
        Queue<Integer> que = new ArrayDeque<>();
        int count = 0;
        for (int i = 0; i < numCourses; i++)
            if (indegree[i] == 0) {
                que.offer(i);
                ans[index++] = i;
            }
        while (!que.isEmpty()) {
            int u = que.poll();
            count++;
            for (int v : graph.get(u)) {
                indegree[v]--;
                if (indegree[v] == 0) {
                    que.offer(v);
                    ans[index++] = v;
                }
            }
        }
        return count == numCourses ? ans : new int[0];
    }
}
```

#### 🟥课程表 III

[630. 课程表 III](https://leetcode.cn/problems/course-schedule-iii/)

#### 🟨课程表 IV

[1462. 课程表 IV](https://leetcode.cn/problems/course-schedule-iv/)

####

####

### Flood Fill

#### 图像渲染

[733. 图像渲染](https://leetcode.cn/problems/flood-fill/)

- DFS

```java
class Solution {
    public int[][] floodFill(int[][] image, int sr, int sc, int color) {
        if (image[sr][sc] == color) return image;
        dfs(image, sr, sc, image[sr][sc], color);
        return image;
    }

    void dfs(int[][] image, int x, int y, int v1, int v2) {
        if (x < 0 || x >= image.length || y < 0 || y >= image[0].length || image[x][y] != v1) return;
        image[x][y] = v2;
        dfs(image, x - 1, y, v1, v2);
        dfs(image, x, y + 1, v1, v2);
        dfs(image, x + 1, y, v1, v2);
        dfs(image, x, y - 1, v1, v2);
    }
}
```

- BFS

```java
class Solution {
    public int[][] floodFill(int[][] image, int sr, int sc, int color) {
        if (image[sr][sc] == color) return image;
        int oldColor = image[sr][sc];
        int m = image.length;
        int n = image[0].length;
        Queue<Integer> que = new ArrayDeque<>();
        int[][] dir = { { 0, 1 }, { 0, -1 }, { 1, 0 }, { -1, 0 } };
        que.offer(sr);
        que.offer(sc);
        while (!que.isEmpty()) {
            int r = que.poll();
            int c = que.poll();
            image[r][c] = color;
            for (int[] d : dir) {
                int rr = r + d[0];
                int cc = c + d[1];
                if (rr >= 0 && rr < m && cc >= 0 && cc < n && image[rr][cc] == oldColor) {
                    que.offer(rr);
                    que.offer(cc);
                }
            }
        }
        return image;
    }
}
```

- [并查集](../数据结构-并查集/#图像渲染)

#### 岛屿数量

[200. 岛屿数量](https://leetcode.cn/problems/number-of-islands/)

- DFS

```java
class Solution {
    public int numIslands(char[][] grid) {
        int ans = 0;
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j] == '1') {
                    ans++;
                    dfs(grid, i, j, '1', '2');
                }
            }
        }
        return ans;
    }

    void dfs(char[][] grid, int x, int y, char v1, char v2) {
        if (x < 0 || x >= grid.length || y < 0 || y >= grid[0].length || grid[x][y] != v1) return;
        grid[x][y] = v2;
        dfs(grid, x - 1, y, v1, v2);
        dfs(grid, x, y + 1, v1, v2);
        dfs(grid, x + 1, y, v1, v2);
        dfs(grid, x, y - 1, v1, v2);
    }
}
```

- BFS

```java
class Solution {
    int m, n;
    Queue<Integer> que = new ArrayDeque<>();
    int[][] dir = { { 0, 1 }, { 0, -1 }, { 1, 0 }, { -1, 0 } };

    public int numIslands(char[][] grid) {
        m = grid.length;
        n = grid[0].length;
        int ans = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '1') {
                    ans++;
                    bfs(grid, i, j, '1', '2');
                }
            }
        }
        return ans;
    }

    void bfs(char[][] grid, int x, int y, char v1, char v2) {
        que.offer(x);
        que.offer(y);
        grid[x][y] = v2;
        while (!que.isEmpty()) {
            x = que.poll();
            y = que.poll();
            for (int[] d : dir) {
                int xx = x + d[0];
                int yy = y + d[1];
                if (xx >= 0 && xx < m && yy >= 0 && yy < n && grid[xx][yy] == v1) {
                    que.offer(xx);
                    que.offer(yy);
                    grid[xx][yy] = v2;
                }
            }
        }
    }
}
```

- [并查集](../数据结构-并查集/#岛屿数量)

#### 被围绕的区域

[130. 被围绕的区域](https://leetcode.cn/problems/surrounded-regions/)

- DFS

```java
class Solution {
    public void solve(char[][] board) {
        int m = board.length;
        int n = board[0].length;
        // 1.将边缘连通块改为其他值
        for (int i = 0; i < m; i++) {
            dfs(board, i, 0, 'O', '#');
            dfs(board, i, n - 1, 'O', '#');
        }
        for (int j = 0; j < n; j++) {
            dfs(board, 0, j, 'O', '#');
            dfs(board, m - 1, j, 'O', '#');
        }
        // 2.修改中间连通块
        for (int i = 1; i + 1 < m; i++)
            for (int j = 1; j + 1 < n; j++)
                if (board[i][j] == 'O')
                    board[i][j] = 'X';
        // 3.还原边缘连通块
        for (int i = 0; i < m; i++) {
            dfs(board, i, 0, '#', 'O');
            dfs(board, i, n - 1, '#', 'O');
        }
        for (int j = 0; j < n; j++) {
            dfs(board, 0, j, '#', 'O');
            dfs(board, m - 1, j, '#', 'O');
        }
    }

    void dfs(char[][] board, int x, int y, char v1, char v2) {
        if (x < 0 || x >= board.length || y < 0 || y >= board[0].length || board[x][y] != v1) return;
        board[x][y] = v2;
        dfs(board, x + 1, y, v1, v2);
        dfs(board, x, y + 1, v1, v2);
        dfs(board, x - 1, y, v1, v2);
        dfs(board, x, y - 1, v1, v2);
    }
}
```

- BFS

```java
class Solution {
    int m, n;
    Queue<Integer> que = new ArrayDeque<>();
    int[][] dir = { { 0, 1 }, { 0, -1 }, { 1, 0 }, { -1, 0 } };

    public void solve(char[][] board) {
        m = board.length;
        n = board[0].length;
        // 1.将边缘连通块改为其他值
        for (int i = 0; i < m; i++) {
            bfs(board, i, 0, 'O', '#');
            bfs(board, i, n - 1, 'O', '#');
        }
        for (int j = 0; j < n; j++) {
            bfs(board, 0, j, 'O', '#');
            bfs(board, m - 1, j, 'O', '#');
        }
        // 2.修改中间连通块
        for (int i = 1; i + 1 < m; i++)
            for (int j = 1; j + 1 < n; j++)
                if (board[i][j] == 'O')
                    board[i][j] = 'X';
        // 3.还原边缘连通块
        for (int i = 0; i < m; i++) {
            bfs(board, i, 0, '#', 'O');
            bfs(board, i, n - 1, '#', 'O');
        }
        for (int j = 0; j < n; j++) {
            bfs(board, 0, j, '#', 'O');
            bfs(board, m - 1, j, '#', 'O');
        }
    }

    void bfs(char[][] mat, int x, int y, char v1, char v2) {
        if (mat[x][y] != v1) return;
        que.offer(x);
        que.offer(y);
        mat[x][y] = v2;
        while (!que.isEmpty()) {
            x = que.poll();
            y = que.poll();
            for (int[] d : dir) {
                int xx = x + d[0];
                int yy = y + d[1];
                if (xx >= 0 && xx < m && yy >= 0 && yy < n && mat[xx][yy] == v1) {
                    que.offer(xx);
                    que.offer(yy);
                    mat[xx][yy] = v2;
                }
            }
        }
    }
}
```

- [并查集](../数据结构-并查集/#被围绕的区域)

#### 岛屿的最大面积

[695. 岛屿的最大面积](https://leetcode.cn/problems/max-area-of-island/)

- DFS

```java
class Solution {
    public int maxAreaOfIsland(int[][] grid) {
        int ans = 0;
        int m = grid.length;
        int n = grid[0].length;
        for (int i = 0; i < m; i++)
            for (int j = 0; j < n; j++)
                if (grid[i][j] == 1)
                    ans = Math.max(ans, dfs(grid, i, j, 1, 2));
        return ans;
    }

    int dfs(int[][] mat, int x, int y, int v1, int v2) {
        if (x < 0 || x >= mat.length || y < 0 || y >= mat[0].length || mat[x][y] != v1) return 0;
        mat[x][y] = v2;
        int ans = 1;
        ans += dfs(mat, x - 1, y, v1, v2);
        ans += dfs(mat, x, y + 1, v1, v2);
        ans += dfs(mat, x + 1, y, v1, v2);
        ans += dfs(mat, x, y - 1, v1, v2);
        return ans;
    }
}
```

- BFS

- 并查集

### 最小生成树

#### 连接所有点的最小费用

[1584. 连接所有点的最小费用](https://leetcode.cn/problems/min-cost-to-connect-all-points/)

- Kruskal

```java
class Solution {
    public int minCostConnectPoints(int[][] points) {
        int n = points.length;
        Edge[] edges = new Edge[n * (n - 1) / 2];
        int k = 0;
        for (int i = 0; i < n; i++)
            for (int j = i + 1; j < n; j++)
                edges[k++] = new Edge(i, j, manhattanDistance(points[i], points[j]));
        return kruskal(edges, n);
    }

    int kruskal(Edge[] edges, int n) {
        Arrays.sort(edges, (a, b) -> a.w - b.w);
        UnionFind uf = new UnionFind(n);
        int sum = 0;   // 边权和
        int count = 0; // 已选边的数量
        for (Edge e : edges) {
            if (uf.isConnected(e.u, e.v)) continue;
            uf.union(e.u, e.v);
            sum += e.w;
            if (++count == n - 1) break;
        }
        return sum;
    }

    int manhattanDistance(int[] x, int[] y) {
        return Math.abs(x[0] - y[0]) + Math.abs(x[1] - y[1]);
    }
}

class Edge {
    int u, v, w; // 起点、终点、边权

    public Edge(int u, int v, int w) {
        this.u = u;
        this.v = v;
        this.w = w;
    }
}

class UnionFind {
    private int[] parent; // 指向父结点
    private int size;     // 连通块的数量

    public UnionFind(int n) {
        parent = new int[n];
        for (int i = 0; i < n; i++) parent[i] = i;
        size = n;
    }

    public int find(int i) { // 寻找根结点
        if (parent[i] != i)  // 路径压缩
            parent[i] = find(parent[i]);
        return parent[i];
    }

    public boolean isConnected(int i, int j) { // 判断是否连通
        return find(i) == find(j);
    }

    public void union(int i, int j) { // 连通
        if (find(i) != find(j)) size--;
        parent[find(j)] = find(i);    // j 加入 i
    }

    public int size() {
        return size;
    }
}
```

- Prim

```java
class Solution {
    public int minCostConnectPoints(int[][] points) {
        int n = points.length;
        List<Edge>[] graph = new ArrayList[n];
        for (int i = 0; i < n; i++)
            graph[i] = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                int dis = manhattanDistance(points[i], points[j]);
                graph[i].add(new Edge(i, j, dis));
                graph[j].add(new Edge(j, i, dis));
            }
        }
        return prim(graph);
    }

    int prim(List<Edge>[] graph) {
        int n = graph.length;
        boolean[] vis = new boolean[n]; // 顶点是否已选择
        int sum = 0;                    // 最小边权和
        int u = 0;                      // 随机选择一个顶点
        int count = 1;                  // 已选择顶点数量
        vis[u] = true;
        PriorityQueue<Edge> pq = new PriorityQueue<>((a, b) -> a.w - b.w); // 按边权升序
        while (count < n) {
            for (Edge e : graph[u])     // 连接未选择顶点的边才会添加
                if (!vis[e.v]) pq.offer(e);
            if (pq.isEmpty()) break;
            Edge e;
            do {
                e = pq.poll(); // 跳过没有连接未选择顶点的边
            } while (!pq.isEmpty() && vis[e.v]);
            u = e.v;
            vis[u] = true;
            sum += e.w;
            count++;
        }
        return sum;
    }

    int manhattanDistance(int[] x, int[] y) {
        return Math.abs(x[0] - y[0]) + Math.abs(x[1] - y[1]);
    }
}

class Edge {
    int u, v, w; // 起点、终点、边权

    public Edge(int u, int v, int w) {
        this.u = u;
        this.v = v;
        this.w = w;
    }
}
```

### 其他

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

## 参考

- [《算法（第4版）》]()

