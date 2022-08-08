# 数据结构-并查集


<!--more-->

## 基础

```java
class UnionFind {
    int[] p;   // 指向父结点
    int count; // 树的数量

    public UnionFind(int n) {
        p = new int[n];
        for (int i = 0; i < n; i++)
            p[i] = i; // 初始化为指向自己
        count = n;
    }

    int find(int[] p, int i) {               // 寻找根结点
        if (p[i] != i) p[i] = find(p, p[i]); // 路径压缩
        return p[i];
    }

    int union(int[] p, int i, int j) { // 合并两棵树
        int pi = find(p, i);
        int pj = find(p, j);
        p[pi] = pj;                    // 根结点插入另一棵树
        if (pi != pj) count--;
    }
}
```

## 2.实战

### 图像渲染

[733. 图像渲染](https://leetcode.cn/problems/flood-fill/)

```java
class Solution {
    public int[][] floodFill(int[][] image, int sr, int sc, int color) {
        int oldColor = image[sr][sc];
        if (oldColor == color) return image;
        int m = image.length;
        int n = image[0].length;
        int[] p = new int[m * n];
        for (int i = 0; i < m * n; i++) p[i] = i;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                int x = i * n + j;
                // 只需判断右、下
                if (j + 1 < n && image[i][j] == image[i][j + 1]) union(p, x, x + 1);
                if (i + 1 < m && image[i][j] == image[i + 1][j]) union(p, x, x + n);
            }
        }
        int root = find(p, sr * n + sc);
        for (int i = 0; i < m; i++)
            for (int j = 0; j < n; j++)
                if (find(p, i * n + j) == root)
                    image[i][j] = color;
        return image;
    }

    int find(int[] p, int i) {
        if (i != p[i]) p[i] = find(p, p[i]);
        return p[i];
    }

    void union(int[] p, int i, int j) {
        int pi = find(p, i);
        int pj = find(p, j);
        p[pi] = pj;
    }
}
```

### 岛屿数量

[200. 岛屿数量](https://leetcode.cn/problems/number-of-islands/)

```java
class Solution {
    int ans;

    public int numIslands(char[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        int[] p = new int[m * n];
        for (int i = 0; i < m * n; i++) p[i] = i;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '1') {
                    ans++;
                    int x = i * n + j;
                    // 只需判断右、下
                    if (j + 1 < n && grid[i][j + 1] == '1') union(p, x, x + 1);
                    if (i + 1 < m && grid[i + 1][j] == '1') union(p, x, x + n);
                }
            }
        }
        return ans;
    }

    int find(int[] p, int i) {
        if (i != p[i]) p[i] = find(p, p[i]);
        return p[i];
    }

    void union(int[] p, int i, int j) {
        int pi = find(p, i);
        int pj = find(p, j);
        p[pi] = pj;
        if (pi != pj) ans--;
    }
}
```

