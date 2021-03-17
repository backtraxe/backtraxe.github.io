# FreeCodeCamp 2018


freeCodeCamp 2018 年新版课程，包括响应式 Web 设计、算法和数据结构、前端库和框架、数据可视化、API 和微服务、信息安全和质量保证和面试攻略等方面。

<!--more-->

## 响应式 Web 设计

### HTML 基础

`HTML`的全称是`HyperText Markup Language`（超文本标记语言），它是一种用来描述网页结构的标记语言。

#### 文档类型和结构

```html
<!DOCTYPE html> <!--DOCTYPE 一定要大写；html 表示版本为 HTML5，大小写均可-->
<html>
  <head> <!--link、meta、title 和 style 都应该放入 head 标签-->
    <title>title</title>
  </head>
  <body>
    <h1>header</h1>
    <p>text</p>
  </body>
</html>
```

#### 常用标签

```html
<!--标题-->
<h1>一级标题</h1>
<h2>二级标题</h2>
<h3>三级标题</h3>
<h4>四级标题</h4>
<h5>五级标题</h5>
<h6>六级标题</h6>

<!--文本媒体-->
<p>段落</p>
<main>主要内容</main>
<img src="https://test.jpg" alt="图片">
<div></div>
<hr>

<!--特效-->
<strong>加粗</strong>
<em>斜体</em>
<u>下划线</u>
<s>删除线</s>

<!--锚点-->
<a href="https://test.jpg" target="_blank">锚点-1-新标签页网页间跳转</a>
<a href="#contacts-header">锚点-2-网页内跳转</a>
<h2 id="contacts-header">Contacts</h2>
<a href="#">锚点-3-固定链接</a>

<!--列表-->
<ul>
  <li>无序列表1</li>
  <li>无序列表2</li>
</ul>
<ol>
  <li>有序列表1</li>
  <li>有序列表2</li>
</ol>
```

> - `lorem ipsum text`: 占位符

#### 表单

```html
<form action="/url">
  <input type="text" placeholder="占位符" required> <!--必填-->

  <!--label 标签设置 for 属性，让其值与按钮的 id 属性值相等-->
  <!--所有关联的单选按钮应该拥有相同的 name 属性-->
  <label for="A"><input type="radio" id="A" name="A-B">单选按钮-A</label>
  <label for="B"><input type="radio" id="B" name="A-B" checked>单选按钮-B</label> <!--默认选中-->
  <label for="C"><input type="checkbox" id="C" name="C-D">多选按钮-C</label>
  <label for="D"><input type="checkbox" id="D" name="C-D">多选按钮-D</label>

  <button type="submit">提交按钮</button>
</form>
```

### CSS 基础

`CSS`的全称是`Cascading Style Sheet`（层叠样式表），它主要用来控制网页的样式。

> CSS 的选择器区分大小写，因此要谨慎使用大写。

#### 内联样式和内部样式

使用 CSS 的三种方式：

- 内联样式：直接在 HTML 元素里使用`style`属性。
- 内部样式：在`style`标签里编写样式规则。
- 外部样式：创建一个单独的`.css`文件，然后在文件中编写样式规则，最后在文档中引用该文件。（常用）

```html
<!-- 内联样式 -->
<h2 style="color: red;">header</h2>

<!-- 内部样式，style 定义在 head 标签中 -->
<style>
.blue-class {
  color: blue;
}
</style>
<p class="blue-class">text</p>
```

#### 外部样式

```css
h2 {}              /* 标签选择器 */

.class-selector {} /* class 选择器 */

#id-selector {}    /* id 选择器*/

.links {}          /* 超链接选择器 */

a:hover {}         /* 锚点的悬停状态选择器 */

[type="radio"] {}  /* 属性选择器 */
```

#### 调整元素边距

所有的 HTML 元素基本都是以矩形为基础。

每个 HTML 元素周围的矩形空间由三个重要的属性来控制：`padding`（内边距），`margin`（外边距）和`border`（边框）。

![CSS Box Model](/box-model.png)

```css
.red-box {
  background-color: red;
  color: black;

  border-style: solid;
  border-color: black;
  border-width: 5px;
  /* border: 5px solid black; */

  padding: 20px;
  /*
  padding-top: 40px;
  padding-right: 20px;
  padding-bottom: 20px;
  padding-left: 40px;
  padding: 40px 20px 20px 40px; /* 上右下左 */
  */

  margin: 20px; /* 若为负值，元素会变得更大 */
  /*
  margin-top: 40px;
  margin-right: 20px;
  margin-bottom: 20px;
  margin-left: 40px;
  margin: 40px 20px 20px 40px; /* 上右下左 */
  */
}
```

