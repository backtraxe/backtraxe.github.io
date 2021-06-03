# You-Get 使用教程


You-Get 是一个轻量级命令行工具，可以方便的从一些网站上下载媒体内容（视频、音频、图像）。

<!--more-->

## 一、介绍

- [官网](https://you-get.org/)
- [Github](https://github.com/soimort/you-get)

## 二、安装

`pip install --upgrade you-get`

## 三、使用

```bash
# 查看所有可选质量与格式
you-get -i/--info <URL>

# 自定义下载文件路径和名称
you-get -o/--output-dir <PATH> <URL>
you-get -O/--output-filename <FILENAME> <URL>

# 代理
you-get -x/--http-proxy <PROXY_IP:PORT> <URL>

# cookies
you-get --cookies/-c <cookies.txt/cookies.sqlite> <URL>
```

### 3.1 cookie 获取

Chrome 扩展程序：[Get cookies.txt](https://chrome.google.com/webstore/detail/get-cookiestxt/bgaddhkoddajcdgocldbbfleckgcbcid)

## 四、支持网站

- [YouTube](https://www.youtube.com/)
- [bilibili](https://www.bilibili.com/)

