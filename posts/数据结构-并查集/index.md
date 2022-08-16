# 数据结构-并查集


<!--more-->

## 1.基础

### 模板

- 定义

```java
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

- 使用

```java
UnionFind uf = new UnionFind(n);
int root = uf.find(i);
uf.isConnected(i, j);
uf.union(i, j);
int size = uf.size();
```

## 2.实战

### Flood Fill

#### 图像渲染

[733. 图像渲染](https://leetcode.cn/problems/flood-fill/)

```java
class Solution {
    public int[][] floodFill(int[][] image, int sr, int sc, int color) {
        int oldColor = image[sr][sc];
        if (oldColor == color) return image;
        int m = image.length;
        int n = image[0].length;
        UnionFind uf = new UnionFind(m * n);
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                int x = i * n + j;
                // 只需判断右、下
                if (j + 1 < n && image[i][j] == image[i][j + 1]) uf.union(x, x + 1);
                if (i + 1 < m && image[i][j] == image[i + 1][j]) uf.union(x, x + n);
            }
        }
        int x = sr * n + sc;
        for (int i = 0; i < m; i++)
            for (int j = 0; j < n; j++)
                if (uf.isConnected(x, i * n + j))
                    image[i][j] = color;
        return image;
    }
}

class UnionFind {
    private int[] p;   // 指向父结点
    private int count; // 树的数量

    public UnionFind(int n) {
        p = new int[n];
        for (int i = 0; i < n; i++) p[i] = i;
        count = n;
    }

    public boolean isConnected(int i, int j) {
        return find(i) == find(j);
    }

    public void union(int i, int j) {      // 合并两棵树
        if (find(i) != find(j)) count--;
        p[find(j)] = find(i);              // j 插入 i
    }

    public int size() {
        return count;
    }

    private int find(int i) {              // 寻找根结点
        if (p[i] != i) p[i] = find(p[i]);  // 路径压缩
        return p[i];
    }
}
```

#### 岛屿数量

[200. 岛屿数量](https://leetcode.cn/problems/number-of-islands/)

```java
class Solution {
    public int numIslands(char[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        UnionFind uf = new UnionFind(m * n);
        int zeroCount = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '1') {
                    int x = i * n + j;
                    // 只需判断右、下
                    if (j + 1 < n && grid[i][j + 1] == '1') uf.union(x, x + 1);
                    if (i + 1 < m && grid[i + 1][j] == '1') uf.union(x, x + n);
                } else zeroCount++;
            }
        }
        // 连通块数量 - 0 的数量 = 1 的连通块数量
        return uf.size() - zeroCount;
    }
}

class UnionFind {
    private int[] p;   // 指向父结点
    private int count; // 树的数量

    public UnionFind(int n) {
        p = new int[n];
        for (int i = 0; i < n; i++) p[i] = i;
        count = n;
    }

    public boolean isConnected(int i, int j) {
        return find(i) == find(j);
    }

    public void union(int i, int j) {      // 合并两棵树
        if (find(i) != find(j)) count--;
        p[find(j)] = find(i);              // j 插入 i
    }

    public int size() {
        return count;
    }

    private int find(int i) {              // 寻找根结点
        if (p[i] != i) p[i] = find(p[i]);  // 路径压缩
        return p[i];
    }
}
```

#### 被围绕的区域

[130. 被围绕的区域](https://leetcode.cn/problems/surrounded-regions/)

```java
class Solution {
    public void solve(char[][] board) {
        int m = board.length;
        int n = board[0].length;
        // 额外一个空间，相当于哨兵结点
        UnionFind uf = new UnionFind(m * n + 1);
        int dummy = m * n;
        // 1.将边缘连通块与 dummy 连通
        for (int i = 0; i < m; i++) {
            if (board[i][0] == 'O') uf.union(dummy, i * n);
            if (board[i][n - 1] == 'O') uf.union(dummy, i * n + n - 1);
        }
        for (int j = 0; j < n; j++) {
            if (board[0][j] == 'O') uf.union(dummy, j);
            if (board[m - 1][j] == 'O') uf.union(dummy, (m - 1) * n + j);
        }
        // 2.连通相同块
        for (int i = 0; i + 1 < m; i++) {
            for (int j = 0; j + 1 < n; j++) {
                int x = i * n + j;
                // 只需要 右、下
                if (board[i][j] == board[i][j + 1]) uf.union(x, x + 1);
                if (board[i][j] == board[i + 1][j]) uf.union(x, x + n);
            }
        }
        // 3.修改中间连通块
        for (int i = 1; i + 1 < m; i++)
            for (int j = 1; j + 1 < n; j++)
                if (board[i][j] == 'O' && !uf.isConnected(dummy, i * n + j))
                    board[i][j] = 'X';
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

### 二分图

#### 判断二分图

[785. 判断二分图](https://leetcode.cn/problems/is-graph-bipartite/)

```java
class Solution {
    public boolean isBipartite(int[][] graph) {
        int n = graph.length;
        UnionFind uf = new UnionFind(n);
        for (int u = 0; u < n; u++) { // 非连通图
            for (int v : graph[u]) {
                if (uf.isConnected(u, v)) return false; // 相邻顶点不连通
                uf.union(graph[u][0], v);               // 连通 u 的所有相邻顶点
            }
        }
        return true;
    }
}

class UnionFind {
    private int[] p;   // 指向父结点
    private int count; // 树的数量

    public UnionFind(int n) {
        p = new int[n];
        for (int i = 0; i < n; i++) p[i] = i;
        count = n;
    }

    public boolean isConnected(int i, int j) {
        return find(i) == find(j);
    }

    public void union(int i, int j) {      // 合并两棵树
        if (find(i) != find(j)) count--;
        p[find(j)] = find(i);              // j 插入 i
    }

    public int size() {
        return count;
    }

    private int find(int i) {              // 寻找根结点
        if (p[i] != i) p[i] = find(p[i]);  // 路径压缩
        return p[i];
    }
}
```

#### 等式方程的可满足性

[990. 等式方程的可满足性](https://leetcode.cn/problems/satisfiability-of-equality-equations/)

```java
class Solution {
    public boolean equationsPossible(String[] equations) {
        Arrays.sort(equations, (a, b) -> b.charAt(1) - a.charAt(1));
        UnionFind uf = new UnionFind(26);
        for (String eq : equations) {
            int i = eq.charAt(0) - 'a';
            int j = eq.charAt(3) - 'a';
            if (eq.charAt(1) == '=') uf.union(i, j);
            else if (uf.isConnected(i, j)) return false;
        }
        return true;
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
