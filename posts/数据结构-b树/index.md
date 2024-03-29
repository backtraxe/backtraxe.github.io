# 数据结构-B树


<!--more-->

又称**多路平衡查找树**，规定B树的阶 $m$ 为每个结点的最大孩子数量。

- 每个结点最多 $m$ 个孩子。
- 若根结点不是叶结点，则至少有两个孩子。
- 除根结点外，每个非叶结点至少有 $\lceil \frac{m}{2} \rceil$ 个孩子。
- 结点中关键字从小到大排序，关键字两侧为指向孩子的指针。
- 关键字左侧指针指向的子树中的关键字均小于该关键字，右侧则大于。
- 所有叶结点在同一层，不存储信息（空结点）。

**高度**

对任意一棵包含 $n$ 个关键字、高度为 $h$、阶数为 $m$ 的B树（不考虑空的叶结点）：

- 下界：每层关键字越多，则高度越矮。B树中每个结点最多有 $m$ 棵子树，$m-1$ 个关键字，所以

{{< math >}}
$$
\begin{align}
n&\le(m-1)(1+m+m^2+\cdots+m^{h-1})=m^h-1 \\
h&\ge\log_m(n+1)
\end{align}
$$
{{< /math >}}

- 上界：每层关键字越少，则高度越高。B树中根结点最少1个关键字，非叶结点最少 $\lceil \frac{m}{2} \rceil$ 棵子树， $\lceil \frac{m}{2} \rceil-1$ 个关键字，所以

{{< math >}}
$$
\begin{align}
n&\ge1+2\times(\lceil \frac{m}{2} \rceil-1)(1+\lceil \frac{m}{2} \rceil+\lceil \frac{m}{2} \rceil^2+\cdots+\lceil \frac{m}{2} \rceil^{h-2})=2\times\lceil \frac{m}{2} \rceil^{h-1}-1 \\
h&\le\log_{\lceil \frac{m}{2} \rceil}(\frac{n+1}{2})+1
\end{align}
$$
{{< /math >}}

**查找**

1. 查找结点。将结点信息读入内存。
2. 结点中查找关键字。在结点关键字表中二分查找，未找到则查找对应子树。
3. 若子树为空，则查找失败。

**插入**

1. 定位。利用查找算法，找出插入该关键字的最底层中的某个非叶结点。
2. 插入。除了根结点，每个结点的关键字个数都在区间 $[\lceil \frac{m}{2} \rceil-1,m-1]$ 内。若插入后的结点关键字个数在范围内，则可以直接插入，否则必须对结点进行分裂。
3. 分裂。将待分裂结点从中间位置（$\lceil \frac{m}{2} \rceil$）将其中的关键宇分为两部分，左部分包含的关键字放在原结点中，右部分包含的关键字放到新结点中，中间位置（$\lceil \frac{m}{2} \rceil$）的结点插入原结点的父结点。若此时导致其父结点的关键字个数也超过了上限，则继续进行这种分裂操作，直至这个过程传到根结点为止，进而导致B树高度增1。

**删除**

1. 定位。
2. 删除。
3. 借位。

