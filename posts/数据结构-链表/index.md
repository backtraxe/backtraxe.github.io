# 数据结构-链表


<!--more-->

## 1.实现

### 1.1 单向链表

```java
class ListNode {
    int val;
    ListNode next;

    public ListNode() {}

    public ListNode(int val) {
        this.val = val;
    }

    public ListNode(int val, ListNode next) {
        this.val = val;
        this.next = next;
    }
}
```

```java
class SingleLinkedList {
    // 哨兵结点、虚拟头结点，存储结点数量
    ListNode dummy = new ListNode(0);
    // 尾结点
    ListNode tail = dummy;

    public void add(int val) {
        // 向链表尾部添加结点
        tail.next = new ListNode(val);
        tail = tail.next;
        dummy.val++;
    }

    public boolean remove() {
        // 删除链表尾结点
        if (isEmpty()) return false;
        ListNode node = dummy;
        while (node.next != tail)
            node = node.next;
        node.next = null;
        tail = node;
        dummy.val--;
        return true;
    }

    public boolean isEmpty() {
        return tail == dummy;
    }

    public int size() {
        return dummy.val;
    }
}
```

### 1.2 双向链表

```java
class ListNode {
    int val;
    ListNode prev, next;

    public ListNode() {}

    public ListNode(int val) {
        this.val = val;
    }

    public ListNode(int val, ListNode prev, ListNode next) {
        this.val = val;
        this.prev = prev;
        this.next = next;
    }
}
```

```java
class DoubleLinkedList {
    // 哨兵结点、虚拟头/尾结点，存储结点数量
    ListNode dummy = new ListNode(0);

    public DoubleLinkedList() {
        // 初始化
        dummy.prev = dummy;
        dummy.next = dummy;
    }

    public void add(int val) {
        // 向链表尾部添加结点
        dummy.prev.next = new ListNode(val, dummy.prev, dummy);
        dummy.prev = dummy.prev.next;
        dummy.val++;
    }

    public boolean remove() {
        // 删除链表尾结点
        if (isEmpty()) return false;
        dummy.prev.prev.next = dummy;
        dummy.prev = dummy.prev.prev;
        dummy.val--;
        return true;
    }

    public boolean isEmpty() {
        return dummy.next == dummy;
    }

    public int size() {
        return dummy.val;
    }
}
```

### 1.3 链表遍历

- 递归

```java
void traverse(TreeNode head) {
    if (head == null) return;
    // 前序
    traverse(head.next);
    // 后序
}
```

- 迭代

```java
ListNode p = head;
while (p != null) {
    p = p.next;
}
```

## 2. 实战

### 2.1 小技巧

- 虚拟头结点
- 快慢指针
- 二路归并
- k路归并
- 递归

### 2.2 简单

#### 2.2.1 合并两个有序链表

[21. 合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/)

- 递归

```java
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        if (list1 == null) return list2;
        if (list2 == null) return list1;
        if (list1.val < list2.val) {
            list1.next = mergeTwoLists(list1.next, list2);
            return list1;
        } else {
            list2.next = mergeTwoLists(list1, list2.next);
            return list2;
        }
    }
}
```

- 二路归并

```java
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        ListNode dummy = new ListNode();
        ListNode p = dummy;
        while (list1 != null && list2 != null) {
            if (list1.val < list2.val) {
                p.next = list1;
                list1 = list1.next;
            } else {
                p.next = list2;
                list2 = list2.next;
            }
            p = p.next;
        }
        p.next = list1 == null ? list2 : list1;
        return dummy.next;
    }
}
```

#### 2.2.2 有序链表去重

[83. 删除排序链表中的重复元素](https://leetcode.cn/problems/remove-duplicates-from-sorted-list/)

- 递归

```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        if (head == null || head.next == null) return head;
        head.next = deleteDuplicates(head.next);
        if (head.val == head.next.val) head = head.next;
        return head;
    }
}
```

- 迭代

```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        if (head == null) return head;
        ListNode cur = head;
        while (cur.next != null) {
            if (cur.val == cur.next.val)
                cur.next = cur.next.next;
            else
                cur = cur.next;
        }
        return head;
    }
}
```

#### 2.2.3 判断链表中是否有环

[141. 环形链表](https://leetcode.cn/problems/linked-list-cycle/)

- 快慢指针

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        while (fast != null && fast.next != null) {
            // 慢指针走一步，快指针走两步
            fast = fast.next.next;
            slow = slow.next;
            if (slow == fast) return true; // 快慢指针相遇，说明含有环
        }
        return false;
    }
}
```

#### 2.2.4 找出两个链表的交点

[160. 相交链表](https://leetcode.cn/problems/intersection-of-two-linked-lists/)

- 双指针

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if (headA == null || headB == null) return null;
        ListNode pA = headA;
        ListNode pB = headB;
        while (pA != pB) {
            // 消除长度差，pA 和 pB 只可能在 交点 或 null 处相等
            pA = pA == null ? headB : pA.next;
            pB = pB == null ? headA : pB.next;
        }
        return pA;
    }
}
```

