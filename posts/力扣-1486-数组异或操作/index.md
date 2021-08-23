# 力扣 1486 数组异或操作


[题目链接](https://leetcode-cn.com/problems/xor-operation-in-an-array/)

<!--more-->

## 方法一：模拟

### 代码

```cpp
class Solution {
public:
    int xorOperation(int n, int start) {
        int ans = start;
        for (int i = 1; i < n; ++i) {
            ans ^= (start + i * 2);
        }
        return ans;
    }
};
```

### 复杂度分析

- 时间复杂度：$ O(n) $
- 空间复杂度：$ O(1) $

## 方法二：数学

### 思路

首先介绍异或运算：

- 对于单个比特的异或，**相同为0，相异为1**，即：
    - $ 0 \oplus 0 = 0 $
    - $ 0 \oplus 1 = 1 $
    - $ 1 \oplus 0 = 1 $
    - $ 1 \oplus 1 = 0 $
- 对于整数的异或，按最低位对齐，相同位置的比特进行异或运算，如：
    - $ \ \ \ \ 0001 \ (1) $
    - $ \underline{\oplus \ 0011 \ (3)} $
    - $ \ \ \ \ 0010 \ (2) $

异或运算满足以下基本性质：

1. $ a \oplus a = 0 $
1. $ a \oplus 0 = a $
1. $ a \oplus b = b \oplus a $
1. $ (a \oplus b) \oplus c = a \oplus (b \oplus c) $

可以推导出如下性质：

1. $ a \oplus b \oplus b = a $
1. $ \forall i \in \mathbb{Z}, 有 \underbrace{a \oplus 0 \oplus a \oplus a \oplus \cdots \oplus 0 \oplus a}_{2i+1个a} = a $
1. $ \forall i \in \mathbb{Z}, 有 \underbrace{a \oplus 0 \oplus a \oplus a \oplus \cdots \oplus 0 \oplus a}_{2i个a} = 0 $
1. $ \forall i \in \mathbb{Z}, 有 4i \oplus (4i+1) \oplus (4i+2) \oplus (4i+3) = 0 $

回到本题，我们需要计算 $ start \oplus (start+2) \oplus (start+4) \oplus \cdots \oplus (start+2(n-1)) $，观察公式可以知道每一项奇偶性相同，因此它们的二进制表示中的最低位或者均为1或均为0。

于是我们可以把参与运算的数的二进制位的最低位提取出来单独处理。当且仅当start为奇数且n也为奇数时，结果才为奇数，即最低位为1。令 $ e = n \oplus start \oplus 1 $。

此时不考虑start的最后一位，我们将start**右移一位**，令 $ s = \lfloor \frac{start}{2} \rfloor $，公式转化为 $ s \oplus (s+1) \oplus (s+2) \oplus \cdots \oplus (s+n-1) + e $。

这样我们可以自定义函数`sumXor(x)`来计算 $ 0 \oplus 1 \oplus 2 \oplus \cdots \oplus x $，根据上面异或运算的**推导性质第4条**，可以得出如下结论：

- $ 当x=4i + 0,i \in \mathbb{Z}时，sumXor(x) = x $
- $ 当x=4i + 1,i \in \mathbb{Z}时，sumXor(x) = (x-1) \oplus x = 1 $
- $ 当x=4i + 2,i \in \mathbb{Z}时，sumXor(x) = (x-2) \oplus (x-1) \oplus x = x + 1 $
- $ 当x=4i + 3,i \in \mathbb{Z}时，sumXor(x) = 0 $

所以，最终结果为 $ [(sumXor(s+n-1) \oplus sumXor(s-1)) \times 2] \oplus e $。

### 代码

```cpp
class Solution {
public:
    int xorOperation(int n, int start) {
        int s = start >> 1;
        int e = n & start & 1;
        int ret = sumXor(s - 1) ^ sumXor(s + n - 1);
        return (ret << 1) | e;
    }

    int sumXor(int x) {
        if (x % 4 == 0) return x;
        else if (x % 4 == 1) return 1;
        else if (x % 4 == 2) return x + 1;
        return 0;
    }
};
```

### 复杂度分析

- 时间复杂度：$ O(1) $
- 空间复杂度：$ O(1) $

