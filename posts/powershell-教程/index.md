# PowerShell 教程


PowerShell

<!--more-->

> 所有指令或参数均不区分大小写！

## 帮助

```powershell
help | Get-Help
   [[-Name] <string>]  # 本地查询某命令的用法
   [-Online]           # 微软官方文档查询某命令的用法
```

## 查看目录

```powershell
ls | dir | gci | Get-ChildItem
   [[-Path] <string[]>]    #
   [[-Filter] <string>]
   [-Include <string[]>]
   [-Exclude <string[]>]
   [-Recurse]              # 递归列出文件夹中内容
   [-Depth <uint32>]       # 限制递归层数
   [-Force]
   [-Name]
   [-Attributes <FlagsExpression[FileAttributes]>]
   [-FollowSymlink]
   [-Directory]
   [-File]
   [-Hidden]
   [-ReadOnly]
   [-System]
   [<CommonParameters>]
```

文件属性：[`d`]^(directory), `a`(archive), `r`(read-only), `h`(hidden), `l`(link),
