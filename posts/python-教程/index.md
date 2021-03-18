# Python 教程


Python 是现在最受欢迎的语言。

<!--more-->

## dict

```python
dict()                                 # {}
dict(a=1, b=2, c=3)                    # {'a': 1, 'b': 2, 'c': 3}
dict(zip(['a', 'b', 'c'], [1, 2, 3]))  # {'a': 1, 'b': 2, 'c': 3}
dict([('a', 1), ('b', 2), ('c', 3)])   # {'a': 1, 'b': 2, 'c': 3}
```

## Python 内置函数

```python
abs(x)                    # 返回绝对值。若参数为复数，则返回复数的模。
divmod(a, b)              # 返回 tuple(a // b, a % b)
input(words)              # 打印 words，读取用户输入，返回 str 类型
ord(c)                    # 返回字符对应的 ASCII 数值，或者 Unicode 数值
chr(i)                    # 返回整数（0～255，10进制或16进制）对应的 ASCII 字符。
bin(i)                    # 返回一个整数（int 或 long int）的二进制表示，str 类型。
complex(real=0, image=0)  # 复数

```

### staticmethod

声明静态方法，即可以不实例化类而直接调用该方法。类中类也可以。

```python
class C(object):
    @staticmethod
    def f(arg1, arg2, ...):
        ...
```

### enumerate

将一个可遍历的数据对象（如列表、元组或字符串）组合为一个索引序列，同时列出数据和下标，一般用在 for 循环中。

`enumerate(sequence, start=0)`

```python
list(enumerate([1, 2, 3]))       # [(0, 1), (1, 2), (2, 3)]
list(enumerate((1, 2, 3)))       # [(0, 1), (1, 2), (2, 3)]
list(enumerate('abc'))           # [(0, 'a'), (1, 'b'), (2, 'c')]
list(enumerate('abc', start=1))  # [(1, 'a'), (2, 'b'), (3, 'c')]
```

### int

- 若 x 为数字，返回整数部分。（不能有 base 参数，否则报错）
- 若 x 为字符串，则将 x 视为 base 进制的数，返回转换为 10 进制后的数。（x 不能为小数或不存在的数，否则报错）

`int(x, base=10)`

```python
int()           # 0
int(3.9)        # 3
int(-3.9)       # -3
int('10', 2)    # 2
int('0xA', 16)  # 10
int('aB', 16)   # 171
```

## 常用内置库

### random

`random`库

```python

```

### os

`os`模块常用来用来处理文件和目录。

```python
import os

os.chdir(path)                # 切换目录，相当于 cd
os.chmod(path, mode)          # 改变文件权限，相当于 chmod

os.getcwd()                   # 返回当前目录绝对路径，相当于 pwd
os.listdir(path)              # 返回文件夹下所有文件或文件夹的名字的列表，相当于 ls

os.open(file, flags[, mode])  # 打开文件，并且设置打开选项

os.mkdir(path[, mode])        # 以权限 mode (int) 创建一个名或路径为 path 的空文件夹，默认 mode 是 0777 (八进制)

os.remove(path)               # 删除文件，不能删除文件夹
os.rmdir(path)                # 删除空文件夹
os.removedirs(path)           # 递归删除空文件夹

os.rename(src, dst)           # 重命名，原名 src ，改后 dst
```

#### os.path

`os.path`模块主要用于获取文件的属性。

```python
os.path.exists(path)                 # 判断路径是否存在
os.path.isdir(path)                  # 判断路径是否为目录
os.path.abspath(path)                # 返回绝对路径
os.path.dirname(path)                # 返回文件路径
os.path.basename(path)               # 返回文件名
os.path.commonprefix(pathList)       # 返回多个路径的公共最长路径
os.path.join(path1[, path2[, ...]])  # 路径合并

# 返回上一级路径
# 'A/B' -> 'A'
# 'A' -> ''
os.path.dirname(path)

# 路径分割，返回 tuple(dirname, basename)
# 'A/B/C' -> ('A/B', 'C')
os.path.split(path)

# 路径分割，返回 tuple(pathname, extension)
# 'A/B/C.exe' -> ('A/B/C', '.exe')
os.path.splitext(path)
```

### 文件操作

```python
open()
```

## tricks

### python 自动给数字前面补 0 的方法

```python
s1 = "12"
s1.zfill(4)  # "0012"

s2 = "-12"
s2.zfill(4)  # "-0012"

a = 12
'%04d' % a   # "0012"
```

## Q&A

### UnicodeDecodeError

问题描述:

`UnicodeDecodeError: 'gbk' codec can't decode byte 0xad in position 7: illegal multibyte sequence`

解决方案:

将`open(filename, 'r')`改为`open(filename, 'r', encoding='utf-8')`

