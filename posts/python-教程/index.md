# Python 教程


Python 是现在最受欢迎的语言。

<!--more-->

```python
# 三元运算符
smaller = x if x < y else y

# 其他运算符
'A' in ['A', 'B', 'C']
'D' not in ['A', 'B', 'C']
"hello" is "hello"
"Hello" is not "hello"
# is 对比的是两个变量的内存地址
# == 对比的是两个变量的值
# 地址不可变的类型（str 等），那么 is 和 == 完全等价
# 地址可变的类型（list，dict，tuple 等），两者不等价

# 异常处理
try:
    a = 1 / 0
except ZeroDivisionError:
    print('除数不能为0')

# 指定精度
from decimal import Decimal
decimal.getcontext().prec = 4
c = Decimal(1) / Decimal(3)  # 0.3333

# 获取类型信息
isinstance(1, int)
# type() 不会认为子类是一种父类类型，不考虑继承关系
# isinstance() 会认为子类是一种父类类型，考虑继承关系

# 类型转换
int('520')       # 520
float('520.52')  # 520.52
str(520)         # '520'

# print
# print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False)
# sep   分隔符
# end   结束符
# file  输出的文件
# flush 立即输出，不作缓存

# 数据存储
# 计算机内部使用补码存储数据
# 以 int8 为例
# 原码：第一位为符号位（正0负1），其余位为该数绝对值的二进制表示
# 3  : 00 00 00 11
# -3 : 10 00 00 11
# 反码：正数同原码，负数为原码符号位不变，其余位取反
# 3  : 00 00 00 11
# -3 : 11 11 11 00
# 补码：正数同原码，负数为反码加1
# 3  : 00 00 00 11
# -3 : 11 11 11 01

# 位运算
# 按位与 &
a = 3  # 00 00 00 11
b = 5  # 00 00 01 01
a & b  # 00 00 00 01 : 1
# 按位或 |
a = 3  # 00 00 00 11
b = 5  # 00 00 01 01
a | b  # 00 00 01 11 : 7
# 按位取反 ~
a = 3  # 00 00 00 11
~a     # 11 11 11 00
# 按位异或 ^ （相同为0，不同为1）
a = 3  # 00 00 00 11
b = 5  # 00 00 01 01
a ^ b  # 00 00 01 10 : 6
# 任何值与自身异或为0
a ^ a  # 00 00 00 00 : 0
# 任何值与0异或不变
a ^ 0  # 00 00 00 11 : 3
# 按位左移 <<


```

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

## Python 风格规范

- 不要在行尾加分号，也不要用分号将两条命令放在同一行。
- 每行不超过80个字符。（导入和注释例外）
- 不要使用反斜杠连接行。（Python 会将括号中的行隐式连接起来）

```python
foo_bar(self, width, height, color='black', design=None, x='foo',
        emphasis=None, highlight=0)
if (width == 0 and height == 0 and
    color == 'red' and emphasis == 'strong'):
```

- 文本字符串在一行放不下，可以使用括号来实现隐式行连接。

```python
s = ('This will build a very long long '
     'long long long long long long string')
```

- 除非是用于实现行连接，否则不要在返回语句或条件语句中使用括号。
- 用4个空格来缩进代码

```python
# Aligned with opening delimiter
foo = long_function_name(var_one, var_two,
                         var_three, var_four)

# Aligned with opening delimiter in a dictionary
foo = {
    long_dictionary_key: value1 +
                         value2,
    ...
}

# 4-space hanging indent; nothing on first line
foo = long_function_name(
    var_one, var_two, var_three,
    var_four)

# 4-space hanging indent in a dictionary
foo = {
    long_dictionary_key:
        long_dictionary_value,
    ...
}
```

