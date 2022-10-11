# RSSHub Miniflux 搭建


<!--more-->

## 1.安装 Docker

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

## 2.部署 Miniflux 和 RSSHub

1. 准备配置文件。

```bash
vim docker-compose.yml
```

2. 填入以下内容。

```yaml
version: '4.0'

services:

  miniflux:
    image: miniflux/miniflux:latest
    container_name: miniflux
    restart: unless-stopped
    ports:
      - "8888:8080" # 8888 为自定义端口
    depends_on:
      - db
      - rsshub
    environment:
      - DATABASE_URL=postgres://miniflux:secret@db/miniflux?sslmode=disable
      - RUN_MIGRATIONS=1
      - CREATE_ADMIN=1
      - ADMIN_USERNAME=admin   # 自定义用户名
      - ADMIN_PASSWORD=test123 # 自定义密码

  db:
    image: postgres:latest
    container_name: postgres
    restart: unless-stopped
    environment:
      - POSTGRES_USER=miniflux   # 可修改，需和 DATABASE_URL 中对应
      - POSTGRES_PASSWORD=secret # 可修改，需和 DATABASE_URL 中对应
    volumes:
      - miniflux-db:/var/lib/postgresql/data
    healthcheck:
      test: [ "CMD", "/usr/bin/miniflux", "-healthcheck", "auto" ]
      interval: 10s
      start_period: 30s

  rsshub:
    image: diygod/rsshub:chromium-bundled
    container_name: rsshub
    restart: unless-stopped
    ports:
      - '1200:1200'
    environment:
      NODE_ENV: production
      CACHE_TYPE: redis
      REDIS_URL: 'redis://redis:6379/'
      PUPPETEER_WS_ENDPOINT: 'ws://browserless:3000'
    depends_on:
      - redis
      - browserless

  browserless:
    image: browserless/chrome:latest
    container_name: browserless
    restart: unless-stopped
    ulimits:
      core:
        hard: 0
        soft: 0

  redis:
    image: redis:alpine
    container_name: redis
    restart: unless-stopped
    volumes:
      - redis-data:/data

volumes:
  miniflux-db:
  redis-data:
```

3. 启动容器。

```bash
# 启动
docker compose up -d
# 查看容器状态，STATUS 显示正常即可
docker ps -a
```

4. 配置。
    - 访问`http://ip:port`，输入刚才自定义的用户名和密码。
    - 设置里启动 fever，然后设置用户名和密码。

## 3.配置 HTTPS

1. 安装`acme.sh`。

```bash

```

2. 注册 cloudflare 并将域名解析到 cloudflare。



3. 获取 cloudflare 的 Global API Key。

```bash
export CF_Key="3a...08"          # 替换为 Global API Key
export CF_Email="test@gmail.com" # 替换为 cloudflare 的注册邮箱
```

4. `acme.sh`使用 cloudflare 的 DNS API 申请泛域名证书，任意二级域名都可以使用。将域名换成自己的域名。

```bash
# 申请证书
acme.sh --issue --dns dns_cf -d backtraxe.top -d "*.backtraxe.top"

# 创建目录
mkdir -p /etc/nginx/ssl/backtraxe.top

# 复制证书到指定目录
acme.sh --install-cert -d backtraxe.top \
--key-file /etc/nginx/ssl/backtraxe.top/backtraxe.top.key \
--fullchain-file /etc/nginx/ssl/backtraxe.top/fullchain.cer \
--reloadcmd "service nginx force-reload"
```

5. 配置 Nginx，实现`rss.backtraxe.top`代理`https://127.0.0.1:8888`，`rsshub.backtraxe.top`代理`https://127.0.0.1:1200`。

```conf
server {
    listen 443 ssl;
    server_name rsshub.backtraxe.top;

    ssl_certificate "/etc/nginx/ssl/backtraxe.top/fullchain.cer";
    ssl_certificate_key "/etc/nginx/ssl/backtraxe.top/backtraxe.top.key";
    ssl_session_cache shared:SSL:1m;
    ssl_session_timeout  10m;
    ssl_ciphers HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers on;

    location / {
        proxy_set_header Host $http_host;
        proxy_pass http://127.0.0.1:1200;
    }
}

server {
    listen 443 ssl;
    server_name rss.backtraxe.top;

    ssl_certificate "/etc/nginx/ssl/backtraxe.top/fullchain.cer";
    ssl_certificate_key "/etc/nginx/ssl/backtraxe.top/backtraxe.top.key";
    ssl_session_cache shared:SSL:1m;
    ssl_session_timeout  10m;
    ssl_ciphers HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers on;

    location / {
        proxy_set_header Host $http_host;
        proxy_pass http://127.0.0.1:8888;
    }
}

server {
    listen 80;
    listen [::]:80;

    server_name backtraxe.top;

    return 301 https://$http_host$request_uri;
}
```

## 4.配置客户端

Fever API:
- url: `http://ip:port/fever/`
- username: `自定义 fever 的用户名`
- password: `自定义 fever 的密码`

## 参考

1. [Install Docker Engine on Debian | Docker Documentation](https://docs.docker.com/engine/install/debian/)
1. [Docker Installation | Miniflux Installation Instructions](https://miniflux.app/docs/installation.html#docker)
1. [用Miniflux自建轻便好用的RSS服务](https://zoomyale.com/2018/miniflux_rss/)
1. [Docker Compose 部署 | RSSHub](https://docs.rsshub.app/install/#docker-compose-bu-shu)
1. []()
1. []()
1. []()
1. []()
1. []()

https://www.psay.cn/toss/126.html
https://cloud.tencent.com/developer/article/1840147
https://suzuhafan.com/tutorials/acmesh-use-cloudflare-to-create-ssl.html
https://luyuhuang.tech/2020/06/03/cloudflare-free-https.html
https://learnku.com/articles/13496/lets-encrypt-pan-domain-name-application-and-configuration
https://luyuhuang.tech/2020/06/03/cloudflare-free-https.html
https://mincong.io/cn/nginx-subdomains/

