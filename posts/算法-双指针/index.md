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

### 🟩移动零

[283. 移动零](https://leetcode.cn/problems/move-zeroes/)

- 快慢双指针

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int j = 0; // 指向 0
        for (int i = 0; i < nums.length; i++)
            if (nums[i] != 0)
                swap(nums, i, j++);
    }

    void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

### 🟩链表的中间结点

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

### 🟩反转字符串

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

### 🟩有序数组的平方

[977. 有序数组的平方](https://leetcode.cn/problems/squares-of-a-sorted-array/)

- 首尾双指针

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int n = nums.length;
        int[] ans = new int[n];
        int k = n - 1;
        for (int i = 0, j = n - 1; i <= j;) {
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

### 🟨盛最多水的容器

[11. 盛最多水的容器](https://leetcode.cn/problems/container-with-most-water/)

- 首尾双指针

```java
public class Solution {
    public int maxArea(int[] height) {
        int ans = 0;
        for (int l = 0, r = height.length - 1; l < r;) {
            ans = Math.max(ans, Math.min(height[l], height[r]) * (r - l));
            if (height[l] <= height[r]) l++;
            else r--;
        }
        return ans;
    }
}
```

### 🟨删除链表的倒数第 N 个结点

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

### 🟨两数之和 II

[167. 两数之和 II - 输入有序数组](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/)

- 首尾双指针

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        for (int l = 0, r = numbers.length - 1; l < r;) {
            int sum = numbers[l] + numbers[r];
            if (sum == target) return new int[] { l + 1, r + 1 };
            else if (sum > target) r--;
            else l++;
        }
        return null;
    }
}
```

### 🟨两数之和

[1. 两数之和](https://leetcode.cn/problems/two-sum/)

- 首尾双指针

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int n = nums.length;
        int[][] arr = new int[n][2];
        for (int i = 0; i < n; i++) {
            arr[i][0] = nums[i];
            arr[i][1] = i;
        }
        Arrays.sort(arr, (a, b) -> a[0] - b[0]);
        for (int l = 0, r = n - 1; l < r;) {
            int sum = arr[l][0] + arr[r][0];
            if (sum == target) return new int[] { arr[l][1], arr[r][1] };
            else if (sum > target) r--;
            else l++;
        }
        return null;
    }
}
```

### 🟩两数之和 III

[170. 两数之和 III - 数据结构设计](https://leetcode.cn/problems/two-sum-iii-data-structure-design/)

- 首尾双指针

```java
class TwoSum {
    private List<Integer> arr = new ArrayList<>();
    private boolean sorted = true;

    public void add(int number) {
        arr.add(number);
        sorted = false;
    }

    public boolean find(int value) {
        if (!sorted) {
            Collections.sort(arr);
            sorted = true;
        }
        for (int l = 0, r = arr.size() - 1; l < r;) {
            int sum = arr.get(l) + arr.get(r);
            if (sum == value) return true;
            else if (sum > value) r--;
            else l++;
        }
        return false;
    }
}
```

### 🟩两数之和 IV

[653. 两数之和 IV - 输入二叉搜索树](https://leetcode.cn/problems/two-sum-iv-input-is-a-bst/)

- 首尾双指针

```java
class Solution {
    public boolean findTarget(TreeNode root, int k) {
        if (root == null) return false;
        Deque<TreeNode> seq = new ArrayDeque<>(); // 中序遍历
        Deque<TreeNode> rev = new ArrayDeque<>(); // 逆序的中序遍历
        seq.push(root);
        rev.push(root);
        boolean goLeft = true;
        boolean goRight = true;
        while (!seq.isEmpty() && !rev.isEmpty()) {
            if (goLeft) {
                TreeNode p = seq.peek();
                while (p.left != null) {
                    seq.push(p.left);
                    p = p.left;
                }
            }
            if (goRight) {
                TreeNode p = rev.peek();
                while (p.right != null) {
                    rev.push(p.right);
                    p = p.right;
                }
            }
            TreeNode head = seq.peek();
            TreeNode tail = rev.peek();
            if (head == tail) return false;
            int sum = head.val + tail.val;
            if (sum == k) return true;
            else if (sum > k) {
                rev.pop();
                goLeft = false;
                goRight = true;
            } else {
                seq.pop();
                goLeft = true;
                goRight = false;
            }
            if (goLeft && head.right != null) seq.push(head.right);
            else goLeft = false;
            if (goRight && tail.left != null) rev.push(tail.left);
            else goRight = false;
        }
        return false;
    }
}
```

### 🟩小于 K 的两数之和

[1099. 小于 K 的两数之和](https://leetcode.cn/problems/two-sum-less-than-k/)

- 首尾双指针

```java
class Solution {
    public int twoSumLessThanK(int[] nums, int k) {
        Arrays.sort(nums);
        int ans = -1;
        for (int l = 0, r = nums.length - 1; l < r;) {
            if (nums[l] + nums[r] >= k) r--;
            else {
                ans = Math.max(ans, nums[l] + nums[r]);
                l++;
            }
        }
        return ans;
    }
}
```

### 🟨三数之和

[15. 三数之和](https://leetcode.cn/problems/3sum/)

- 首尾双指针

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> ans = new ArrayList<>();
        int n = nums.length;
        for (int i = 0; i < n; i++) {
            // 相同元素只选择第一个
            if (i > 0 && nums[i - 1] == nums[i]) continue;
            for (int j = i + 1, k = n - 1; j < k;) {
                int sum = nums[i] + nums[j] + nums[k];
                if (sum == 0) {
                    ans.add(List.of(nums[i], nums[j++], nums[k--]));
                    // 避免重复
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

### 🟨最接近的三数之和

[16. 最接近的三数之和](https://leetcode.cn/problems/3sum-closest/)

- 首尾双指针

```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        int ans = 0;
        int minDiff = Integer.MAX_VALUE;
        for (int a = 0; a < nums.length; a++) {
            // 重复元素只选第一个
            if (a > 0 && nums[a - 1] == nums[a]) continue;
            for (int b = a + 1, c = nums.length - 1; b < c;) {
                int diff = nums[a] + nums[b] + nums[c] - target;
                if (diff == 0) return target;
                if (Math.abs(diff) < minDiff) {
                    ans = diff + target;
                    minDiff = Math.abs(diff);
                }
                if (diff > 0) c--;
                else b++;
            }
        }
        return ans;
    }
}
```

### 🟨较小的三数之和

[259. 较小的三数之和](https://leetcode.cn/problems/3sum-smaller/)

- 首尾双指针

```java
class Solution {
    public int threeSumSmaller(int[] nums, int target) {
        Arrays.sort(nums);
        int ans = 0;
        int n = nums.length;
        for (int a = 0; a < n; a++) {
            for (int b = a + 1, c = n - 1; b < c;) {
                int sum = nums[a] + nums[b] + nums[c];
                if (sum >= target) c--;
                else {
                    ans += c - b;
                    b++;
                }
            }
        }
        return ans;
    }
}
```

### 🟨四数之和

[18. 四数之和](https://leetcode.cn/problems/4sum/)

- 首尾双指针

```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        Arrays.sort(nums);
        List<List<Integer>> ans = new ArrayList<>();
        int n = nums.length;
        for (int a = 0; a < n; a++) {
            // 重复元素只选第一个
            if (a > 0 && nums[a - 1] == nums[a]) continue;
            for (int b = a + 1; b < n; b++) {
                // 重复元素只选第一个
                if (b > a + 1 && nums[b - 1] == nums[b]) continue;
                for (int c = b + 1, d = n - 1; c < d;) {
                    long sum = (long) nums[a] + nums[b] + nums[c] + nums[d];
                    if (sum == target) {
                        ans.add(List.of(nums[a], nums[b], nums[c++], nums[d--]));
                        // 避免重复
                        while (c < d && nums[c - 1] == nums[c] && nums[d] == nums[d + 1]) {
                            c++;
                            d--;
                        }
                    } else if (sum > target) d--;
                    else c++;
                }
            }
        }
        return ans;
    }
}
```



## 参考