#### 长度单位

- `px` 像素，绝对单位
- `in` 英寸，绝对单位
- `mm` 毫米，绝对单位
- `em` 相对单位，基于元素的字体的 font-size 值
- `rem` 相对单位

#### 样式的继承与优先级

- 若子标签未声明样式规则，则继承父标签的样式。
- 相同级别选择器之间，后声明的优先级高于先声明的
- `class`选择器优先级高于标签选择器
- `id`选择器优先级高于`class`选择器
- 内联样式的优先级高于`id`选择器
- `important`的优先级最高

```css
.pink-text {
  color: pink !important;
}
```

#### CSS 变量

```css
:root {
  --dog-color: brown; /* 定义 CSS 全局变量 */
}

.dog {
  --dog-color: white; /* 定义 CSS 局部变量，覆盖同名全局变量 */
  background-color: var(--dog-color, black); /* 使用 CSS 变量，同时设置一个备用值 */
}
```

### 应用视觉设计

`Applied Visual Design Challenges`（应用视觉设计）结合了排版、色彩理论、图形、动画和页面布局等。

#### 常用属性值

color

可用`英语颜色单词`、`#000000`、`#000`、`rgb(0,0,0)`、`rgba(255,255,255,0.5)` 0 代表完全透明，1 代表完全不透明

- `color: red;` 文本颜色
- `background-color: blue;` 背景颜色

font

- `font-size: 20px;` 文本大小
- `font-weight: bold;` 文本加粗，相当于`<strong></strong>`
- `font-weight: 600;` 文本粗细
- `font-family: sans-serif, monospace, serif;` 文本字体，当前面字体不可用时后面字体作为备用
- `font-style: italic;` 文本斜体，相当于`<em></em>`

border

- `border-color: red;` 边框颜色
- `border-width: 2px;` 边框粗细
- `border-style: solid;` 边框类型
- `border-radius: 10px;` 圆角，50% 就是圆形

text

- `text-align: left;` 文本左对齐
- `text-align: center;` 文本居中
- `text-align: right;` 文本右对齐
- `text-align: justify;` 文本两端对齐

- `text-decoration: underline;` 文本下划线，相当于`<u></u>`
- `text-decoration: line-through;` 文本删除线，相当于`<s></s>`

- `text-transform: lowercase;` 英文字母全小写
- `text-transform: uppercase;` 英文字母全大写
- `text-transform: capitalize;` 英文字母首字母大写
- `text-transform: initial;` 默认值
- `text-transform: inherit;` 继承父元素的 text-transform
- `text-transform: none;` 不改变

- `line-height: 25px;` 行高、行间距

- `box-shadow: 0 10px 20px rgba(0,0,0,0.19), 0 6px 6px rgba(0,0,0,0.23);` 水平偏移量、垂直偏移量、模糊距离、阴影尺寸、颜色

- `opacity: 0.5;` 透明度。1 代表不透明，0.5 代表半透明，0 代表透明

- `z-index: 2;` 指定元素的堆叠次序，取整数，数值大的元素优先显示

#### 位置

在 CSS 里一切 HTML 元素皆为盒子，也就是通常所说的盒模型。块级元素自动从新的一行开始（比如标题、段落以及 div），行内元素排列在上一个元素后（比如图片以及 span）。元素默认按照这种方式布局称为文档的普通流，同时 CSS 提供了`position`属性来覆盖它。

`top`、`bottom`、`left`和`right`定义了元素在相应方位的偏移距离。元素将从当前位置，向属性相反的方向偏移。

##### relative

当元素`position: relative;`时，它允许你通过 CSS 指定该元素在当前普通流页面下的相对偏移量。

> `position: relative;`并不会改变该元素在普通流布局所占的位置，也不会对其它元素的位置产生影响。

##### absolute

当元素`position: absolute;`时，会将元素从当前的文档流里面移除，周围的元素会忽略它。`absolute`定位参照于最近的已定位祖先元素，如果它的父元素没有添加定位规则（默认是`relative`），浏览器会继续寻找直到默认的 body 标签。

##### fixed

当元素`position: fixed;`时，会将元素从当前的文档流里面移除，其它元素会忽略它。

> `fixed`定位和`absolute`定位的最明显的区别是`fixed`定位元素不会随着屏幕滚动而移动。

##### float

通过元素的`float`属性来设置。浮动元素不在文档流中，它向左或向右浮动，直到它的外边缘碰到包含框或另一个浮动框的边框为止。通常需要用`width`属性来指定浮动元素占据的水平空间。可以`float: left;`或`float: right;`。
