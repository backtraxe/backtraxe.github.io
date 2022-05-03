# LaTeX 常用数学符号


<!--more-->

标量 - 斜体小写 - $a$

向量 - 粗体小写 - $a$

矩阵 - 粗体大写 - $A$

## 希腊字母

|大写字母|代码|小写字母|代码|变量形式|代码|
|:-:|:-:|:-:|:-:|:-:|:-:|
|||$\alpha$|`\alpha`|
|||$\beta$|`\beta`|
|$\Gamma$|`\Gamma`|$\gamma$|`\gamma`|
|$\Delta$|`\Delta`|$\delta$|`\delta`|
|||$\epsilon$|`\epsilon`|
|$\Theta$|`\Theta`|$\theta$|`\theta`|
|$\Lambda$|`\Lambda`|$\lambda$|`\lambda`|
|||$\mu$|`\mu`|
|$\Pi$|`\Pi`|$\pi$|`\pi`|
|||$\rho$|`\rho`|
|$\Sigma$|`\Sigma`|$\sigma$|`\sigma`|
|$\Phi$|`\Phi`|$\phi$|`\phi`|$\varphi$|`\varphi`|
|$\Psi$|`\Psi`|$\psi$|`\psi`|
|$\Omega$|`\Omega`|$\omega$|`\omega`|

$$\mathcal{L}、\mathbf{L}、\mathbb{L}、\ell、\mathrm{L}、\mathsf{L}、\mathtt{L}、\mathit{L}、\mathfrak{L}、\mathscr{L}$$

## 特殊符号

|符号|代码|解释|
|:-:|:-:|:-:|
|$\partial$|`\partial`|偏导数|
|$\nabla$|`\nabla`|梯度|
|$\ell$|`\ell`||
|$\Complex$|`\Complex` `\cnums`|复数集|
|$\R$|`\Reals` `\reals` `\R`|实数集|
|$\Z$|`\Z`|整数集|
|$\natnums$|`\natnums` `\N`|自然数集|

## 垂直布局

|符号|代码|解释|
|:-:|:-:|:-:|
|$x_n$|`x_n`|下标|
|$e^x$|`e^x`|上标|
|$_u^o$|`_u^o`|上下标|
|$\overset{N}{\sum}$|`\overset{N}{\sum}`|正上标|
|$\underset{i=1}{\sum}$|`\underset{i=1}{\sum}`|正下标|
|$$|``||

## 字体

|符号|代码|解释|
|:-:|:-:|:-:|
|$\sqrt{x}$|`\sqrt{x}`|平方根|
|$\sqrt[3]{x}$|`\sqrt[3]{x}`|三次方根|
|$$|``||

## 逻辑符与集合论

|符号|代码|解释|
|:-:|:-:|:-:|
|$\forall$|`\forall`|所有|
|$\exist$|`\exists` `\exist`|存在|
|$\in$|`\in` `\isin`|属于|
|$\notin$|`\notin`|不属于|
|$\subset$|`\subset`|包含于|
|$\supset$|`\supset`|包含|
|$\emptyset$|`\emptyset` `\empty`|空集|
|$\varnothing$|`\varnothing`|空集|
|$\implies$|`\implies`|充分|
|$\impliedby$|`\impliedby`|必要|
|$\iff$|`\iff`|充分必要|
|$\neg$|`\neg` `\lnot`|非|
|$\lor$|`\lor`|或|
|$\land$|`\land`|与|
|$\because$|`\because`|因为|
|$\therefore$|`\therefore`|所以|
|$\ne$|`\ne` `\neq`|不等于|
|$\approx$|`\approx`|约等于|
|$\coloneqq$|`\coloneqq`|赋值|
|$\gt$|`\gt`|大于|
|$\ge$|`\ge` `\geq`|大于等于|
|$\lt$|`\lt`|小于|
|$\le$|`\le` `\leq`|小于等于|
|$$|``||

## 多元运算符

|符号|代码|解释|
|:-:|:-:|:-:|
|$\sum$|`\sum`|累加|
|$\prod$|`\prod`|累乘|
|$\bigcap$|`\bigcap`|累交|
|$\bigcup$|`\bigcup`|类并|
|$\int$|`\int`|一重积分|
|$\iint$|`\iint`|二重积分|
|$\iiint$|`\iiint`|三重积分|
|$\oint$|`\oint`|一重环路积分|
|$\oiint$|`\oiint`|二重环路积分|
|$\oiiint$|`\oiiint`|三重环路积分|

## 二元运算符

|符号|代码|解释|
|:-:|:-:|:-:|
|$\bmod$|`\bmod`||
|$x \pmod a$|`x \pmod a`||
|$\div$|`\div`||
|$\pm$|`\pm` `\plusmn`||
|$\oplus$|`\oplus`||
|$\otimes$|`\otimes`||
|$$|``||

## 常用数学符号

|符号|代码|解释|
|:-:|:-:|:-:|
|$\lim$|`\lim`||
|$\ln$|`\ln`||
|$\log$|`\log`||
|$\exp$|`\exp`||
|$\max$|`\max`||
|$\min$|`\min`||
|$\argmax$|`\argmax`||
|$\argmin$|`\argmin`||
|$\sin$|`\sin`||
|$\cos$|`\cos`||
|$\tan$|`\tan`||
|$$|``||

## 矩阵

## 一行多个公式

$$
\begin{align}
x&=t & x&=\cos t & x&=t \notag
y&=2t & y&=\sin(t+1) & y&=\sin t
\end{align}
$$

## 大括号多行公式

$$
f(x)=\begin{cases}
2x+1, & \text{if} & x \lt 0; \\
0, & \text{if} & x = 0; \\
x^2-1, & \text{if} & x \gt 0.
\end{cases}
$$

## 参考

1. [Supported Functions - KaTeX](https://katex.org/docs/supported.html)

