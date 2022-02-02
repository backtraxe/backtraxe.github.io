# Git 教程


Git 是一个开源的分布式版本控制系统。

<!--more-->

## 1 Git 基本工作流程

### 1.1 本地仓库

- 本地历史仓库（Repository）：存放不同版本的代码。
- 暂存区（Index）：代码提交前的临时存储区。
- 工作目录（Working Tree）：修改代码的区域。

### 1.2 远程仓库

- 克隆（Clone）：将远程仓库中的内容复制到本地仓库。
- 推送（Push）：将本地仓库中的内容推送到远程仓库。
- 拉取（Pull）：更新远程仓库中的改动到本地仓库。

## 2 Git 常用命令

```bash
# 查看 git 状态
git status
# 查看日志
git log
# 查看简短日志
git reflog
```

### 2.1 本地仓库

```bash
# 初始化，创建 git 仓库
git init
# 添加文件到暂存区
git add <file>
# 将暂存区文件提交到本地历史仓库
git commit -m <message>
# 将所有修改或删除的文件提交到本地历史仓库（不包括新建文件）
git commit -a -m <message>
```

### 2.2 版本切换

```bash
# git reset --hard a3a9cf1
git reset --hard <commit>
```

### 2.3 分支管理

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
# 将指定分支合并到当前分支
git merge <branch-name>
```

### 2.4 远程仓库

#### 2.4.1 远程仓库创建

1. [Github](https://github.com/)：全球最大。
2. [Gitlab](https://about.gitlab.com/)：国外网站。
3. [Gitee](https://gitee.com/)：国内最大。

#### 2.4.2 SSH 配置

1. 配置用户名和邮箱，然后生成密钥（公钥和私钥）。

```bash
# 配置用户名和邮箱
git config --global user.name "backtraxe"
git config --global user.email "backtraxe@gmail.com"
# 生成密钥
ssh-keygen -t rsa -C "backtraxe@gmail.com"
```

2. 进入`$HOME/.ssh`文件夹，复制公钥`id_rsa.pub`文件中的内容。

3. 回到网页进行配置，点击右上角头像。

    - **Github**：`Settings`->`SSH and GPG keys`->`New SSH key`。`Title`随便填，`Key`粘贴公钥内容。
    - **Gitee**：`设置`->`SSH公钥`。`标题`随便填，`公钥`粘贴公钥内容。

4. 输入如下指令测试是否配置成功。

```bash
# Github
ssh -T git@github.com
# Gitlab
ssh -T git@gitlab.com
# Gitee
ssh -T git@gitee.com
```

#### 2.4.3 本地仓库同步到远程仓库

1. 添加或修改文件。
2. 添加到暂存区。
3. 提交到本地仓库。
4. 推送到远程仓库。

```bash
# 添加远程仓库，起一个别名
# git remote add origin https://github.com/backtraxe/backtraxe.github.io.git
git remote add <name> <url>

# 推送到远程仓库，更新远程仓库
# git push -u origin master
git push -u <repository> <refspec>
```

#### 2.4.4 远程仓库同步到本地仓库

```bash
# 克隆远程仓库
# git clone https://github.com/backtraxe/backtraxe.github.io.git
git clone <repo>

# 拉取远端仓库，更新本地仓库
# git pull origin master
git pull <repository> <refspec>
```

### 2.5 代码冲突

同一文件存在多个新版本，如下所示。需要先拉取远程仓库，手动修改冲突文件后，再次推送即可。

```text
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'https://github.com/backtraxe/repo_for_test.git'
```

#### 2.5.1 替换本地改动

```bash
# 丢弃当前文件修改内容
git checkout -- <file>

# 丢弃本地仓库的所有改动与提交版本
git fetch origin
git reset --hard origin/master
```

### 2.6 子模块

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

### 2.7 .gitignore

工作目录中需要 git 忽略的文件目录。

## Q&A

### 1. 无法连接服务器，报错443

问：

`git clone`或`git push`等操作时无法连接至服务器，报错内容如下：

```text
SSL_connect: SSL_ERROR_SYSCALL in connection to github.com:443
```

答：

该问题由开启代理软件导致。`设置`->`网络和Internet`->`代理`，查看`地址`和`端口`，通过如下命令进行配置。

```bash
# git config --global http.proxy 127.0.0.1:10809
git config --global http.proxy IP_ADDRESS:PORT
```

## 参考

1. [Git Cheat Sheets](https://training.github.com/)

