# 算法-滑动窗口


<!--more-->

#### 窗口大小可任意调整

```java
// 窗口大小可任意调整
int left = 0;
int right = 0;
int ans = 0;
// [left, right]
while (right < n) {
    // 增大窗口
    // 右端点 right 操作
    // 修改约束值
    while (condition) { // 约束值满足调整窗口的条件
        // 缩小窗口
        // 左端点 left 操作
        // 修改约束值
        left++;
    }
    ans = Math.max(ans, right - left + 1);
    right++;
}
```

#### 窗口大小单调递增

```java
// 窗口大小单调递增
int left = 0;
int right = 0;
// [left, right]
while (right < n) {
    // 增大窗口
    // 右端点 right 操作
    // 修改约束值
    if (condition) { // 约束值满足调整窗口的条件
        // 此时左端点移动最多使窗口大小不变
        // 左端点 left 操作
        // 修改约束值
        left++;
    }
    right++;
}
int ans = right - left;
```

