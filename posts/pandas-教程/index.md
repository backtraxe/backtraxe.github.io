# Pandas 教程


Pandas 是一个优秀的数据处理库。

<!--more-->

```python
import pandas as pd
```

## 数据结构

### DataFrame

```python
df = pd.DataFrame({"c1": [11, 21, 31],
                   "c2": [12, 22, 32],
                   "c3": [13, 23, 33]},
                  index=["r1", "r2", "r3"])
```

```python
df = pd.DataFrame([[11, 12, 13],
                   [21, 22, 23],
                   [31, 32, 33]],
                  index=['r1', 'r2', 'r3'],
                  columns=['c1', 'c2', 'c3'])
```

```python

```

### Series

### 读取数据

```python
pd.read_csv()
```

## 处理缺失数据

```python
df.dropna()
df.fillna(value)
```

## 参考

1. [User Guide — pandas documentation](https://pandas.pydata.org/docs/user_guide/index.html)

