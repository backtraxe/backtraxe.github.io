# Markdown for PPT


<!--more-->

## Example

[Marp CLI example](https://yhatt-marp-cli-example.netlify.app/)

```markdown
---
marp: true
theme: gaia
_class: lead
paginate: true
backgroundColor: #fff
backgroundImage: url('https://marp.app/assets/hero-background.jpg')
---

![bg left:40% 80%](https://marp.app/assets/marp.svg)

# **Marp**

Markdown Presentation Ecosystem

https://marp.app/

---

# How to write slides

Split pages by horizontal ruler (`---`). It's very simple! :satisfied:

​```markdown
# Slide 1

foobar

---

# Slide 2

foobar
​```
```

![](Markdown for PPT.assets/marp-for-vs-code.png)

## Introduction

```markdown
---
marp: true
---
```

## Advanced

```markdown
---
marp: true
size: 4:3
theme: default
---
```

### Directive

#### Global Directive

#### Local Directive

```<!--_backgroundColor: orange -->```：改变本张幻灯片的背景颜色。

这里单独说明一下控制是否使用标题级别直接作为分页标志的全局命令 `headingDivider`。如果 Markdown 文档本身层级组织较好，可以将它设置为 `ture` 并且不用再通过分割线为幻灯片分页，在输出幻灯的同时也能保证输出 Markdown 文档时不会因为出现大量的分割线影响效果。

此外，Marp 还保留了一个 `<!-- fit -->` 命令用于标题的自适应，将它放在Markdown 标题的 `#` 后可以使得标题自动填充幻灯片的大小，比较适合于首页大标题等场景。

## Reference

1. [Marp](https://marp.app/)
1. [Marpit](https://marpit.marp.app/)
1. [reveal.js](https://github.com/hakimel/reveal.js)
1. [slides](https://slides.com/)
1. [Slideas for Mac](https://www.slideas.app/)
1. [impress.js demo](https://impress.js.org/)
