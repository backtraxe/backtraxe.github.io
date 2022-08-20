# 算法-双指针


<!--more-->

## 1.首尾双指针

- 要求数组有序。

**两数之和：**

[167. 两数之和 II - 输入有序数组](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/)

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int l = 0;
        int r = numbers.length - 1;
        while (l < r) {
            int sum = numbers[l] + numbers[r];
            if (sum > target) r--;
            else if (sum < target) l++;
            else return new int[] { l + 1, r + 1 };
        }
        return null;
    }
}
```

## 2.快慢双指针

**检测链表中的环：**

[]()

## 3.二路归并

```java

```

## 3.三指针

**荷兰国旗问题：**

[75. 颜色分类](https://leetcode.cn/problems/sort-colors/)

```java
class Solution {
    public void sortColors(int[] nums) {
        int l = 0;
        int m = 0;
        int r = nums.length - 1;
        while (m <= r) {
            if (nums[m] == 0) {
                swap(nums, l, m);
                l++;
                m++;
            } else if (nums[m] == 1) {
                m++;
            } else if (nums[m] == 2) {
                swap(nums, m, r);
                r--;
            }
        }
    }

    static void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

**三向切分快排：**

```java
static void quickSort(int[] arr, int low, int high) {
    // [low, high]
    if (low >= high) return;
    // 1. 随机选取 pivot
    int pivotIndex = low + (int) (Math.random() * (high - low + 1));
    // 2. partition
    int pivot = arr[pivotIndex];
    int l = low;
    int m = low;
    int r = high;
    while (m <= r) {
        if (arr[m] < pivot) {
            swap(arr, l, m);
            l++;
            m++;
        } else if (arr[m] > pivot) {
            swap(arr, m, r);
            r--;
        } else {
            m++;
        }
    }
    // 3. 递归子数组
    quickSort(arr, low, l - 1);
    quickSort(arr, r + 1, high);
}

static void swap(int[] arr, int i, int j) {
    int temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
}
```

## 4. k 路归并

## 实战

### 有序数组的平方

[977. 有序数组的平方](https://leetcode.cn/problems/squares-of-a-sorted-array/)

- 首尾双指针

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int n = nums.length;
        int[] ans = new int[n];
        int i = 0;
        int j = n - 1;
        int k = n - 1;
        while (i <= j) {
            if (Math.abs(nums[i]) > Math.abs(nums[j])) {
                ans[k--] = nums[i] * nums[i++];
            } else {
                ans[k--] = nums[j] * nums[j--];
            }
        }
        return ans;
    }
}
```

### 移动零

[283. 移动零](https://leetcode.cn/problems/move-zeroes/)

- 快慢双指针

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int n = nums.length;
        int j = 0; // 指向 0
        for (int i = 0; i < n; i++) {
            if (nums[i] != 0) {
                swap(nums, i, j);
                j++;
            }
        }
    }

    void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

### 两数之和 II - 输入有序数组

[167. 两数之和 II - 输入有序数组](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/)

- 首尾双指针

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int l = 0;
        int r = numbers.length - 1;
        while (l < r) {
            int sum = numbers[l] + numbers[r];
            if (sum > target) r--;
            else if (sum < target) l++;
            else return new int[] { l + 1, r + 1 };
        }
        return null;
    }
}
```

### 反转字符串

[344. 反转字符串](https://leetcode.cn/problems/reverse-string/)

- 首尾双指针

```java
class Solution {
    public void reverseString(char[] s) {
        for (int l = 0, r = s.length - 1; l < r; l++, r--) {
            char temp = s[l];
            s[l] = s[r];
            s[r] = temp;
        }
    }
}
```

### 链表的中间结点

[876. 链表的中间结点](https://leetcode.cn/problems/middle-of-the-linked-list/)

- 快慢双指针

```java
class Solution {
    public ListNode middleNode(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        return slow;
    }
}
```

### 删除链表的倒数第 N 个结点

[19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

- 快慢双指针

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0, head);
        ListNode front = head;
        while (front != null && n-- > 0) front = front.next;
        ListNode rear = dummy;
        while (front != null) {
            front = front.next;
            rear = rear.next;
        }
        rear.next = rear.next.next;
        return dummy.next;
    }
}
```

## 参考
