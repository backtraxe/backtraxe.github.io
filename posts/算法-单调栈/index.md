# 算法-单调栈


<!--more-->

```java
int[] getMonoStack(int[] nums) {
    int n = nums.length;
    int[] ans = new int[n];
    Deque<Integer> stack = new LinkedList<>();
    // for (int i = 0; i < n; i++) { // 左侧
    for (int i = n - 1; i >= 0; i--) { // 右侧
        // 第一个大于等于的元素 stack.peek() < nums[i]
        // 第一个更大的元素 stack.peek() <= nums[i]
        // 第一个小于等于的元素 stack.peek() > nums[i]
        // 第一个更小的元素 stack.peek() >= nums[i]
        if (!stack.isEmpty() && stack.peek() < nums[i]) {
            stack.pop();
        }
        ans[i] = stack.isEmpty() ? -1 : stack.peek();
        stack.push(nums[i]);
    }
    return ans;
}
```

- []()
- []()

