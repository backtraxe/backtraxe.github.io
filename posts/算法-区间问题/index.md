# 算法-区间问题


<!--more-->

## 1.区间重叠

[252. 会议室](https://leetcode.cn/problems/meeting-rooms)

1. 按区间左端点升序排序。
2. 若上个区间的右端点大于下个区间的左端点，则两区间重叠。

```java
class Solution {
    public boolean canAttendMeetings(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        for (int i = 1; i < intervals.length; i++)
            if (intervals[i - 1][1] > intervals[i][0])
                return false;
        return true;
    }
}
```

## 2.区间合并

[56. 合并区间](https://leetcode.cn/problems/merge-intervals/)

### 2.1 排序

1. 按区间左端点升序排序。
2. 若两区间重叠则合并两个区间。

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        int[][] ans = new int[intervals.length][2];
        int index = -1;
        for (int[] interval : intervals) {
            if (index == -1 || interval[0] > ans[index][1])
                ans[++index] = interval;
            else
                ans[index][1] = Math.max(ans[index][1], interval[1]);
        }
        return Arrays.copyOf(ans, index + 1);
    }
}
```

### 2.2 位图

1. 位图上的 1 表示在区间内，0 表示不在区间内。
2. 将所有区间表示在位图上，最后每个连续的全 1 区间即为合并后的区间。

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        BitSet bitSet = new BitSet();
        int maxRight = 0;
        for (int[] interval : intervals) {
            // 乘 2 为了使像 [1, 2] 和 [3, 4] 这样的区间不连续
            // [1, 2] -> [2, 4]
            // [3, 4] -> [6, 8]
            int left = interval[0] * 2;
            int right = interval[1] * 2 + 1;
            bitSet.set(left, right); // [left, right)
            maxRight = Math.max(maxRight, right);
        }
        int left = 0;
        int len = 0;
        while (left < maxRight) {
            // [left, right)
            left = bitSet.nextSetBit(left); // 下一个为 1 的位置
            int right = bitSet.nextClearBit(left); // 下一个为 0 的位置
            intervals[len][0] = left / 2;
            intervals[len][1] = right / 2;
            len++;
            left = right;
        }
        return Arrays.copyOf(intervals, len);
    }
}
```

## 3.插入区间

