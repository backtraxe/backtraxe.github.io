# 数据结构-链表


<!--more-->

### 定义

```java
class ListNode {
    int val;
    // 指向后继结点
    ListNode next;
    // 双向链表，指向前驱结点
    ListNode prev;
}

class LinkedList {
    // 头结点，不存放数据
    ListNode head = new ListNode();
    // 双向链表，尾结点，不存放数据
    ListNode tail = new ListNode();
}
```

### 常用方法

- 双指针（快慢指针）
- 虚拟头结点

### 实战

- [合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/)：双指针
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
- [判断链表是否存在环](https://leetcode.cn/problems/linked-list-cycle/)

{{< admonition tip code false >}}
```java
boolean hasCycle(ListNode head) {
    // 快慢指针初始化指向 head
    ListNode slow = head, fast = head;
    // 快指针走到末尾时停止
    while (fast != null && fast.next != null) {
        // 慢指针走一步，快指针走两步
        slow = slow.next;
        fast = fast.next.next;
        // 快慢指针相遇，说明含有环
        if (slow == fast) {
            return true;
        }
    }
    // 不包含环
    return false;
}
```
{{< /admonition >}}

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

