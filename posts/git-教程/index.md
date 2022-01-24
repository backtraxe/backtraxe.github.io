# Git 教程


Git 是一个开源的分布式版本控制系统。

<!--more-->

## Git 基本工作流程

### 本地仓库

- 本地历史仓库（Repository）：存放不同版本的代码
- 暂存区（Index）：代码提交前的临时存储区
- 工作目录（Working Tree）：修改代码的区域

## Git 常用命令

### 本地仓库

```bash
# 初始化，创建 git 仓库
git init
# 添加文件到暂存区
git add <FILENAME>
# 将暂存区文件提交到本地历史仓库
git commit -m "descriptions"
# 将所有修改或删除的文件提交到本地历史仓库（不包括新建文件）
git commit -a -m "descriptions"
```

```bash
# 查看 git 状态
git status
# 查看日志
git log
# 查看简短日志
git reflog
# 查看文件改动记录
git diff <file>
```

### 远端仓库

```bash
# 克隆仓库
git clone https://github.com/USERNAME/REPONAME.git

# 为本地仓库添加远端 GitHub 仓库
git remote add origin https://github.com/USERNAME/REPONAME.git

# 将已提交的版本推送到远端仓库，方便其他设备同步
git push origin <BRANCH_NAME>

# 取回远端仓库版本，对本地仓库进行更新
git pull
```

### 子模块

```bash
# 将一个 Git 仓库添加为当前仓库的子模块
git submodule add https://github.com/USERNAME/REPONAME.git

# git clone 含有子模块的项目
# 1.项目已经克隆到了本地
git submodule init
git sunmodule update
# 或者
git submodule update --init

# 2.项目还未克隆到本地
git clone --recurse-submodules https://github.com/USERNAME/REPONAME.git
# 或者
git clone --recursive https://github.com/USERNAME/REPONAME.git
```

### 替换本地改动

```bash
# 丢弃当前文件修改内容
git checkout -- FILENAME

# 丢弃本地仓库的所有改动与提交版本
git fetch origin
git reset --hard origin/master
```

### 本地创建仓库并上传至 GitHub

1. [配置 Git](#配置-git)。
2. git bash 输入如下指令：

```bash
git init
git add --all  # git add .
git commit -m "descriptions"
git remote add origin https://github.com/USERNAME/REPONAME.git
git push -u origin master
```

## Git 版本切换

```bash
git reset --hard <commit>
```

## Git 分支管理

- 切换：将`HEAD`指向别的分支。
- 合并：将`main`指向该分支，然后将`HEAD`指向`main`分支。

```bash
# 查看所有分支
git branch
# 创建新分支
git branch <branch-name>
# 删除指定分支
git branch -d <branch-name>
# 切换到其他分支
git checkout <branch>
# 创建新分支，并立即切换过去
git checkout -b <branch>
# 合并到指定分支
git merge <branch-name>
```

## 配置 Git

1. 将`your_email@youremail.com`改为你的 Github 账号的邮箱，然后一路回车。

```bash
ssh-keygen -t rsa -C "your_email@youremail.com"
```

2. 成功的话会在`${USERNAME}/`下生成`.ssh`文件夹，进入该文件夹，打开`id_rsa.pub`文件，复制里面的内容。
3. 回到 Github 网页，登录，点击右上角头像，选择`Settings`->`SSH and GPG keys`->`New SSH key`，`Title`随便填，`Key`粘贴你刚复制的内容，然后点击`Add SSH key`。
4. 测试，git bash 输入如下指令：

```bash
ssh -T git@github.com
```

5. 配置用户名和邮箱：

```bash
git config --global user.name "USERNAME"
git config --global user.email "username@email.com"
```

## Q&A

### 无法连接服务器，报错443

问题：

git clone 或 git push 等操作时无法连接至服务器，报错内容如下：

```bash
SSL_connect: SSL_ERROR_SYSCALL in connection to github.com:443
```

答案：

1. 该问题由开启代理软件导致。
2. 到`设置`->`网络和Internet`->`代理`中查看`地址`和`端口`。
3. git bash 中输入如下命令（`IP_ADDRESS`和`PORT`改为刚才查看的`地址`和`端口`）：

```bash
git config --global http.proxy IP_ADDRESS:PORT
# git config --global http.proxy 127.0.0.1:10809
```

## 参考

1. [Git Cheat Sheets](https://training.github.com/)