[57. 插入区间](https://leetcode.cn/problems/insert-interval/)

1. 按区间左端点升序排序。
2. 二分查找待插入区间的左端点和右端点位置，合并这些区间。

```java
class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        int n = intervals.length;
        // 1. 在 intervals 中查找最后一个右端点小于 newInterval 左端点的区间
        // 该区间（包含）及之前的区间不用合并
        int left = 0;
        int right = n;
        int mid;
        while (left < right) {
            mid = left + (right - left) / 2;
            if (intervals[mid][1] < newInterval[0]) left = mid + 1;
            else right = mid;
        }
        // 在 newInterval 之前最后一个不用合并的区间的下标
        int start = right - 1;
        // 2. 在 intervals 中查找第一个左端点大于 newInterval 右端点的区间
        // 该区间（包含）及之后的区间不用合并
        left = 0;
        right = n;
        while (left < right) {
            mid = left + (right - left) / 2;
            if (intervals[mid][0] > newInterval[1]) right = mid;
            else left = mid + 1;
        }
        // 在 newInterval 之后第一个不用合并的区间的下标
        int end = left;
        // 3. 合并区间
        // [start + 1, end - 1] 区间内的区间需要合并成 1 个区间
        int[][] ans = new int[(start + 1) + 1 + (n - end)][2];
        int idx = 0;
        for (int i = 0; i <= start; i++)
            ans[idx++] = intervals[i];
        ans[idx][0] = newInterval[0];
        ans[idx][1] = newInterval[1];
        if (start + 1 < n) // 需要合并
            ans[idx][0] = Math.min(ans[idx][0], intervals[start + 1][0]);
        if (end - 1 >= 0) // 需要合并
            ans[idx][1] = Math.max(ans[idx][1], intervals[end - 1][1]);
        idx++;
        for (int i = end; i < n; i++)
            ans[idx++] = intervals[i];
        return ans;
    }
}
```

## 4.删除区间

[435. 无重叠区间](https://leetcode.cn/problems/non-overlapping-intervals/)

1. 按区间左端点升序排序。
2. 当两个区间重叠时，我们选择右端点更小的那个区间。（贪心）

```java
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        int ans = 0;
        int pre = 0;
        for (int i = 1; i < intervals.length; i++) {
            if (intervals[i][0] < intervals[pre][1]) { // 重叠
                if (intervals[i][1] < intervals[pre][1]) pre = i; // 被包含
                ans++;
            } else pre = i; // 不重叠
        }
        return ans;
    }
}
```

## 5. 区间合并、插入、删除综合

[715. Range 模块](https://leetcode.cn/problems/range-module/)

### 5.1 有序集合

1. 随时保证有序集合中的区间不重叠。
2. 插入区间 $[left, right)$：
    1. 二分查找最后一个 $l_i \le left$，即 $l_i \le left && l_{i+1} \gt left$ 的区间 $[l_i, r_i)$
        - 若
        - 若
        - 若
        - 若
    2. 
3. 删除区间`[left, right)`：

```java
class RangeModule {
    TreeMap<Integer, Integer> ranges = new TreeMap<>(); // {l=r}

    public void addRange(int left, int right) {
        Integer ll = ranges.floorKey(left);  // 左侧区间的左端点 ll <= left
        Integer rl = ranges.floorKey(right); // 右侧区间的左端点 rl <= right
        // [ll, lr) 和 [left, right) 区间重叠 ll <= left <= lr (ll < lr)
        if (ll != null && ranges.get(ll) >= left) left = ll;
        // [left, right) 和 [rl, rr) 区间重叠 rl <= right <= rr (rl < rr)
        if (rl != null && ranges.get(rl) >= right) right = ranges.get(rl);
        // 将 [left, right) 向两端扩充为合并后的区间
        ranges.put(left, right);
        // 迭代器删除被 [left, right) 包含的区间
        var it = ranges.keySet().iterator();
        while (it.hasNext()) {
            int l = it.next();
            int r = ranges.get(l);
            if (left < l && r <= right) it.remove();
        }
    }

    public boolean queryRange(int left, int right) {
        Integer ll = ranges.floorKey(left); // 左侧区间的左端点 ll <= left
        // 若 ll <= left < right <= lr，则返回 true
        return ll == null ? false : right <= ranges.get(ll);
    }

    public void removeRange(int left, int right) {
        Integer ll = ranges.lowerKey(left);  // 左侧区间的左端点 ll < left
        Integer rl = ranges.lowerKey(right); // 右侧区间的左端点 rl < right
        if (ll != null) {
            int lr = ranges.get(ll);
            if (lr > right) { // 分割区间 ll < left < right < lr
                ranges.put(ll, left);
                ranges.put(right, lr);
            } else if (lr > left) { // 分割区间 ll < left < lr <= right
                ranges.put(ll, left);
            }
        }
        if (rl != null) {
            int rr = ranges.get(rl);
            // 若 rl < left，则 rl == ll，之前已经分割
            if (rr > right) { // 分割区间 left <= rl < right < rr
                ranges.put(rl, right); // 减小区间，便于删除
                ranges.put(right, rr);
            }
        }
        // 迭代器删除被 [left, right) 包含的区间
        var it = ranges.keySet().iterator();
        while (it.hasNext()) {
            int l = it.next();
            int r = ranges.get(l);
            if (left <= l && r <= right)
                it.remove();
        }
    }
}
```

## 参考

- [秒懂力扣区间题目：重叠区间、合并区间、插入区间](https://mp.weixin.qq.com/s/ioUlNa4ZToCrun3qb4y4Ow)
- [【C++】【高频考题】剑指 Offer II 074. 合并区间 题解：排序、扫描线基础题 - 合并区间 - 力扣（LeetCode）](https://leetcode.cn/problems/SsGoHC/solution/c-by-algo-goer-gzym/)

