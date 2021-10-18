# 阿里云盘


阿里云盘相关小技巧。

<!--more-->

## 1）阿里云盘转存115资源

1. 下载 Chrome 或 Edge 或 Firefox 浏览器，并安装 Tampermonkey 插件。
2. 安装[脚本](https://bbs.tampermonkey.net.cn/thread-427-1-1.html)。
3. 打开[阿里云盘网页版](https://www.aliyundrive.com/drive)。
4. 点击右上角`多文件提取`，导入 sha1 链接文件。

### 参考

- [如何使用第三方脚本实现阿里云盘分享文件 - 太空堡垒麦克罗斯](https://saylinrick.icu/index.php/2021/04/12/%e9%98%bf%e9%87%8c%e4%ba%91%e5%88%86%e4%ba%ab/)

## 2）绕过阿里云分享限制

目前阿里云盘只能分享 txt、mp4、jpg 等特定类型的文件, 而不能分享 zip、rar、7z 等压缩文件。

1. 下载十六进制编辑器，修改前四个数值就可以改变文件种类。
    - macOS：`brew install hex-fiend`
2. 示例：将`zip`文件伪装成`png`文件。
    1. 打开文件，将前四个数值改为`89504E47`。
    2. 修改文件后缀为`png`。
    3. 上传阿里云盘，此时可以分享。
    4. 下载文件后打开，将前四个数值改回`504B0304`。
    5. 文件后缀改回`zip`，然后解压。

|文件类型|前四个值|
|:---:|:---:|
|`png`|`89504E47`|
|`jpg/jpeg`|`FFD8FFE0`|
|`gif`|`47494638`|
|`webp`|`52494646`|
|`svg`|`3C737667`|
|`mp4`|`00000020`|
|`zip`|`504B0304`|
|`7z`|`377ABCAF`|

### 参考

- [如何绕过阿里云盘文件分享种类限制 - 哔哩哔哩](https://www.bilibili.com/read/cv12436193)

