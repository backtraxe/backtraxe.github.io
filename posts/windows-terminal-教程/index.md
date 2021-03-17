# Windows Terminal 教程


Windows 终端是一个面向命令行工具和 shell（如命令提示符、PowerShell 和适用于 Linux 的 Windows 子系统 (WSL)）用户的新式终端应用程序。 它的主要功能包括多个选项卡、窗格、Unicode 和 UTF-8 字符支持、GPU 加速文本呈现引擎，你还可用它来创建你自己的主题并自定义文本、颜色、背景和快捷方式。

<!--more-->

## 一、安装

推荐从 [Microsoft Store](https://aka.ms/terminal) 安装，也可从 [官方Github](https://github.com/microsoft/terminal) 下载安装。

## 二、自定义配色方案

打开 `Windows Terminal`，点击当前标签页顶端右侧向下的箭头，出现下拉菜单，点击设置。

`Ctrl + Alt + ,` (逗号)

## 三、自定义背景

## 四、命令行参数

使用三个窗格从 PowerShell 打开 Windows 终端（左窗格运行命令提示符配置文件，右窗格拆分为两个，上面用于 PowerShell，下面用于命令提示符）：

```powershell
wt -p cmd `; split-pane -p powershell `; split-pane -H cmd
```

## 参考文章

1. [新生代 Windows 终端：Windows Terminal 的全面自定义 - 少数派](https://sspai.com/post/59380/)
1. [Windows 终端 - Microsoft](https://docs.microsoft.com/zh-cn/windows/terminal/)
