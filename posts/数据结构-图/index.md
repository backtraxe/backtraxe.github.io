# 数据结构-图


<!--more-->

### 术语表

- 图（graph）
- 边（edge）：连接两个顶点。
    - 自环：起点和终点是同一个顶点的边。
    - 平行边：连接同一对顶点的两条边。
- 顶点（vertex）
- 度（degree）：顶点连接的边的数量。
    - 入度（in degree）：有向图专有，终点是当前顶点的边的数量。
    - 出度（out degree）：有向图专有，起点是当前顶点的边的数量。
- 路径（path）：由边连接的一系列顶点。
    - 简单路径：无重复顶点的路径。
- 环：起点和终点相同的路径。
    - 简单环：无重复顶点的环。

是否存在平行边：

- 简单图：无平行边的图。
- 多重图：存在平行边的图。

边是否存在方向：

- 有向图：边有方向。
- 无向图：边无方向。


- 无环图：不存在环的图。
- 子图：边和连接的顶点的子集。
- 连通图：任意一个顶点都存在路径到达另一个任意顶点。
    - 连通子图：
    - 极大连通子图：
- 二分图：能将所有顶点分为两个部分的图，每条边的两个顶点分别属于不同的部分。

### 图的存储

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
-

### 图的遍历

#### 深度优先搜索（DFS）

```java
boolean[] vis;                                   // 判断顶点是否访问过，
Deque<Integer> trace;                            // 记录当前路径
boolean[] onPath;                                // 判断某顶点是否在当前路径上
List<List<Integer>> traces = new LinkedList<>(); // 记录所有路径

// 邻接表
void dfs(List<Integer>[] graph, int begin, int end) {
    // ===== 1.进入当前顶点
    // 访问当前顶点
    vis[begin] = true;
    onPath[begin] = true;
    trace.offerLast(begin);
    // 到达终点
    if (begin == end) {
        // 防止已添加路径被修改
        traces.add(new LinkedList<Integer>(trace));
        // 回溯
        vis[begin] = false;
        onPath[begin] = false;
        trace.pollLast();
        return;
    }
    // 继续遍历
    for (int next : graph[begin]) {
        if (!vis[next]) {
            // ===== 2.进入相邻顶点
            dfs(graph, next, end);
            // ===== 3.从相邻顶点返回
        }
    }
    // ===== 4.离开当前顶点
    // 回溯
    vis[begin] = false;
    onPath[begin] = false;
    trace.pollLast();
}
```

```java
boolean[] vis;
Deque<Integer> trace;                            // 记录当前路径
boolean[] onPath;                                // 判断某顶点是否在当前路径上
List<List<Integer>> traces = new LinkedList<>(); // 记录所有路径

// 邻接矩阵
void dfs(int[][] graph, int begin) {
    if (vis[begin]) {
        return;
    }
    // 访问当前顶点
    vis[begin] = true;
    onPath[begin] = true;
    trace.offerLast(begin);
    // 到达终点
    if (begin == end) {
        // 防止已添加路径被修改
        traces.add(new LinkedList<Integer>(trace));
        // 回溯
        vis[begin] = false;
        onPath[begin] = false;
        trace.pollLast();
        return;
    }
    // 下个顶点
    for (int next = 0; next < graph.length; next++) {
        if (!vis[next] && graph[begin][next] != 0) {
            dfs(graph, next);
        }
    }
    // 回溯
    vis[begin] = false;
    onPath[begin] = false;
    trace.pollLast();
}
```

#### 广度优先搜索（BFS）

```python
def BFS(开始节点 s):
    q = 队列
    vis = 集合/哈希表/布尔数组
    将s添加到q中
    将s添加到vis中/标记s为已访问
    步数 = 0
    while(q非空):
        for q的每一个节点，记为h # 每次清空队列
            if 满足结束条件:
                return
            for h的每个相邻节点x:
                if x未在vis中/x未访问:
                    将x添加到q中
                    将x添加到vis中/标记x为已访问
        步数 += 1
```

```python
# 双向BFS
def BiBFS(开始节点 s，结束节点 e):
    s1 = 从s开始的搜索集合
    s2 = 从e开始的搜索集合
    vis = 集合/哈希表/布尔数组
    将s添加到s1中
    将s添加到vis中/标记s为已访问
    将e添加到s2中
    将e添加到vis中/标记e为已访问
    步数 = 0
    while(s1非空 and s2非空):
        if s1中元素比s2多:
            # 交换s1和s2
            s1, s2 = s2, s1
        s3 = 过渡集合
        for s1的每一个节点，记为h # 每次清空队列
            if h在s2中存在:
                return
            for h的每个相邻节点x:
                if x未在vis中/x未访问:
                    将x添加到s3中
                    将x添加到vis中/标记x为已访问
        # 交换q1和q2（一边一轮）
        s1 = s2
        s2 = s3
        步数 += 1
```

### Flood Fill

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

