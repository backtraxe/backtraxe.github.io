# You-Get 使用教程


You-Get 是一个轻量级命令行工具，可以方便的从一些网站上下载媒体内容（视频、音频、图像）。

<!--more-->

## 安装

`pip install you-get`

## 教程

### 查看所有可选质量与格式

命令：`you-get -i/--info [URL]`

示例：`you-get -i https://www.bilibili.com/video/BV1es41197aW`

### 自定义下载文件路径和名称

命令：

- `you-get -o/--output-dir [PATH] [URL]`
- `you-get -O/--output-filename [FILENAME] [URL]`

示例：`you-get -o "钢笔换本子" -O "钢笔换本子" https://www.bilibili.com/video/BV1es41197aW`

### 设置代理

命令：`you-get -x/--http-proxy [PROXY_IP:PORT] [URL]`

示例：`you-get -x 127.0.0.1:10809 https://www.youtube.com/watch?v=mixodgV2ERg`

### 导入 cookies

命令：`you-get --cookies/-c [cookies.txt/cookies.sqlite] [URL]`

示例：

## 支持网站


