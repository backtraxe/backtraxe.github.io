# 环境配置


<!--more-->

## Windows 添加环境变量

### 窗口化操作

1. 按`Win + R`，并输入`sysdm.cpl`。

<div align="center"><img src="/imgs/环境配置/Windows添加环境变量1.png"></div>

2. 选择`高级`，然后点击`环境变量`。

<div align="center"><img src="/imgs/环境配置/Windows添加环境变量2.png"></div>

3. 在系统变量区中选择`Path`，点击`编辑`。

<div align="center"><img src="/imgs/环境配置/Windows添加环境变量3.png"></div>

4. 点击`新建`，输入程序的执行路径（绝对路径）。

<div align="center"><img src="/imgs/环境配置/Windows添加环境变量4.png"></div>

### 误删系统环境变量的补救措施

1. **若未关闭当前CMD**。输入`echo %PATH%`会显示原来的 PATH 值。

1. **若已关闭当前CMD**。每个正在运行的 Windows 程序都会有自己已加载的 PATH，可以使用[Process Explorer](https://docs.microsoft.com/zh-cn/sysinternals/downloads/process-explorer)来查看当前正在运行的程序的环境变量。例如：如果你之前打开了 Chrome，且一直未关闭，按`Ctrl+O`打开`C:\Windows\System32\cmd.exe`，然后输入`echo %PATH%`会显示原来的 PATH 值。恢复之后删除`C:\Program Files\Google\Chrome\Application`和用户变量中的PATH值即可。

1. **若已重启电脑**。手动恢复 PATH 到默认值`%SystemRoot%\system32;%SystemRoot%;%SystemRoot%\System32\Wbem;%SYSTEMROOT%\System32\WindowsPowerShell\v1.0`，其他值已丢失。

参考：[如何恢复我删除的Path环境变量？](https://qastack.cn/superuser/523688/deleted-path-environment-variable-how-to-restore)

## VS Code

[Visual Studio Code 官网](https://code.visualstudio.com/)

### 1. C++

1. 安装编译器
    - Windows（二选一）
        - 安装 [Visual Studio](https://visualstudio.microsoft.com/zh-hans/)
        - 安装 [MinGW-w64](https://www.mingw-w64.org/downloads/)，推荐从 [SourceForge](https://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win64/Personal%20Builds/mingw-builds/8.1.0/threads-posix/seh/) 下载安装。
    - Linux: `sudo apt install gcc g++ gdb build-essential`
    - Mac OS: `xcode-select --install`
2. 安装 VS Code C/C++ 扩展`ms-vscode.cpptools`。
3. asd
4. 解决头文件找不到
    - 修改`c_cpp_properties.json`
    - Linux：`gcc -v -E -x c++ -`

{{< admonition quote "Windows 10 + Visual Studio 2019" false >}}
```json
{
    "configurations": [
        {
            "name": "Win32",
            "includePath": [
                "${workspaceFolder}/**",
                "C:/Program Files (x86)/Microsoft Visual Studio/2019/Community/VC/Tools/MSVC/14.29.30037/include/**"
            ],
            "defines": [
                "_DEBUG",
                "UNICODE",
                "_UNICODE"
            ],
            "compilerPath": "C:/Program Files (x86)/Microsoft Visual Studio/2019/Community/VC/Tools/MSVC/14.29.30037/bin/Hostx64/x64/cl.exe",
            "cStandard": "c17",
            "cppStandard": "c++17",
            "intelliSenseMode": "windows-msvc-x64"
        }
    ],
    "version": 4
}
```
{{< /admonition >}}

{{< admonition quote "Windows 10 + Mingw-w64" false >}}
```json
{
    "configurations": [
        {
            "name": "Win32",
            "includePath": [
                "${workspaceFolder}/**",
                "C:/Portable/mingw64/lib/gcc/x86_64-w64-mingw32/8.1.0/include/**",
                "C:/Portable/mingw64/lib/gcc/x86_64-w64-mingw32/8.1.0/include/c++/**"
            ],
            "defines": [
                "_DEBUG",
                "UNICODE",
                "_UNICODE"
            ],
            "compilerPath": "C:/Portable/mingw64/bin/g++.exe",
            "cStandard": "c17",
            "cppStandard": "c++17",
            "intelliSenseMode": "gcc-x64"
        }
    ],
    "version": 4
}
```
{{< /admonition >}}

5. 打开设置。
    - `C_Cpp: Clang_format_fallback Style`设为`{BasedOnStyle: LLVM, UseTab: Never, IndentWidth: 4, TabWidth: 4}`
    - `C_Cpp › Default: Cpp Standard`设为``
    - `C_Cpp › Default: C Standard`设为``

[参考](https://code.visualstudio.com/docs/cpp/config-msvc)

**关闭 Windows Defender “首次看到时阻止”：**

问题描述：

编译运行程序的时候总是弹出一个 Microsoft Defender 防病毒程序窗口，提示“需要扫描当前程序”。

解决方法：

1. 按`Win+R`，输入`gpedit.msc`，打开本地组策略编辑器。
2. 左侧选择`计算机配置`->`管理模板`->`Windows 组件`-`Microsoft Defender 防病毒`->`MAPS`。
3. 右侧双击`配置“首次看到时阻止”功能`，选择`已禁用`，然后点击`确定`，保存退出。

## Python

1. [Python官网](https://www.python.org/downloads/)
1. [Miniconda官网](https://docs.conda.io/en/latest/miniconda.html)
1. [Anaconda官网](https://www.anaconda.com/products/individual#Downloads)

### pip 设置代理

- Windows：`%APPDATA%\pip\pip.ini`

```ini
[global]
proxy = http://user:password@proxy_name:port
```

### Conda 常用命令

更新：`conda update`

查看环境：`conda env list`或`conda info -e`

创建环境：`conda create -n <ENVNAME> python=3.X -y`

删除环境：`conda remove -n <ENVNAME> --all -y`

设置代理：`conda config --set proxy_servers.http http://127.0.0.1:10809`

## PyTorch

[PyTorch 官网](https://pytorch.org/)

**CPU：**

Anaconda:

`conda install pytorch torchvision torchaudio cpuonly -c pytorch`

Pip:

`pip install torch==1.8.1+cpu torchvision==0.9.1+cpu torchaudio===0.8.1 -f https://download.pytorch.org/whl/torch_stable.html`

**GPU (CUDA 11.0)：**

Anaconda:

`conda install pytorch torchvision torchaudio cudatoolkit=11.0 -c pytorch`

Pip:

`pip install torch==1.7.1+cu110 torchvision==0.8.2+cu110 torchaudio===0.7.2 -f https://download.pytorch.org/whl/torch_stable.html`

检查是否安装成功：

```python
import torch
# 检查 pytorch 是否安装成功
print(torch.__version__)
# 检查 CUDA 是否可用
print(torch.cuda.is_available())
```

## TensorFlow

[TensorFlow 官网](https://www.tensorflow.org/)

Pip:

CPU and GPU: `pip install tensorflow`

Wheel:

```bash
# https://storage.googleapis.com/tensorflow/windows/cpu/tensorflow_cpu-2.4.0-cp38-cp38-win_amd64.whl
pip install tensorflow_cpu-2.4.1-cp38-cp38-win_amd64.whl  # CPU
# https://storage.googleapis.com/tensorflow/windows/gpu/tensorflow_gpu-2.4.0-cp38-cp38-win_amd64.whl
pip install tensorflow_gpu-2.4.0-cp38-cp38-win_amd64.whl  # GPU
```

检查是否安装成功：

```python
import tensorflow as tf
# 检查 tensorflow 是否安装成功
print(tf.__version__)
# 检查 CUDA 是否可用
# 输出最后一行显示 [PhysicalDevice(name='/physical_device:GPU:0', device_type='GPU')]
print(tf.config.experimental.list_physical_devices('GPU'))
```

### Q&A

**问题描述：**

安装 CUDA 11.1 + cuDNN 8.1，tensorflow 2.4.1 检查 GPU 时报错：

`Could not load dynamic library 'cusolver64_10.dll'; dlerror: cusolver64_10.dll not found`

**解决方案：**

卸载重新安装 CUDA 11.0 + cuDNN 8.0，参考 [Windows 经过测试的构建配置](https://www.tensorflow.org/install/source_windows#gpu)。

## CUDA & cuDNN

> - CUDA 和 cuDNN 版本：[Windows 经过测试的构建配置](https://www.tensorflow.org/install/source_windows#gpu)
> - 以笔记本 RTX2060 显卡为例

1. [NVIDIA 驱动程序下载](https://www.nvidia.cn/Download/index.aspx?lang=cn)，选择对应版本，下载安装。

<img src="/imgs/NVIDIA驱动程序下载.png" alt="NVIDIA 驱动程序下载">

2. [CUDA 工具包下载](https://developer.nvidia.com/cuda-toolkit-archive)，下载对应版本安装。

<img src="/imgs/CUDA工具包下载.png" alt="CUDA 工具包下载">

在`CMD`输入`nvcc -V`，出现如下输出表示安装成功。

```bash
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2020 NVIDIA Corporation
Built on Tue_Sep_15_19:12:04_Pacific_Daylight_Time_2020
Cuda compilation tools, release 11.1, V11.1.74
Build cuda_11.1.relgpu_drvr455TC455_06.29069683_0
```

3. [cuDNN 下载](https://developer.nvidia.com/rdp/cudnn-download)，需要登陆账号，登陆后下载对应版本，解压将`bin`、`include`和`lib`三个文件夹的内容复制到 CUDA 安装目录下。

