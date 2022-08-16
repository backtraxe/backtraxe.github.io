# RSS Miniflux 搭建


<!--more-->

## 准备

### 安装 Docker Engine

```bash
# 卸载旧版本
sudo apt-get remove docker docker-engine docker.io containerd runc
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg lsb-release
# 添加 Docker 源
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
# 安装 Docker
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
# 测试是否安装成功
sudo docker run hello-world
docker --version
```

### 安装 docker-compose

```bash
# 将 2.9.0 替换为最新版本
curl -L https://github.com/docker/compose/releases/download/v2.9.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose && chmod +x /usr/local/bin/docker-compose
# 测试
docker-compose --version
```

## RSS 搭建

### Miniflux

1. 准备配置文件。

```bash
vim docker-compose.yml
```

2. 填入以下内容。
    - 将第 6 行的`80`改为自定义端口号。
    - 将第 13 行的`admin`改为自定义用户名。
    - 将第 14 行的`test123`改为自定义密码。

```yaml
version: '3.4'
services:
  miniflux:
    image: miniflux/miniflux:latest
    ports:
      - "80:8080"
    depends_on:
      - db
    environment:
      - DATABASE_URL=postgres://miniflux:secret@db/miniflux?sslmode=disable
      - RUN_MIGRATIONS=1
      - CREATE_ADMIN=1
      - ADMIN_USERNAME=admin
      - ADMIN_PASSWORD=test123
  db:
    image: postgres:latest
    environment:
      - POSTGRES_USER=miniflux
      - POSTGRES_PASSWORD=secret
    volumes:
      - miniflux-db:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD", "pg_isready", "-U", "miniflux"]
      interval: 10s
      start_period: 30s
volumes:
  miniflux-db:
```

3. 启动容器。

```bash
# 启动
docker-compose up -d
# 查看容器状态
docker ps -a
# 当 miniflux 对应的 STATUS 为以下状态时，即正在运行
# CONTAINER ID   IMAGE                      COMMAND                  CREATED       STATUS                 PORTS      NAMES
# 01393843e9e8   miniflux/miniflux:latest   "/usr/bin/miniflux"      2 hours ago   Up 2 hours                        root-miniflux-1
# e40d1929924a   postgres:latest            "docker-entrypoint.s…"   2 hours ago   Up 2 hours (healthy)   5432/tcp   root-db-1
# 否则可以手动启动容器
docker start 01393843e9e8
```

4. 配置。

    - 访问`http://ip:port`，输入刚才自定义的用户名和密码。
    - 在设置界面设置启用 fever，并设置用户名和密码。

## RSSHub 搭建

### docker-compose 搭建

```bash
# 下载 docker-compose.yml
wget https://raw.githubusercontent.com/DIYgod/RSSHub/master/docker-compose.yml
# 创建 volume 持久化 Redis 缓存
docker volume create redis-data
# 启动
docker-compose up -d
```

### Vercel 搭建

[部署](https://vercel.com/import/project?template=https://github.com/DIYgod/RSSHub)

## 客户端

Fever API:
- url: `http://ip:port/fever/`
- username: `自定义 fever 的用户名`
- password: `自定义 fever 的密码`

## 参考

1. [Install Docker Engine on Debian | Docker Documentation](https://docs.docker.com/engine/install/debian/)
2. [Docker Installation | Miniflux Installation Instructions](https://miniflux.app/docs/installation.html#docker)
3. [用Miniflux自建轻便好用的RSS服务](https://zoomyale.com/2018/miniflux_rss/)
4. [Docker Compose 部署 | RSSHub](https://docs.rsshub.app/install/#docker-compose-bu-shu)

