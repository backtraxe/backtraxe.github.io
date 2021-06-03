# youtube-dl 使用教程


从 YouTube 和其他视频网站下载视频的命令行程序。

<!--more-->

## 一、介绍

- [官网](https://youtube-dl.org/)
- [Github](https://github.com/ytdl-org/youtube-dl/)

## 二、安装

Windows:

1. 下载[exe](https://yt-dl.org/latest/youtube-dl.exe)，然后添加环境变量。
2. `pip install --upgrade youtube-dl`

```bash
# 列出帮助菜单
youtube-dl -h/--help

# 查看版本
youtube-dl --version

# 升级
youtube-dl -U/--update
```

## 三、使用

```bash
# 列出所有清晰度和格式
youtube-dl -F/--list-formats <URL>

# 下载对应格式或清晰度
youtube-dl -f/--format <FORMAT> <URL>

# 下载后视频和音频合并
youtube-dl -f XXX+YYY <URL>

# 下载最佳质量
youtube-dl -f bestvideo+bestaudio <URL>

# 列出所有字幕
youtube-dl --list-subs <URL>

# 下载英文（en）字幕，格式为 srt
youtube-dl --sub-lang en --write-auto-sub --sub-format srt --skip-download <URL>

# 代理
youtube-dl --proxy <URL>

# cookies
youtube-dl --cookies <FILE>

# 登录
youtube-dl -u/--username <USERNAME>
youtube-dl -p/--password <PASSWORD>
youtube-dl -2/--twofactor <TWOFACTOR>
youtube-dl --video-password <PASSWORD>
```

