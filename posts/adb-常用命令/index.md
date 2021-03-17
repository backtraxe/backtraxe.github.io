# ADB 常用命令


Android Debug Bridge (ADB) 是一种功能多样的命令行工具，可让您与安卓设备进行通信。ADB 命令可用于执行各种设备操作（例如安装和调试应用），并提供对 Unix shell（可用来在设备上运行各种命令）的访问权限。

<!--more-->

## 准备

1. 手机打开`开发者模式`。
2. 进入`开发者选项`，打开`USB调试`。
3. 手机 USB 连接电脑。
4. 电脑下载 [Android SDK Platform Tools](https://developer.android.com/studio/releases/platform-tools.html)，解压后进入文件夹。
5. 运行 cmd，输入以下内容。

## 命令

提取已安装 APP 的 APK 安装包

```bash
# 1.列出所有已安装APP的包名
adb shell pm list packages

# 2.获取所需APP的APK文件的完整路径
adb shell pm path XXX.XXX.XXX

# 3.根据上一步的输出，提取安装包到电脑当前目录
adb pull /data/app/XXX.XXX.XXX.apk .
```