#### 2.2.5 删除所有满足条件的节点

[203. 移除链表元素](https://leetcode.cn/problems/remove-linked-list-elements/)

- 递归

```java
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        if (head == null) return null;
        head.next = removeElements(head.next, val);
        return head.val == val ? head.next : head;
    }
}
```

- 迭代

```java
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        ListNode dummy = new ListNode(0, head);
        ListNode p = dummy;
        while (p.next != null) {
            if (p.next.val == val) p.next = p.next.next;
            else p = p.next;
        }
        return dummy.next;
    }
}
```

#### 2.2.6 反转链表

[206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/)

- 递归

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) return head;
        ListNode newHead = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        return newHead;
    }
}
```

- 迭代

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode dummy = new ListNode(0);
        while (head != null) {
            ListNode next = head.next;
            head.next = dummy.next;
            dummy.next = head;
            head = next;
        }
        return dummy.next;
    }
}
```

#### 2.2.7 判断是否回文链表

[234. 回文链表](https://leetcode.cn/problems/palindrome-linked-list/)

- 递归

```java
class Solution {
    // 一前一后两个结点比较来判断是否回文串
    ListNode front; // 存储前面的结点

    public boolean isPalindrome(ListNode head) {
        front = head;
        return isPalindrome1(head);
    }

    boolean isPalindrome1(ListNode back) {
        // 递归后面的结点
        if (back == null) return true;
        // 递归、剪枝
        if (!isPalindrome1(back.next)) return false;
        // 后序，递归栈中结点是逆序的
        // 第一次进入该代码时是链表的尾结点，应和第一个结点比较
        if (front.val != back.val) return false;
        // 比较完毕，将前面的结点后移一位，以和后面的结点对应
        front = front.next;
        return true;
    }
}
```

- 迭代

```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        // 1. 快慢指针找中点，slow 奇数在中点，偶数在中间偏右
        ListNode fast = head;
        ListNode slow = head;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        // 2. 逆序后半链表
        ListNode prev = null;
        ListNode curr = slow;
        while (curr != null) {
            ListNode next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        // 3. 前后链表回文比较并恢复后半链表
        boolean ans = true;
        curr = prev;
        prev = null;
        while (curr != null) {
            if (head.val != curr.val) ans = false;
            ListNode next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
            head = head.next;
        }
        return ans;
    }
}
```

#### 2.2.8 删除给定节点

[237. 删除链表中的节点](https://leetcode.cn/problems/delete-node-in-a-linked-list/)

- 用下个结点替换当前结点，然后删除下个结点。

```java
class Solution {
    public void deleteNode(ListNode node) {
        node.val = node.next.val;
        node.next = node.next.next;
    }
}
```

#### 2.2.9 找出链表的中间节点

[876. 链表的中间结点](https://leetcode.cn/problems/middle-of-the-linked-list/)

- 快慢指针

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

#### 2.2.10 二进制链表转整数

[1290. 二进制链表转整数](https://leetcode.cn/problems/convert-binary-number-in-a-linked-list-to-integer/)

```java
class Solution {
    public int getDecimalValue(ListNode head) {
        int ans = 0;
        while (head != null) {
            ans = ans * 2 + head.val;
            head = head.next;
        }
        return ans;
    }
}
```

#### 2.2.11 删除 M 个节点之后的 N 个节点

[1474. 删除链表 M 个节点之后的 N 个节点](https://leetcode.cn/problems/delete-n-nodes-after-m-nodes-of-a-linked-list/)

```java
class Solution {
    public ListNode deleteNodes(ListNode head, int m, int n) {
        ListNode p1 = head;
        ListNode p2 = null;
        while (p1 != null) {
            int mm = m;
            int nn = n;
            while (--mm > 0 && p1 != null) p1 = p1.next;
            if (p1 == null) break;
            p2 = p1.next;
            while (nn-- > 0 && p2 != null) p2 = p2.next;
            p1.next = p2;
            p1 = p2;
        }
        return head;
    }
}
```

#### 2.2.12 逆序打印链表

[剑指 Offer 06. 从尾到头打印链表](https://leetcode.cn/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

- 递归

```java
class Solution {
    public int[] reversePrint(ListNode head) {
        List<Integer> list = new ArrayList<>();
        traverse(head, list);
        return list.stream().mapToInt(i -> i).toArray();
    }

    void traverse(ListNode head, List<Integer> list) {
        if (head == null) return;
        traverse(head.next, list);
        list.add(head.val);
    }
}
```

- 迭代

```java
class Solution {
    public int[] reversePrint(ListNode head) {
        ListNode p = head;
        int len = 0;
        while(p != null){
            len++;
            p = p.next;
        }
        int[] ans = new int[len];
        p = head;
        while(p != null){
            ans[len - 1] = p.val;
            len--;
            p = p.next;
        }
        return ans;
    }
}
```

#### 2.2.13 删除第一个满足条件的节点

[剑指 Offer 18. 删除链表的节点](https://leetcode.cn/problems/shan-chu-lian-biao-de-jie-dian-lcof/)

- 递归

```java
class Solution {
    public ListNode deleteNode(ListNode head, int val) {
        if (head == null) return null;
        if (head.val == val) return head.next;
        head.next = deleteNode(head.next, val);
        return head;
    }
}
```

- 迭代

```java
class Solution {
    public ListNode deleteNode(ListNode head, int val) {
        ListNode dummy = new ListNode(0, head);
        ListNode prev = dummy;
        ListNode curr = head;
        while (curr != null) {
            if (curr.val == val) {
                prev.next = curr.next;
                break;
            }
            prev = curr;
            curr = curr.next;
        }
        return dummy.next;
    }
}
```

#### 2.2.14 找出倒数第 k 个节点

[剑指 Offer 22. 链表中倒数第k个节点](https://leetcode.cn/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

#### 2.2.15 未排序链表去重

[面试题 02.01. 移除重复节点](https://leetcode.cn/problems/remove-duplicate-node-lcci/)

### 2.3 中等

### 2.4 困难

- [合并k个升序链表](https://leetcode.cn/problems/merge-k-sorted-lists/)：K指针 + 优先队列
- [返回链表中倒数第k个节点](https://leetcode.cn/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

{{< admonition tip code false >}}
```java
public static ListNode findFromEnd1(ListNode head, int k) {
    // 要求 1 <= k <= n
    ListNode p1 = head, p2 = head;
    // p1 先走 k 步
    for (int i = 0; i < k && p1 != null; i++) {
        p1 = p1.next;
    }
    // 然后 p1 和 p2 同步走，走到头
    // 即 p2 走了 n - k 步，也就是倒数第 k 个
    while (p1 != null) {
        p1 = p1.next;
        p2 = p2.next;
    }
    return p2;
}
```

```java
// 不适用于多测试用例，因为 count 是静态的
// 或者每次调用前将 count 归零
public static count = 0;

public static ListNode findFromEnd2(ListNode head, int k) {
    // 要求 1 <= k <= n
    if (head == null) {
        return null;
    }
    ListNode node = findFromEnd2(head.next, k);
    // 从尾部向前计数
    count++;
    if (count == k) {
        return head;
    }
    return node;
}
```
{{< /admonition >}}

- [删除链表的倒数第n个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

1. 添加虚表头结点。
2. 返回链表的倒数第 k+1 个节点，删除后继结点。

- [找到链表的中间结点](https://leetcode.cn/problems/middle-of-the-linked-list/)：快慢指针，慢走一步，快走两步。
- [找出链表中环的起点](https://leetcode.cn/problems/linked-list-cycle-ii/)

{{< admonition tip code false >}}
```java
ListNode detectCycle(ListNode head) {
    ListNode fast = head, slow = head;
    while (fast != null && fast.next != null) {
        fast = fast.next.next;
        slow = slow.next;
        if (fast == slow) {
            break;
        }
    }
    if (fast == null || fast.next == null) {
        // 无环
        return null;
    }
    /*
       相遇时，slow 走了 k 步，fast 走了 2k 步
       则 k 为环的长度，设相遇点距环起点距离为 m
       则表头到环起点距离为 k - m，相遇点向前走到环起点距离为 k - m
       则将 slow 放到表头，然后 slow 和 fast 同步走
       相遇即为环起点
    */
    // 重新指向头结点
    slow = head;
    // 快慢指针同步前进，相交点就是环起点
    while (slow != fast) {
        fast = fast.next;
        slow = slow.next;
    }
    return slow;
}
```
{{< /admonition >}}

- [找出两个链表的交点](https://leetcode.cn/problems/intersection-of-two-linked-lists/)

{{< admonition tip code false >}}
1. 让 p1 遍历完链表 A 之后开始遍历链表 B，让 p2 遍历完链表 B 之后开始遍历链表 A，这样相当于「逻辑上」两条链表接在了一起。

```java
ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    // p1 指向 A 链表头结点，p2 指向 B 链表头结点
    ListNode p1 = headA, p2 = headB;
    while (p1 != p2) {
        // p1 走一步，如果走到 A 链表末尾，转到 B 链表
        if (p1 == null) {
            p1 = headB;
        } else {
            p1 = p1.next;
        }
        // p2 走一步，如果走到 B 链表末尾，转到 A 链表
        if (p2 == null) {
            p2 = headA;
        } else {
            p2 = p2.next;
        }
    }
    return p1;
}
```

2. 如果把两条链表首尾相连，那么「寻找两条链表的交点」的问题转换成了前面讲的「寻找环起点」的问题。

3. 预先计算两条链表的长度。

```java
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    int lenA = 0, lenB = 0;
    // 计算两条链表的长度
    for (ListNode p1 = headA; p1 != null; p1 = p1.next) {
        lenA++;
    }
    for (ListNode p2 = headB; p2 != null; p2 = p2.next) {
        lenB++;
    }
    // 让 p1 和 p2 到达尾部的距离相同
    ListNode p1 = headA, p2 = headB;
    if (lenA > lenB) {
        for (int i = 0; i < lenA - lenB; i++) {
            p1 = p1.next;
        }
    } else {
        for (int i = 0; i < lenB - lenA; i++) {
            p2 = p2.next;
        }
    }
    // 看两个指针是否会相同，p1 == p2 时有两种情况：
    // 1、要么是两条链表不相交，他俩同时走到尾部空指针
    // 2、要么是两条链表相交，他俩走到两条链表的相交点
    while (p1 != p2) {
        p1 = p1.next;
        p2 = p2.next;
    }
    return p1;
}
```

4. 哈希表存储结点。
{{< /admonition >}}

- [反转链表](https://leetcode.cn/problems/reverse-linked-list/)

{{< admonition tip code false >}}
```java
ListNode reverse(ListNode head) {
    if (head == null || head.next == null) {
        return head;
    }
    // 返回从 head.next 开始的链表的逆序的头结点
    // newHead 即为原链表尾结点
    ListNode newHead = reverse(head.next);
    // head.next 原来是待逆序链表的头结点
    // 逆序后变为尾结点
    // 将 head 作为新的尾结点，实现逆序
    head.next.next = head;
    head.next = null;
    // 返回逆序后链表的头结点
    return newHead;
}
```
{{< /admonition >}}

- [反转链表第m个节点到第n个节点](https://leetcode.cn/problems/reverse-linked-list-ii/)

{{< admonition tip code false >}}
```java
// 后继结点
ListNode successor = null;

ListNode reverseN(ListNode head, int n) {
    if (n == 1) {
        successor = head.next;
        return head;
    }
    ListNode newHead = reverseN(head.next, n - 1);
    head.next.next = head;
    // 将尾结点连接到后继结点上
    head.next = successor;
    return newHead;
}
```

```java
ListNode reverseBetween(ListNode head, int m, int n) {
    if (m == 1) {
        return reverseN(head, n);
    }
    head.next = reverseBetween(head.next, m - 1, n - 1);
    return head;
}
```
{{< /admonition >}}

- [K个一组翻转链表](https://leetcode.cn/problems/reverse-nodes-in-k-group/)

{{< admonition tip code false >}}
```java

```
{{< /admonition >}}

