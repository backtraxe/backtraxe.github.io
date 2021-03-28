# 贝叶斯分类器


<!--more-->

对分类任务来说，在所有相关概率都已知的理想情形下，贝叶斯决策论（Bayesian decision theory）考虑如何基于这些概率和误判损失来选择最优的类别标记。

以多分类任务为例。

假设有$N$种可能的类别标记，即$Y=\{c_1,c_2,...,c_N\}$，$\lambda_{ij}$是将一个真实标记为$c_j$的样本**误分类**为$c_i$所产生的损失。

基于后验概率$P(c_i|x)$可获得将样本$x$分类为$c_i$所产生的期望损失（expected loss），即在样本$x$上的“条件风险“（conditional risk）：$R(c_i|x)=\sum_{j=1}^N\lambda_{ij}P(c_j|x)$

我们要找到一个判定准则$h:X\rightarrow Y$以最小化总体风险$R(h)=\mathbb{E}_x[R(h(x)|x)]$

显然，对每个样本$x$，若$h$能最小化条件风险$R(h(x)|x)$，则总体风险$R(h)$也将最小化。

贝叶斯判定准则（Bayes decision rule）：为最小化总体风险，只需在每个样本上选择那个能使条件风险$R(c|x)$最小的类别标记，即$h^*(x)=argmin_{c\in y}R(c|x)$

$h^*(x)$称为贝叶斯最优分类器（Bayes optional classifier），与之对应的总体风险$R(h^*)$称为贝叶斯风险（Bayes risk）。

$1-R(h^*)$反映了分类器所能达到的最好性能，即通过机器学习所能产生的模型精度的理论上限。

若目标是最小化分类错误率，则误判损失$\lambda_{ij}$可写为

