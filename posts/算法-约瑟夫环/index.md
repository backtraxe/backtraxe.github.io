# 算法-约瑟夫环


<!--more-->

[找出游戏的获胜者](https://leetcode.cn/problems/find-the-winner-of-the-circular-game/)

## 原理

**倒推法**

1. 还剩`1`个人，此人获胜，下标为

$$f(1,k)=0$$

2. 还剩`2`个人，淘汰第`k`个人，即淘汰下标`(k-1)%2`，下次从`k%2`开始，则获胜者下标为

$$f(2,k)=(f(1,k)+k)\mod 2=(0+k)\mod 2$$

3. 还剩`3`个人，淘汰第`k`个人，即淘汰下标`(k-1)%3`，下次从`k%3`开始，则获胜者下标为

$$f(3,k)=(f(2,k)+k)\mod 3=((0+k)\mod 2+k)\mod 3$$

4. ...

5. `n`个人，淘汰第`k`个人，即淘汰下标`(k-1)%n`，下次从`k%n`开始，则获胜者下标为

$$f(n,k)=(f(n-1,k)+k)\bmod{n}=(\cdots((0+k)\bmod{2}+k)\bmod{3}\cdots)\bmod{n}$$

```java
int findTheWinner(int n, int k) {
    int ans = 0;
    for (int i = 2; i <= n; i++) {
        ans = (ans + k) % i;
    }
    return ans + 1;
}
```

