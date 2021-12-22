# Numpy 教程


<!--more-->

`import numpy as np`

## 一维数组

```python
np.array([1, 2, 3])                   # array([1, 2, 3])

# numpy.arange([start, ]stop, [step, ]dtype=None, *, like=None)
np.arange(stop=3)                     # array([0, 1, 2])
np.arange(start=0, stop=3)            # array([0, 1, 2])
np.arange(start=0, stop=3, step=1)    # array([0, 1, 2])

# numpy.linspace(start, stop, num=50, endpoint=True, retstep=False, dtype=None, axis=0)
np.linspace(start=0, stop=10, num=3)  # array([ 0.,  5., 10.])
```

## 多维数组

```python
# 全 0 数组
np.zeros((2, 3))
# array([[0., 0., 0.],
#        [0., 0., 0.]])

# 全 1 数组
np.ones((2, 3))
# array([[1., 1., 1.],
#        [1., 1., 1.]])

# 指定值填充
np.full((2, 3), 2)
# array([[2, 2, 2],
#        [2, 2, 2]])
np.full((2, 3), [1, 2, 3])
# array([[1, 2, 3],
#        [1, 2, 3]])
```

```python
# 单位矩阵
np.eye(2)
# array([[1., 0.],
#        [0., 1.]])

# 伪单位矩阵
np.eye(3, k=1)
# array([[0., 1., 0.],
#        [0., 0., 1.],
#        [0., 0., 0.]])
```

