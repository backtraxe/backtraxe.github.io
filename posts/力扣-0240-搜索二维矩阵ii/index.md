# 力扣 0240 搜索二维矩阵II


[题目链接](https://leetcode-cn.com/problems/search-a-2d-matrix-ii/)

<!--more-->

## 反对角线查找

### 步骤

沿着反对角线进行查找，可以右上到左下，也可以反过来，以右上到左下为例：

- 如果当前元素与 target 相等，返回 true；
- 如果当前元素大于 target，由于每一列的元素都是升序排列的，那么当前元素所在列往下所有元素全都大于 target，因此考虑左侧的元素；
- 如果当前元素小于 target，由于每一行的元素都是升序排列的，那么当前元素所在行往左所有元素全都小于 target，因此考虑下方的元素。

### 代码

```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix.size();
        int n = matrix[0].size();
        // 右上角
        int x = 0, y = n - 1;
        while (x < m && y >= 0) {
            if (matrix[x][y] == target) return true;
            // 先左
            else if (matrix[x][y] > target) y--;
            // 后下
            else x++;
        }
        return false;
    }
};
```

### 复杂度分析

- 时间复杂度：$O(m + n)$
- 空间复杂度：$O(1)$

