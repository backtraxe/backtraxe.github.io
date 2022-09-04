# Linux 命令大全


<!--more-->

## 快捷键

- `Tab` 补全命令
- `Ctrl + C` 停止当前运行中的程序

## grep

Global search REgular expression and Print out the line

打印指定模式匹配到的所有行，支持正则表达式。

```bash
grep [OPTION]... PATTERNS [FILE]...
```

默认支持正则表达式语法：`^ $ . * []`

|参数|含义|
|:---:|:---:|
|`--color=auto`|高亮匹配内容|
|`-E`|使用扩展正则表达式，支持：`+ ? \| () {}`|
|`-i`|忽略大小写|
|`-w`|只匹配整个单词|
|`-x`|只匹配整行|
|`-v`|打印未匹配的所有行|
|`-m NUM`|指定打印行数|
|`-H`|打印文件名|
|`-n`|打印行号|
|`-c`|打印行数|
|`-o`|只打印匹配到的内容，而不是整行|
|`-r`|递归匹配|
|`-l`|打印有匹配行的文件名|
|`-L`|打印无匹配行的文件名|

[正则表达式教程](../正则表达式教程/)

```bash
# 匹配空行
grep "^$" <FILE>

# 匹配非空行
grep -v "^$" <FILE>

# 统计匹配行数
grep -c <PATTERN> <FILE>

# 统计匹配次数
grep -o <PATTERN> <FILE> | wc -l

# 打印注释行
grep "^#.*" <FILE>
```

## sed

Stream EDitor



```bash
sed [OPTION]... {script-only-if-no-other-script} [input-file]...
```



## awk

## ls

显示目录内容。

```bash
# 指定目录，默认当前目录
ls <path>

# 列出详细信息
# 权限、用户、用户组、大小、修改时间、名称
ls -l
# 文件大小显示更直观
ls -lh

# 列出所有文件（包括隐藏）
ls -a

# 按修改时间从近到远排序
ls -t

# 按文件大小从大到小排序
ls -S

# 逆序排序
ls -r

# 纵向显示（一行一个）
ls -1
# 横向显示（逗号分隔）
ls -m
```

`drwxrwxrwx`

- 第1位，`d`，文件类型。
    - `d`：目录。
    - `-`：一般文件。
- 第2-4位，`rwx`，文件创建者的权限。
- 第5-7位，`rwx`，文件创建者同用户组成员的权限。
- 第8-10位，`rwx`，其他成员的权限。

`rwx`

- `r`，读取权限。二进制为`100`，十进制即`4`。
- `w`，写入权限。二进制为`010`，十进制即`2`。
- `x`，执行权限。二进制为`001`，十进制即`1`。

## scp

在本地主机和远程主机之间传输文件。

```bash
# 本地主机的文件上传到远程主机
# 单个文件
scp <file> <user>@<ip>:<path> -P <ssh_port>
# 目录
scp -r <folder> <user>@<ip>:<path> -P <ssh_port>

# 远程主机的文件下载到本地主机
# 单个文件
scp <user>@<ip>:<file> <path> -P <ssh_port>
# 目录
scp -r <user>@<ip>:<folder> <path> -P <ssh_port>
```

## ssh

远程登录。

```bash
# 默认端口 22
ssh <user>@<ip>
# 指定端口
ssh <user>@<ip> -p <port>

# ssh 免密登录
# 为当前用户生成 ssh 公钥 + 私钥。
# 默认保存在 $home/.ssh/ 文件夹中。
# id_rsa 是私钥，id_rsa.pub 是公钥。
ssh-keygen
# 将当前用户的公钥复制到服务器的 ~/.ssh/authorized_keys 文件
ssh-copy-id <user>@<ip>:<port>
# 或者手动将公钥添加到服务器的 ~/.ssh/authorized_keys 文件
```

## 参考