$\begin{equation} \lambda_{ij}=\left\{ \begin{aligned} &0,if \ i=j \\ &1,otherwise \end{aligned} \right.  \end{equation}$

此时，条件风险$R(c|x)=1-P(c|x)$，则最小化分类错误率的贝叶斯最优分类器为$h^*(x)=argmax_{c \in y}P(c|x)$，即对每个样本$x$，选择能使后验概率$P(c|x)$最大的类别标记。

使用贝叶斯判定准则来最小化决策风险，首先需要获得后验概率$P(c|x)$，这在现实中很难获得。

机器学习所要实现的是基于有限的训练样本集尽可能准确的估计出后验概率$P(c|x)$

1. 给定$x$，通过直接建模$P(c|x)$来预测$c$，这样得到的是”判别式模型“（discriminative models），如决策树，BP神经网络，支持向量机等
2. 先对联合概率密度分布$P(x,c)$建模，然后得出$P(c|x)$，这样得到的是”生成式模型“（generative models）

对于生成式模型来说，$P(c|x)=\frac{P(x,c)}{P(x)}$，基于贝叶斯定理，$P(c|x)$可写为$P(c|x)=\frac{P(c)P(x|c)}{P(x)}$

- $P(c)$是类”先验“（prior）概率
- $P(x|c)$是样本$x$相对于类标记$c$的类条件概率（class-conditional probability），或称为”似然“（likelihood）
- $P(x)$是用于归一化的”证据“（evidence）因子

对于给定样本$x$，证据因子$P(x)$与类标记无关，因此估计$P(c|x)$的问题就转化为如何基于训练数据$D$来估计先验$P(c)$和似然$P(x|c)$。

## 唐雨迪

**正向概率**：初始条件已知，求某个事件发生的概率。

​					例：求从一袋球中拿出黑球的概率。

**逆向概率**：已知结果来推测初始条件。

​					例：已知拿出的是黑球，求这球是从A袋还是B袋拿出的概率。


**贝叶斯公式**：$P(A|B)=\frac{P(AB)}{P(B)}=\frac{P(A)P(B|A)}{P(B)}=\frac{P(A)P(B|A)}{P(A)P(B|A)+P(\bar A)P(B|\bar A)}$


**先验概率（Prior Probability）**：
**最大似然估计**：
**奥卡姆剃刀**：

## StatQuest

**朴素贝叶斯**：假设特征之间相互独立，即$P(AB)=P(A)P(B)$。
**多项式朴素贝叶斯分类器（Multinomial Naive Bayes Classifier）**：
**高斯朴素贝叶斯分类器（Gaussian Naive Bayes Classification）**：

垃圾邮件分类：

正常邮件（Normal）：8封
- Dear：8
- Friend：5
- Lunch：3
- Money：1

$P(Dear|Normal)=\frac{8}{17} \\
P(Friend|Normal)=\frac{5}{17} \\
P(Lunch|Normal)=\frac{3}{17} \\
P(Money|Normal)=\frac{1}{17}$

垃圾邮件（Spam）：4封
- Dear：2
- Friend：1
- Lunch：0
- Money：4

$P(Dear|Spam)=\frac{2}{7} \\
P(Friend|Spam)=\frac{1}{7} \\
P(Lunch|Spam)=\frac{0}{7} \\
P(Money|Spam)=\frac{4}{7}$

**先验概率（Prior Probability）**：

$P(Normal)=\frac{8}{12} \\
P(Spam)=\frac{4}{12}$

**出现Dear Friend的邮件是正常邮件的概率：**

$P(Normal|Dear,Friend)=\frac{P(Normal,Dear,Friend)}{P(Dear,Friend)}=\frac{P(Normal)P(Dear,Friend|Normal)}{P(Dear,Friend|Normal)+P(Dear,Friend|Spam)}$

**朴素贝叶斯：假设特征之间相互独立**，这里忽视单词顺序。

$P(Dear,Friend) = P(Dear)P(Friend) \\
P(Dear,Friend|Normal) = P(Dear|Normal)P(Friend|Normal) \\
P(Dear,Friend|Spam) = P(Dear|Spam)P(Friend|Spam)$

则：$P(Normal|Dear,Friend)=\frac{P(Normal)P(Dear|Normal)P(Friend|Normal)}{P(Normal)P(Dear|Normal)P(Friend|Normal)+P(Spam)P(Dear|Spam)P(Friend|Spam)}\approx 0.87$

**出现Lunch Money Money Money Money的邮件是垃圾邮件的概率：**

$P(Spam|Lunch,Money,Money,Money,Money)=\frac{P(Spam)P(Lunch|Spam)P^4(Money|Spam)}{P(Spam)P(Lunch|Spam)P^4(Money|Spam)+P(Normal)P(Lunch|Normal)P^4(Money|Normal)}=0$

这不符合实际情况。

**为了防止词频出现0，设置$\alpha=1$，即每个词的词频增加$\alpha$个。**

**概率（Probability）**：
**可能性（Likelihood）：**

## 阮一峰

贝叶斯分类是一种分类算法的总称，这种算法均以贝叶斯定理为基础，故统称为贝叶斯分类。贝叶斯分类器的分类原理是通过某对象的先验概率，利用贝叶斯公式计算出其后验概率，即该对象属于某一类的概率，选择具有最大后验概率的类作为该对象所属的类。

- $n$项特征：$F_1,F_2,...F_n$
- $m$个类别：$C_1,C_2,...,C_m$

贝叶斯分类器就是计算出概率最大的那个分类，即求$P(C|F_1F_2...F_n)=\frac{P(C)P(F_1F_2...F_n|C)}{P(F_1F_2...F_n)}$的最大值。

由于$P(F_1F_2...F_n)$对于所有的类别都是相同的，可以省略，问题就变成了找出使$P(C)P(F_1F_2...F_n|C)$的值最大的类别C。

**朴素贝叶斯分类器**

朴素贝叶斯分类器假设所有特征都彼此独立，因此$P(C)P(F_1F_2...F_n|C)=P(C)P(F_1|C)P(F_2|C)...P(F_n|C)$，可计算出每个类别对应的概率，找出概率最大的那个类。

**连续值变为离散值**

1. 划分有限个区间。（样本数量少一般难以划分）
2. 假设分布状况。(例如：根据样本计算均值$\mu$和方差$\sigma^2$，根据正态分布进行计算)

$正态分布概率密度函数：f(x)=\frac{1}{\sqrt{2\pi}\sigma}e^{-\frac{(x-\mu)^2}{2\sigma^2}}$

## 参考

