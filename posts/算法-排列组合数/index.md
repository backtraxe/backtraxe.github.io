# 算法-排列组合数


<!--more-->

## 排列数

$A_n^m$ 表示从 $n$ 个不同的数字中选择 $m$ 个数字进行排列，不同排列的数量。

$$
\begin{aligned}
\newline
A_n^0 &= 1 \newline
A_n^1 &= n \newline
A_n^n &= n(n-1)(n-2) \cdots 1 = n! \newline
A_n^m &= n(n-1)(n-2) \cdots (n-m+1) = \frac{n!}{(n-m)!} = \frac{A_n^n}{A_{n-m}^{n-m}} \newline
A_n^m &= n A_{n-1}^{m-1} \newline
A_n^m &= m A_{n-1}^{m-1} + A_{n-1}^m \newline
\newline
\end{aligned}
$$

## 组合数

$C_n^m$ 表示从 $n$ 个不同的数字中选择 $m$ 个数字，不同选择的数量。

$$
\begin{aligned}
\newline
C_n^0 &= C_n^n = 1 \newline
C_n^1 &= n \newline
C_n^m &= C_n^{n-m} \newline
C_n^m &= \frac{n(n-1) \cdots (n-m+1)}{m(m-1) \cdots 1} = \frac{A_n^m}{A_m^m} = \frac{n!}{m!(n-m)!} = \frac{A_n^n}{A_m^m A_{n-m}^{n-m}} \newline
C_n^m &= C_{n-1}^m + C_{n-1}^{m-1} \newline
2^n &= C_n^0 + C_n^1 + \cdots + C_n^n \newline
\newline
\end{aligned}
$$

```java
static long combination(int n, int m) {
    // C(n, m)
    m = Math.max(m, n - m);
    long ans = 1L;
    for (int i = 1; i <= n - m; i++) {
        ans *= m + i;
        ans /= i;
    }
    return ans;
}
```

```java

```

```cpp
ll combine1(ll n,ll m) //计算组合数C(n,m)
{
    ll sum=1; //线性计算
    for(ll i=1,j=n;i<=m;i++,j--)
        sum=sum*j/i;
    return sum;
}
```

```cpp
void extend_gcd(ll a,ll b,ll &d,ll &x,ll &y)
{
    if(!b) d=a,x=1,y=0;
    else
    {
        extend_gcd(b,a%b,d,y,x);
        y-=x*(a/b);
    }
}
ll Mod(ll a,ll b,ll mod)//算出的逆元不对，明天检查一下才行
{
    if(!b) return 1;
    ll ans=Mod(a,b>>1,mod);
    ans=ans*ans%mod;
    if(b&1) ans=ans*a%mod;
    return ans;
}
ll inv(ll a,ll mod)//计算a对mod的逆元
{

    ll d,x,y;
    extend_gcd(a,mod,d,x,y);
    return d==1?(x+mod)%mod:-1;
    //return Mod(a,mod-2,mod);
}
ll combine1(ll n,ll m,ll mod)//计算组合C(n,m)%mod
{
    ll sum=1; //线性计算
    for(ll i=1,j=n; i<=m; i++,j--)
        sum=sum*j*inv(i,mod)%mod;
    return sum;
}
```

## 参考

1. [关于排列组合的一个总结 - 知乎](https://zhuanlan.zhihu.com/p/56326836)

