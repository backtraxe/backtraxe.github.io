# Linux 命令大全


<!--more-->

- `Tab` 补全命令
- `Ctrl + C` 停止当前运行中的程序
-

## scp

scp 是 Linux 系统下基于 ssh 登陆进行安全的远程文件拷贝命令。

```bash
# 1.从本地复制到远程
# 1.1.单文件
scp -P <SSH_PORT> <LOCAL_FILE> <USERNAME>@<IP_ADDRESS>:<REMOTE_FOLDER>
# 1.2.目录
scp -P <SSH_PORT> -r <LOCAL_FOLDER> <USERNAME>@<IP_ADDRESS>:<REMOTE_FOLDER>

# 2.从远程复制到本地
# 1.1.单文件
scp -P <SSH_PORT> <USERNAME>@<IP_ADDRESS>:<REMOTE_FILE> <LOCAL_FOLDER>
# 2.2.目录
scp -P <SSH_PORT> -r <USERNAME>@<IP_ADDRESS>:<REMOTE_FOLDER> <LOCAL_FOLDER>
```

