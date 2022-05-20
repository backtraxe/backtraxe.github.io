# Matplotlib 教程


<!--more-->

## 画布

```python
plt.figure(
    figsize=(10, 8),   # 画布大小。（宽，高）
    dpi=None,          # 分辨率。300、600
    tight_layout=None, # 紧凑布局。True
)
```

## 网格

```python
plt.grid(
    visible=None,
    which='major',
    axis='both'
)


```

## 边框

- 获取边框

```python
ax = plt.gca()
ax.spines['top']
ax.spines['right']
ax.spines['bottom']
ax.spines['left']
```

- 边框设置

```python
ax.set_facecolor('white')
ax.spines['top'].set_color('white')
```

## 轴

- x轴

```python
plt.xlabel(
    xlabel,
    loc='center', # 标签位置。'left'、'center'、'right'
)
```

- 刻度

```python
# x轴刻度
plt.xticks(
    ticks=None,    # 刻度。[0, 1, 2]
    labels=None,   # 刻度值。['Jan', 'Feb', 'Mar']
    rotation=None, # 旋转。'vertical'、45
)
# y轴刻度
plt.yticks(
    ticks=None,    # 刻度。[0, 1, 2]
    labels=None,   # 刻度值。['Jan', 'Feb', 'Mar']
    rotation=None, # 旋转。'vertical'、45
)
```

- 设置y轴刻度的最大/最小值

```python
plt.ylim(
    bottom=bottom, # 最小值
    top=top        # 最大值
)
```

## 其他

- 紧凑布局

```python
plt.tight_layout()
```

- 保存图片

```python
plt.savefig(
    fname,             # 文件路径。image.png
    dpi='figure',      # 图片分辨率。600、300
    format=None,       # 文件格式。'png'、'pdf'、'svg'、'eps'
    transparent=False, # 是否透明
)
```

