# 力扣 0015 三数之和


[15. 三数之和](https://leetcode.cn/problems/3sum/)

<!--more-->

## 方法一：排序+双指针+跳过重复项

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> ans = new ArrayList<>();
        int n = nums.length;
        Arrays.sort(nums);
        for (int i = 0; i < n; i++) {
            if (nums[i] > 0) break;
            // 保证第一个数不重复
            if (i > 0 && nums[i - 1] == nums[i]) continue;
            for (int j = i + 1, k = n - 1; j < k;) { // 双指针
                int sum = nums[i] + nums[j] + nums[k];
                if (sum == 0) {
                    ans.add(List.of(nums[i], nums[j], nums[k]));
                    j++;
                    k--;
                    // 保证第二和第三个数不重复
                    while (j < k && nums[j] == nums[j - 1] && nums[k] == nums[k + 1]) {
                        j++;
                        k--;
                    }

                } else if (sum > 0) k--;
                else j++;
            }
        }
        return ans;
    }
}
```

## 方法二：排序+双指针+哈希表去重

- 时间复杂度更高

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> ans = new ArrayList<>();
        HashSet<List<Integer>> set = new HashSet<>();
        Arrays.sort(nums);
        int n = nums.length;
        for (int i = 0; i < n; i++) {
            for (int j = i + 1, k = n - 1; j < k;) {
                int sum = nums[i] + nums[j] + nums[k];
                List<Integer> tuple = List.of(nums[i], nums[j], nums[k]);
                if (sum == 0 && !set.contains(tuple)) {
                    set.add(tuple);
                    ans.add(tuple);
                    j++;
                    k--;
                } else if (sum > 0) k--;
                else j++;
            }
        }
        return ans;
    }
}
```

