# Markdown 基本语法


<!--more-->

## 1.标题

```markdown
# 一级标题 h1
## 二级标题 h2
### 三级标题 h3
#### 四级标题 h4
##### 五级标题 h5
###### 六级标题 h6
```

## 2.强调

```markdown
**粗体**
*斜体*
~~删除线~~
***斜体加粗***
~~**删除线加粗**~~
~~*斜体删除线*~~
~~***斜体删除线加粗***~~
```

**粗体**、*斜体*、~~删除线~~、***斜体加粗***、~~**删除线加粗**~~、~~*斜体删除线*~~、~~***斜体删除线加粗***~~

## 3.引用

```markdown
> 引用
>> 嵌套引用
```

> 引用
>> 嵌套引用

## 4.分割线

```markdown
---
***
```

---

***

## 5.图片

```markdown
![Backtraxe's Blog](https://backtraxe.github.io/apple-touch-icon.png "Backtraxe's Blog")

<div style="text-align: center">
  <img src="https://backtraxe.github.io/apple-touch-icon.png" alt="Backtraxe's Blog" width="10%" align="center">
</div>
```

![Backtraxe's Blog](https://backtraxe.github.io/apple-touch-icon.png "Backtraxe's Blog")

<div style="text-align: center">
  <img src="https://backtraxe.github.io/apple-touch-icon.png" alt="Backtraxe's Blog" width="10%" align="center">
</div>

## 6.超链接

```markdown
[Backtraxe's Blog](https://backtraxe.github.io/)
<https://backtraxe.github.io/>

这个链接用 1 作为网址变量 [traXe][1]
这个链接用 traxe 作为网址变量 [traXe][traxe]

然后在文档的结尾为变量赋值（网址）
[1]: https://backtraxe.github.io/
[traxe]: https://backtraxe.github.io/
```

[Backtraxe's Blog](https://backtraxe.github.io/)

<https://backtraxe.github.io/>

这个链接用 1 作为网址变量 [traXe][1]

这个链接用 traxe 作为网址变量 [traXe][traxe]

然后在文档的结尾为变量赋值（网址）

[1]: https://backtraxe.github.io/

[traxe]: https://backtraxe.github.io/

## 7.列表

### 7.1 无序列表

```markdown
- 北京
- 上海
- 广州
- 深圳
```

- 北京
- 上海
- 广州
- 深圳

### 7.2 有序列表

```markdown
1. 北京
1. 上海
1. 广州
1. 深圳
或者
1. 北京
2. 上海
3. 广州
4. 深圳
```

1. 北京
1. 上海
1. 广州
1. 深圳

### 7.3 列表嵌套

```markdown
- 北京
- 上海
- 广东
  1. 广州
  2. 深圳
```

- 北京
- 上海
- 广东
  1. 广州
  2. 深圳

## 8.表格

### 8.1 markdown 风格

```markdown
姓名|分数|排名
--|:--:|--:
张三|100|1
李四|85|2
王五|60|3
```

姓名|分数|排名
--|:--:|--:
张三|100|1
李四|85|2
王五|60|3

> - `--`，`:--` : 左对齐
> - `:--:` : 居中
> - `--:` : 右对齐

### 8.2 html 风格

```html
<table>
  <th>
    <td style="text-align: center"><b>姓名</b></td>
    <td style="text-align: center"><b>分数</b></td>
    <td style="text-align: center"><b>排名</b></td>
  </th>
  <tr>
    <td>张三</td>
    <td>100</td>
    <td>1</td>
  </tr>
  <tr>
    <td>李四</td>
    <td>85</td>
    <td>2</td>
  </tr>
  <tr>
    <td>王五</td>
    <td>60</td>
    <td>3</td>
  </tr>
</table>
```

<table>
  <tr>
    <td style="text-align: center"><b>姓名</b></td>
    <td style="text-align: center"><b>分数</b></td>
    <td style="text-align: center"><b>排名</b></td>
  </tr>
  <tr>
    <td>张三</td>
    <td>100</td>
    <td>1</td>
  </tr>
  <tr>
    <td>李四</td>
    <td>85</td>
    <td>2</td>
  </tr>
  <tr>
    <td>王五</td>
    <td>60</td>
    <td>3</td>
  </tr>
</table>

[Tables Generator](https://tablesgenerator.com/)

## 9.代码

### 9.1 单行代码

```markdown
`print("Hello World!")`
```

`print("Hello World!")`

### 9.2 多行代码

```markdown
```cpp
#include<iostream>
int main() {
    std::cout << "Hello World!" << std::endl;
    return 0;
}
``去掉`
```

```cpp
#include<iostream>
int main() {
    std::cout << "Hello World!" << std::endl;
    return 0;
}
```

## 10.公式

### 10.1 行内公式

```markdown
$E=mc^2$
```

$E=mc^2$

### 10.2 多行公式

```markdown
$$
\sum_{i=1}^n a_i=0
$$
```

$$
\sum_{i=1}^n a_i=0
$$

```markdown
$$
\begin{align}
x &= v_0\cos\theta t \newline
y &= v_0\sin\theta t - \frac{1}{2}gt^2
\end{align}
$$
```

$$
\begin{align}
x &= v_0\cos\theta t \newline
y &= v_0\sin\theta t - \frac{1}{2}gt^2
\end{align}
$$

```markdown
$$
\begin{aligned}
x &= v_0\cos\theta t \newline
y &= v_0\sin\theta t - \frac{1}{2}gt^2
\end{aligned}
$$
```

$$
\begin{aligned}
x &= v_0\cos\theta t \newline
y &= v_0\sin\theta t - \frac{1}{2}gt^2
\end{aligned}
$$

