# Linux 命令大全


常用 Linux 命令。

<!--more-->

## 快捷键

- `Tab` 补全命令
- `Ctrl + C` 停止当前运行中的程序

## cp

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
```

### 排序

```bash
# 按修改时间从近到远排序
ls -t

# 按文件大小从大到小排序
ls -S

# 逆序排序
ls -r
```

### 打印方式

```bash
# 纵向显示（一行一个）
ls -1
# 横向显示（逗号分隔）
ls -m
```

### 文件权限

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

## mv

## scp

在本地主机和远程主机之间传输文件。

### 本地主机的文件上传到远程主机

```bash
# 单个文件
scp <file> <user>@<ip>:<path> -P <ssh_port>
# 目录
scp -r <folder> <user>@<ip>:<path> -P <ssh_port>
```

### 远程主机的文件下载到本地主机

```bash
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
```

### ssh 免密登录

```bash
# 为当前用户生成 ssh 公钥 + 私钥。
# 默认保存在 $home/.ssh/ 文件夹中。
# id_rsa 是私钥，id_rsa.pub 是公钥。
ssh-keygen
# 将当前用户的公钥复制到服务器的 ~/.ssh/authorized_keys 文件
ssh-copy-id <user>@<ip>:<port>
# 或者手动将公钥添加到服务器的 ~/.ssh/authorized_keys 文件
```

