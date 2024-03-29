# 正则表达式教程


用来描述字符串模式匹配规则的表达式。

<!--more-->

## 1.规则

### 1.1 单个字符匹配

| 正则表达式 | 意义 | 等价 |
|:---:|:---:|:---:|
| `a` | 单个指定字母 |  |
| `0` | 单个指定数字 |  |
| `\\` | `\` |  |
| `\*` | `*` |  |
| `\.` | `.` |  |
| `\n` | 换行符 |  |
| `\f` | 换页符 |  |
| `\t` | 制表符 |  |
| `.` | 单个除了换行符之外的任何字符 |  |
| `\d` | 单个数字 | `[0-9]` |
| `\D` | 单个非数字字符 | `[^0-9]` 、 `[^\d]` |
| `\w` | 单个字母或数字或下划线 | `[a-zA-Z0-9_]` |
| `\W` | 单个非字母非数字非下划线字符 | `[^a-zA-Z0-9_]`、`[^\w]` |
| `\s` | 单个空白字符（空格、换行符、换页符、制表符） | `[ \n\f\t]` |
| `\S` | 单个非空白字符 | `[^ \n\f\t]`、`[^\s]` |

### 1.2 匹配次数

| 正则表达式 | 意义 |
|:---:|:---:|
| `?` | 匹配0或1次 |
| `*` | 匹配任意次（大于等于0） |
| `+` | 至少匹配一次（大于等于1） |
| `{a}` | 匹配a次 |
| `{a,b}` | 至少匹配a次，至多匹配b次（[a,b]） |
| `{a,}` | 至少匹配a次（大于等于a） |
| `{0,b}` | 至多匹配b次（[0,b]） |

### 1.3 边界匹配

| 正则表达式 | 意义 |
|:---:|:---:|
| `\b` | 单词边界 |
| `\B` | 非单词边界 |
| `^` | 字符串开头 |
| `$` | 字符串结尾 |
| `/规则/m` | 多行模式 |
| `/规则/i` | 忽略大小写 |
| `/规则/g` | 全局模式 |

### 1.4 逻辑关系

| 正则表达式 | 意义 |
|:---:|:---:|
| `|` | 逻辑或 |
| `[^]` | 逻辑非 |

## 2.高级用法

- 回溯引用
- 前向查找
- 后向查找

## 3.代码中使用

### 3.1 C++

```cpp
#include <regex>

// 转义字符
regex reg1("^[\\w-]+(\\.[\\w-]+)*@[\\w-]+(\\.[\\w-]+)+$");
regex_match("google@gmail.com", reg1); // 1

// 原生字符串 R"(...)"
regex reg2(R"(^[\w-]+(\.[\w-]+)*@[\w-]+(\.[\w-]+)+$)");
regex_match("google@gmail.com", reg2); // 1

//
```

### 3.2 Java


### 3.3 Python


## 4.常用正则表达式

1. 整数：`^-?\d+$`
    1. 自然数（非负整数）：`^\d+$`
    1. 正整数：`^[0-9]*[1-9][0-9]*$`
    1. 非正整数：`^((-\d+) ?(0+))$`
    1. 负整数：`^-[0-9]*[1-9][0-9]*$`
1. 浮点数：`^(-?\d+)(\.\d+)?$`
    1. 非负浮点数：`^\d+(\.\d+)?$`
    1. 正浮点数：`^(([0-9]+\.[0-9]*[1-9][0-9]*) ?([0-9]*[1-9][0-9]*\.[0-9]+) ?([0-9]*[1-9][0-9]*))$`
    1. 非正浮点数：`^((-\d+(\.\d+)?) ?(0+(\.0+)?))$`
    1. 负浮点数：`^(-(([0-9]+\.[0-9]*[1-9][0-9]*) ?([0-9]*[1-9][0-9]*\.[0-9]+) ?([0-9]*[1-9][0-9]*)))$`
1. 下划线、数字和大小写字母：`^\w+$`
    1. 数字和大小写字母：`^[A-Za-z0-9]+$`
        1. 大小写字母：`^[A-Za-z]+$`
            1. 大写字母：`^[A-Z]+$`
            1. 小写字母：`^[a-z]+$`
1. 中文字符：`[\u4e00-\u9fa5]`
1. 双字节字符：`[^\x00-\xff]`，可以用来计算字符串的长度
1. 空行：`\n[\s ? ]*\r`，可以用来删除空白行
1. HTML标记：`/ <(.*)>.* <\/\1> ? <(.*) \/>/`，仅仅能匹配部分，对于复杂的嵌套标记依旧无能为力
1. 首尾空格：`(^\s*) ?(\s*$)` `^\s* ?\s*$`，可以用来删除行首行尾的空白字符
1. 电子邮件：`^[\w-]+(\.[\w-]+)*@[\w-]+(\.[\w-]+)+$` `\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*`
1. 网址（URL）：`(\w+(-\w+)*)(\.(\w+(-\w+)*))*(\?\S*)?$` `[a-zA-z]+://[^\s]*`
1. 命名规则（字母开头，长度5-16，允许字母数字下划线）：`^[a-zA-Z][a-zA-Z0-9_]{4,15}$`
1. 中国座机号码：`\d{3}-\d{8} ?\d{4}-\d{7}`
1. QQ号：`[1-9][0-9]{4,}`
1. 中国邮政编码：`[1-9]\d{5}(?!\d)`
1. 中国身份证：`\d{15} ?\d{18}`
1. IP地址：`\d+\.\d+\.\d+\.\d+`

## 5.辅助网站

- [Regexper](https://regexper.com/)：正则表达式规则可视化。

## 参考资料

1. [正则表达式不要背 - 掘金](https://juejin.cn/post/6844903845227659271)
1. [C++正则表达式 - cpluspluser - 博客园](https://www.cnblogs.com/coolcpp/p/cpp-regex.html)

