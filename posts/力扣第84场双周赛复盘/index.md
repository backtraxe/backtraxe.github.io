# 力扣第84场双周赛复盘


[第 84 场双周赛](https://leetcode.cn/contest/biweekly-contest-84/)

<!--more-->

## 复盘

- 全国排名：745 / 4574（16.29%）
- 全球排名：1631 / 23102（7.06%）
- 分数变化：2112 - 5 = 2107
- 完成时间：1h52m53s（+25m）
- 题目1
    - 难度：
    - 顺序：1
    - 用时：2m46s
    - 错误：0
- 题目2
    - 难度：
    - 顺序：3
    - 用时：6m41s
    - 错误：2
        - WA：int 溢出
- 题目3
    - 难度：
    - 顺序：2
    - 用时：25m49s
    - 错误：0
- 题目4
    - 难度：
    - 顺序：4
    - 用时：20m6s
    - 错误：3
        - WA：思路错误

## 合并相似的物品

[6141. 合并相似的物品](https://leetcode.cn/problems/merge-similar-items/)

```java
class Solution {
    public List<List<Integer>> mergeSimilarItems(int[][] items1, int[][] items2) {
        TreeMap<Integer, Integer> map = new TreeMap<>();
        for (int[] a : items1)
            map.put(a[0], map.getOrDefault(a[0], 0) + a[1]);
        for (int[] a : items2)
            map.put(a[0], map.getOrDefault(a[0], 0) + a[1]);
        List<List<Integer>> ans = new ArrayList<>();
        for (int key : map.keySet())
            ans.add(List.of(key, map.get(key)));
        return ans;
    }
}
```

## 统计坏数对的数目

[6142. 统计坏数对的数目](https://leetcode.cn/problems/count-number-of-bad-pairs/)

```java
class Solution {
    public long countBadPairs(int[] nums) {
        int n = nums.length;
        long ans = (long) n * (n - 1) / 2;
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < n; i++) {
            int x = i - nums[i];
            ans -= map.getOrDefault(x, 0);
            map.put(x, map.getOrDefault(x, 0) + 1);
        }
        return ans;
    }
}
```

## 任务调度器 II

[6174. 任务调度器 II](https://leetcode.cn/problems/task-scheduler-ii/)

```java
class Solution {
    public long taskSchedulerII(int[] tasks, int space) {
        HashMap<Integer, Long> map = new HashMap<>(); // 任务起始时间
        long ans = 0;
        for (int task : tasks) {
            // 当前任务开始之前必须上个任务结束并且上个同类型任务间隔 space 天
            ans = Math.max(ans + 1, map.getOrDefault(task, 0L));
            map.put(task, ans + space + 1);
        }
        return ans;
    }
}
```

## 将数组排序的最少替换次数

[6144. 将数组排序的最少替换次数](https://leetcode.cn/problems/minimum-replacements-to-sort-the-array/)

```java
class Solution {
    public long minimumReplacement(int[] nums) {
        long ans = 0;
        int next = Integer.MAX_VALUE;
        for (int i = nums.length - 1; i >= 0; i--) {
            if (nums[i] > next) {
                int x = (nums[i] + next - 1) / next; // 向上取整
                ans += x - 1;
                next = nums[i] / x;
            } else {
                next = nums[i];
            }
        }
        return ans;
    }
}
```

## 总结

- 哈希表
- 遍历

