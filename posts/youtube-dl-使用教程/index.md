# Youtube-dl 使用教程


<!--more-->

```bash
# 列出所有清晰度和格式
youtube-dl -F <URL>

# 下载对应格式或清晰度
youtube-dl -f XXX <URL>

# 下载后视频和音频合并
youtube-dl -f XXX+YYY <URL>

# 列出所有字幕
youtube-dl --list-subs <URL>

# 下载英文（en）字幕，格式为 srt
youtube-dl --sub-lang en --write-auto-sub --sub-format srt --skip-download <URL>
```

