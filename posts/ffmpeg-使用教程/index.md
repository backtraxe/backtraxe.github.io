# FFmpeg 使用教程


FFmpeg 是视频处理最常用的开源软件。

<!--more-->

## 常用参数

- `-c`：指定编码器。`-c copy`：直接复制，不重新编码，加快生成速度
- `-c:v`或`-vcodec`：指定视频编码器。`-c:v copy`或`-vcodec copy`：不改变视频编码，直接拷贝
- `-c:a`或`-acodec`：指定音频编码器。`-c:a copy`或`-acodec copy`：不改变音频编码，直接拷贝
- `-i`：指定输入文件
- `-an`：去除音频流
- `-vn`：去除视频流
- `-preset`：指定输出的视频质量，会影响生成速度。可用值：`ultrafast`, `superfast`, `veryfast`, `faster`, `fast`, `medium`, `slow`, `slower`, `veryslow`
- `-y`：不经过确认，输出时直接覆盖同名文件
- `-hwaccel cuvid`：指定使用 cuvid 硬件加速

举例：

```bash
ffmpeg -y -c:a libfdk_aac -c:v libx264 -i input.mp4 -c:a libvorbis -c:v libvpx-vp9 output.webm
```

## 格式转换

```bash
ffmpeg -i input.webm output.mp4
```

## 提取视频中的音频

```bash
ffmpeg -i input.mp4 -vn -acodec copy output.aac
```

## 去除视频中的音频

```bash
ffmpeg -i input.mp4 -an -vcodec copy output.mp4
```

## 合并音频和视频

**视频不包含音频：**

```bash
ffmpeg -i video.mp4 -i audio.aac -c:v copy -c:a copy -strict experimental output.mp4
```

**视频包含音频，需要被替换：**

```bash
ffmpeg -i video.mp4 -i audio.aac -c:v copy -c:a copy -strict experimental -map 0:v:0 -map 1:a:0 output.mp4
```

## 视频截图

在第 4.5s 截取一帧图片

```bash
ffmpeg -i input.mp4 -ss 4.5 -vframes 1 output.png
```

在第 4.5s 截取 10 帧图片

```bash
ffmpeg -i input.mp4 -ss 4.5 -vframes 10 output%d.png
```

