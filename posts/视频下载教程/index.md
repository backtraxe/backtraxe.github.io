# 视频下载教程


<!--more-->

## 前置

1. 安装 [FFmpeg](https://ffmpeg.org/)。

```bash
choco install ffmpeg-full
```

## Lux

[Github](https://github.com/iawia002/lux)，支持 Bilibili、YouTube 等。

1. 安装。

```bash
go install github.com/iawia002/lux@latest
```

2. 查看视频的所有分辨率，记住对应分辨率的`id`。

```bash
lux -i <url>
```

3. 根据`id`下载指定分辨率的视频。

```bash
lux -f <id> <url>
```

4. 指定 cookie 视频的所有分辨率。

```bash
lux -c "<cookie>" -i <url>
```

5. 指定 cookie 下载指定分辨率的视频。

```bash
lux -c "<cookie>" -f <id> <url>
```

## youtube-dl

[Github](https://github.com/ytdl-org/youtube-dl)，支持 Bilibili、YouTube 等。

1. 安装。

```bash
pip install -U youtube-dl
# mac only
brew install youtube-dl
```

2. 查看。

```bash
# 列出所有清晰度和格式
youtube-dl -F <url>

# 列出所有字幕
youtube-dl --list-subs <url>
```

3. 下载。

```bash
# 下载最佳质量
youtube-dl -f bestvideo+bestaudio <url>

# cookies
youtube-dl --cookies <cookies.txt> -f bestvideo+bestaudio <url>

# 下载英文（en）字幕，格式为 srt
youtube-dl --sub-lang en --write-auto-sub --sub-format srt --skip-download <url>
```

## you-get

[Github](https://github.com/soimort/you-get)，支持 Bilibili、YouTube 等。

1. 安装。

```
pip install -U you-get
```

2. 查看。

```bash
# 查看所有可选质量与格式
you-get -i/--info <URL>
```

3. 下载。

```bash
# 自定义下载文件路径和名称
you-get -o <path> <url>
you-get -O <filename> <url>

# cookies
you-get -c <cookies.txt> <url>
```

