# Linux 命令大全


<!--more-->

## 快捷键

- `Tab` 补全命令
- `Ctrl/Command + C` 停止当前运行中的程序

## crontab

设置定时任务。

文件位置：`/var/spool/cron/<用户名>`

```bash
crontab
# -l 查看当前用户的所有定时任务
# -u 查看指定用户的定时任务
# -r 删除当前用户的所有定时任务
# -e 编辑定时任务
```

```bash
# minute 代表一小时内的第几分，范围 0-59。
# hour   代表一天中的第几小时，范围 0-23。
# mday   代表一个月中的第几天，范围 1-31。
# month  代表一年中第几个月，范围 1-12。
# wday   代表星期几，范围 0-7 (0及7都是星期天)。
```

```bash
# 每隔 1 分钟执行一次
* * * * * (<shell1>;<shell2>)

# 每个小时的 3 分整执行一次
3 * * * * (<shell1>;<shell2>)

# 每天凌晨 4 点整执行一次
0 4 * * * (<shell1>;<shell2>)
```

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

## grep

Global search REgular expression and Print out the line

打印指定模式匹配到的所有行，支持多文件、正则表达式。

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

对文件内容进行增删改查，支持正则表达式。

```bash
sed [OPTION]... {script-only-if-no-other-script} [input-file]...
```

默认支持正则表达式语法：`^ $ . * []`

|参数|含义|
|:---:|:---:|
|`--color=auto`|高亮匹配内容|
|`-E`|使用扩展正则表达式，支持：`+ ? \| () {}`|
|`-n`|只输出匹配内容|
|`-i`|输出同时修改文件内容|
|`-e`|多个规则依次匹配|

|内置命令符|含义|
|:---:|:---:|
|`a`|append，指定行后面追加|
|`d`|delete，删除|
|`i`|insert，指定行前面插入|
|`p`|print，打印|
|`/模式/操作`|匹配模式，然后进行操作|
|`s/替换前/替换后/g`|替换，支持正则表达式，`g`表示全局匹配|

- 指定行号

```bash
# 打印第二行和第三行
sed -n "2,3p" <FILE>

# 删除第二行和第三行
sed -i "2,3d" <FILE>

# 打印第二行开始的三行
sed -n "2,+3p" <FILE>

# 删除第二行开始的三行
sed -i "2,+3d" <FILE>

# 打印第二行到最后一行
sed -n "2,$p" <FILE>

# 删除第二行到最后一行
sed -i "2,$d" <FILE>
```

- 指定内容

```bash
# 打印匹配到的行
sed -n "/<PATTERN>/p" <FILE>
# 删除匹配到的行
sed -i "/<PATTERN>/d" <FILE>
```

## split

切分文件。

```bash
# 按文件大小切分文件
# 生成文件名 <split_name>aa, <split_name>ab, <split_name>ac, ...
split -b 100m <file_name> <split_name>

# 按文件行数切分文件
# 生成文件名 <split_name>aa, <split_name>ab, <split_name>ac, ...
split -l 100 <file_name> <split_name>

# 合并文件
cat <split_name>* > <file_name>
```

## kill

进程通信。

```bash
# 杀死进程
kill -9 <PID>
```

## ps

查看进程信息。

```bash
# 显示所有进程的详细信息（包括内存情况和进程状态，无父进程情况）
ps -aux
# -a 显示所有用户创建的进程
# -u 显示进程详细信息
# -x 显示无终端的进程

# 用户 进程id CPU占用率 内存占用率 最大内存占用 实际内存占用 终端 状态 启动时间 已使用CPU时间 命令
# USER PID    %CPU      %MEM       VSZ       RSS     TTY STAT START     TIME   COMMAND
# root   2     0.0       0.0         0         0     ?   S    Aug04     0:01   [kthreadd]

# 进程状态：
#    R  运行、待运行         <  高优先级
#    S  可中断睡眠           N  低优先级
#    D  不可中断睡眠         s  包含子进程
#    T  停止                +  位于前台
#    Z  僵死                l  多线程
```

```bash
# 显示所有进程的详细信息（包括父进程id，无内存情况和进程状态）
ps -ef
# -e
# -f

# 用户id 进程id 父进程id CPU占用率 启动时间  终端 已使用CPU时间  命令
#  UID    PID   PPID      C     STIME   TTY       TIME    CMD
#  root     2      0      0     Aug04   ?     00:00:01    [kthreadd]
```

```bash
# 查找包含 xxx 的进程
ps -aux | grep xxx

# 查找不含 xxx 的进程
ps -aux | grep -v xxx
```

## tail

```bash
# 实时打印
tail -f <file>

# 打印最后 1 行
tail -n 1 <file>

# 从第 10 行开始打印
tail -n +10 <file>
```