- 顶级定义之间空两行（全局类、全局函数、全局变量），方法定义之间空一行（类内函数之间、类与第一个函数之间）。
- 括号内表达式两端不要有空格。
- 在逗号、分号、冒号后面加空格，前面不加。（行尾除外）
- 参数列表、索引或切片的左括号前不加空格。
- 二元操作符两边都加上一个空格。
- '='用于指示参数值或默认值时，不在其两侧使用空格。
- 不要用空格来垂直对齐多行间的标记。（如'#'、'='、':'等）
- 类注释和函数注释规范

```python
class SampleClass(object):
    """Summary of class here.

    Longer class information....
    Longer class information....

    Attributes:
        likes_spam: A boolean indicating if we like SPAM or not.
        eggs: An integer count of the eggs we have laid.
    """

    def __init__(self, likes_spam=False):
        """Inits SampleClass with blah."""
        self.likes_spam = likes_spam
        self.eggs = 0

    def fetch_bigtable_rows(big_table, keys, other_silly_variable=None):
        """Fetches rows from a Bigtable.

        Retrieves rows pertaining to the given keys from the Table instance
        represented by big_table.  Silly things may happen if
        other_silly_variable is not None.

        Args:
            big_table: An open Bigtable Table instance.
            keys: A sequence of strings representing the key of each table row
                to fetch.
            other_silly_variable: Another optional variable, that has a much
                longer name than the other args, and which does nothing.

        Returns:
            A dict mapping keys to the corresponding table row data
            fetched. Each row is represented as a tuple of strings. For
            example:

            {'Serak': ('Rigel VII', 'Preparer'),
            'Zim': ('Irk', 'Invader'),
            'Lrrr': ('Omicron Persei 8', 'Emperor')}

            If a key from the keys argument is missing from the dictionary,
            then that row was not found in the table.

        Raises:
            IOError: An error occurred accessing the bigtable.Table object.
        """
        pass
```

- 行注释应距离代码2个空格。
- 为临时代码使用 TODO 注释。

```python
# TODO(kl@gmail.com): Use a "*" here for string repetition.
# TODO(Zeke) Change this to use relations.
```

- 如果一个类不继承其它类，就显式继承 object。
- 字符串合并规范

```python
x = a + b  # not %
x = '%s, %s!' % (imperative, expletive)
x = '{}, {}!'.format(imperative, expletive)  # not +
x = 'name: %s; score: %d' % (name, n)  # not +
x = 'name: {}; score: {}'.format(name, n)  # not +
```

- 避免在循环中用+和+=操作符来累加字符串。可以将每个子串加入列表，然后在循环结束后用 ''.join() 连接列表。
- 在文件和 sockets 结束时，显式的关闭它。
- 推荐使用 with 打开文件。

```python
with open("hello.txt") as hello_file:
    for line in hello_file:
        print line
```

- 不支持使用 with 语句的类似文件的对象，使用 contextlib.closing()。

```python
import contextlib

with contextlib.closing(urllib.urlopen("http://www.python.org/")) as front_page:
    for line in front_page:
        print line
```

- 每个导入应该独占一行。
- 导入总应该放在文件顶部，位于模块注释和文档字符串之后，模块全局变量和常量之前。
- 导入应该按照标准库、第三方库、应用程序指定导入的顺序分组。每种分组中, 应该根据每个模块的完整包路径按字典序排序, 忽略大小写。
- 通常每个语句应该独占一行。（if 语句只有在没有 else 时才能同处一行）
- 命名规范：module_name, package_name, ClassName, method_name, ExceptionName, function_name, GLOBAL_VAR_NAME, instance_var_name, function_parameter_name, local_var_name。
- 命名约定：
    - 单下划线开头的变量或函数表示 protected（使用 from module import * 时不会包含）。
    - 双下划线开头的变量或函数表示 private。
    - 将相关的类和全局函数放在同一个模块里。
- 主功能应该放在一个 main() 函数中。

```python
def main():
    pass

if __name__ == '__main__':
    main()
```

**参考：**
- [Google Python 风格指南](https://zh-google-styleguide.readthedocs.io/en/latest/google-python-styleguide/contents/)

