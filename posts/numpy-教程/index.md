# Numpy 教程


<!--more-->

`import numpy as np`

## 随机种子

- [`np.random.seed(self, seed=None)`](https://numpy.org/devdocs/reference/random/generated/numpy.random.seed.html)：固定随机数的输出结果。

## 创建数组

### 指定值填充

- [`np.empty(shape, dtype=float, order='C', *, like=None)`](https://numpy.org/devdocs/reference/generated/numpy.empty.html)：返回给定形状和类型的新数组，而无需初始化条目。
- [`np.empty_like(prototype, dtype=None, order='K', subok=True, shape=None)`](https://numpy.org/devdocs/reference/generated/numpy.empty_like.html)：返回形状和类型与给定数组相同的新数组。
- [`np.eye(N, M=None, k=0, dtype=<class 'float'>, order='C', *, like=None)`](https://numpy.org/devdocs/reference/generated/numpy.eye.html)：返回单位矩阵。
- [`np.identity(n, dtype=None, *, like=None)`](https://numpy.org/devdocs/reference/generated/numpy.identity.html)：返回单位矩阵。
- [`np.ones(shape, dtype=None, order='C', *, like=None)`](https://numpy.org/devdocs/reference/generated/numpy.ones.html)：返回给定形状和类型的新数组，并用`1`填充。
- [`np.ones_like(a, dtype=None, order='K', subok=True, shape=None)`](https://numpy.org/devdocs/reference/generated/numpy.ones_like.html)：返回形状与类型与给定数组相同的数组，并用`1`填充。
- [`np.zeros(shape, dtype=float, order='C', *, like=None)`](https://numpy.org/devdocs/reference/generated/numpy.zeros.html)：返回给定形状和类型的新数组，并用`0`填充。
- [`np.zeros_like(a, dtype=None, order='K', subok=True, shape=None)`](https://numpy.org/devdocs/reference/generated/numpy.zeros_like.html)：返回形状与类型与给定数组相同的数组，并用`0`填充。
- [`np.full(shape, fill_value, dtype=None, order='C', *, like=None)`](https://numpy.org/devdocs/reference/generated/numpy.full.html)：返回给定形状和类型的新数组，并用`fill_value`填充。
- [`np.full_like(a, fill_value, dtype=None, order='K', subok=True, shape=None)`](https://numpy.org/devdocs/reference/generated/numpy.full_like.html)：返回形状与类型与给定数组相同的数组，并用`fill_value`填充。

### 范围内填充

- [`np.arange([start, ]stop, [step, ]dtype=None, *, like=None)`](https://numpy.org/devdocs/reference/generated/numpy.arange.html)：返回给定间隔内的均匀间隔的值。
- [`np.linspace(start, stop, num=50, endpoint=True, retstep=False, dtype=None, axis=0)`](https://numpy.org/devdocs/reference/generated/numpy.linspace.html)：返回指定间隔内的等间隔数字。
- [`np.logspace(start, stop, num=50, endpoint=True, base=10.0, dtype=None, axis=0)`](https://numpy.org/devdocs/reference/generated/numpy.logspace.html)：返回数以对数刻度均匀分布。

### 随机值填充

- [`np.random.rand(d0, d1, ..., dn)`](https://numpy.org/doc/stable/reference/random/generated/numpy.random.RandomState.rand.html)：返回服从`[0,1)`均匀分布的指定形状的浮点数数组。
- [`np.random.uniform(low=0.0, high=1.0, size=None)`](https://numpy.org/devdocs/reference/random/generated/numpy.random.uniform.html)：返回服从`[low,high)`均匀分布的指定形状的浮点数数组。
- [`np.random.randint(low, high=None, size=None, dtype=int)`](https://numpy.org/doc/stable/reference/random/generated/numpy.random.RandomState.randint.html)：返回服从`[low,high)`均匀分布的指定形状的整数数组。
- [`np.random.randn(d0, d1, ..., dn)`](https://numpy.org/devdocs/reference/random/generated/numpy.random.RandomState.randn.html)：返回服从`N(0,1)`标准正态分布的指定形状的浮点数数组。
- [`np.random.normal(loc=0.0, scale=1.0, size=None)`](https://numpy.org/devdocs/reference/random/generated/numpy.random.normal.html)：返回服从`N(loc,scale)`正态分布的指定形状的浮点数数组。
- [`np.random.choice(a, size=None, replace=True, p=None)`](https://numpy.org/devdocs/reference/random/generated/numpy.random.choice.html)：从数组中随机有放回采样若干次。
- [`np.random.permutation(x)`](https://numpy.org/devdocs/reference/random/generated/numpy.random.permutation.html)：返回随机排列。

## 数组操作

- [`ndarray.T`](https://numpy.org/devdocs/reference/generated/numpy.ndarray.T.html)：返回转置。
- [`np.transpose(a, axes=None)`](https://numpy.org/devdocs/reference/generated/numpy.transpose.html)：返回转置。
- [`np.c_[tup]`](https://numpy.org/devdocs/reference/generated/numpy.c_.html)：列合并。
- [`np.column_stack(tup)`](https://numpy.org/devdocs/reference/generated/numpy.column_stack.html)：列合并。
- [``]()：行合并。
- [``]()：行合并。
- [``]()：
- [``]()：
- [``]()：

