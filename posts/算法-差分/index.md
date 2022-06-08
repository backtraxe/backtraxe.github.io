# 算法-差分


<!--more-->

```java
int[] getDiff(int[] nums) {
    int n = nums.length;
    for (int i = n; i > 0; i--) {
        nums[i] -= nums[i - 1];
    }
    return nums;
}
```

