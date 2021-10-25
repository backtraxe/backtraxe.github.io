# 力扣 0229 求众数II


[题目链接](https://leetcode-cn.com/problems/majority-element-ii/)

<!--more-->

## 摩尔投票法

### 背景

摩尔投票法的核心思想为对拼消耗。首先我们考虑最基本的摩尔投票问题，比如找出一组数字序列中出现次数大于总数 $\frac{1}{2}$ 的数字（并且假设这个数字一定存在）。我们可以直接利用反证法证明这样的数字只可能有一个。摩尔投票算法的核心思想是基于这个事实：

- 每次从序列里选择两个不相同的数字删除掉（或称为「抵消」），最后剩下一个数字或几个相同的数字，就是出现次数大于总数一半的那个元素。假设我们当前数组中存在次数大于总数一半的元素为 x，数组的总长度为 n，则我们可以把数组分为两部分，一部分为相同的 k 个元素 x，另一部分为 $\frac{n - k}{2}$ 对个不同的元素配对，此时我们假设还存在另外一个次数大于总数一半的元素 y，则此时 y 应该满足 y $\gt \frac{n}{2}$，但是按照我们之前的推理 y 应当满足 y $\le \frac{n - k}{2}$，二者自相矛盾。
- 论文地址：[MJRTYA Fast Majority Vote Algorithm](https://www.cs.ou.edu/~rlpage/dmtools/mjrty.pdf)

### 步骤

### 代码

```cpp
class Solution {
  public:
    vector<int> majorityElement(vector<int> &nums) {
        // 记录两个元素的值
        int value1, value2;
        // 记录对应元素出现的次数
        int count1 = 0, count2 = 0;
        // 第一次遍历：筛选出现次数最多元素（至少0个，至多2个）或者出现位置偏后的元素，用 value1 和 value2 记录
        for (int &v : nums) {
            if (count1 == 0 && count2 == 0 || count1 == 0 && v != value2) {
                value1 = v;
                count1++;
            } else if (count2 == 0 && v != value1) {
                value2 = v;
                count2++;
            }
            else if (v == value1) count1++;
            else if (v == value2) count2++;
            else {
                // 三个元素均不相等则消除
                count1--;
                count2--;
            }
        }
        int total_count1 = 0;
        int total_count2 = 0;
        // 第二次遍历：记录筛选出的元素的出现次数，用来检验是否满足题意
        for (int &v : nums) {
            if (count1 > 0 && v == value1) total_count1++;
            if (count2 > 0 && v == value2) total_count2++;
        }
        vector<int> ans;
        int len = nums.size();
        if (total_count1 > len / 3) ans.push_back(value1);
        if (total_count2 > len / 3) ans.push_back(value2);
        return ans;
    }
};
```

### 复杂度分析

- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$

