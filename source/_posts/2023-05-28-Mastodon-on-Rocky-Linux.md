---
title: 在 Rocky Linux 上安装 Mastodon
date: 2023-05-28 00:02:19
cover: https://img.cubik65536.top/mastodon-logo.png
categories:
  - 折腾
  - 教程
tags:
  - Mastodon
  - Rocky Linux
---

前一段时间，我在我自己的服务器上部署了 [我自己的 Mastodon 示例](https://mstdn.ixor.tech) ，在本文中，我将简单展示如何在 Rocky Linux 上搭配 Docker 和 Docker Compose 来部署 Mastodon。

<!-- more -->

## 安装 Docker 和 Docker Compose

(如果你已经安装了 Docker 和 Docker Compose，可以跳过这一步)

### 使用 dnf 工具添加 Docker 仓库

``` bash
sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

### 安装相关软件包

``` bash
sudo dnf -y install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

### 启动 Docker 服务

``` bash
sudo systemctl --now enable docker
```

(参考：[Docker - Install Engine - Rocky Linux  Documentation](https://docs.rockylinux.org/gemstones/docker/))

## 创建 Mastodon 用户并创建 Mastodon 目录

``` bash
mkdir -p /home/mastodon/mastodon
cd /home/mastodon/mastodon/
```

## 拉取 Mastodon Docker 镜像

``` bash
docker pull ghcr.io/mastodon/mastodon:latest
```

## 下载 Mastodon Docker Compose 文件

``` bash
wget https://raw.githubusercontent.com/tootsuite/mastodon/master/docker-compose.yml
```

## 修改 Mastodon Docker Compose 文件

使用 vim 编辑 docker-compose.yml 文件

``` bash
vim docker-compose.yml
```

将 docker-compose.yml 文件中的内容修改为如下内容：

``` yaml
version: '3'
services:
  db:
    restart: always
    image: postgres:14-alpine
    shm_size: 256mb
    networks:
      - internal_network
    healthcheck:
      test: ['CMD', 'pg_isready', '-U', 'postgres']
    volumes:
      - ./postgres14:/var/lib/postgresql/data
    environment:
      - 'POSTGRES_HOST_AUTH_METHOD=trust'

  redis:
    restart: always
    image: redis:7-alpine
    networks:
      - internal_network
    healthcheck:
      test: ['CMD', 'redis-cli', 'ping']
    volumes:
      - ./redis:/data

  es:
    restart: always
    image: docker.elastic.co/elasticsearch/elasticsearch:7.17.9
    environment:
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m -Des.enforce.bootstrap.checks=true"
      - "xpack.license.self_generated.type=basic"
      - "xpack.security.enabled=false"
      - "xpack.watcher.enabled=false"
      - "xpack.graph.enabled=false"
      - "xpack.ml.enabled=false"
      - "bootstrap.memory_lock=true"
      - "cluster.name=es-mastodon"
      - "discovery.type=single-node"
      - "thread_pool.write.queue_size=1000"
    networks:
       - external_network
       - internal_network
    healthcheck:
       test: ["CMD-SHELL", "curl --silent --fail localhost:9200/_cluster/health || exit 1"]
    volumes:
       - ./elasticsearch:/var/lib/elasticsearch/data
    ulimits:
      memlock:
        soft: -1
        hard: -1
      nofile:
        soft: 65536
        hard: 65536
    ports:
      - '127.0.0.1:9200:9200'

  web:
    build: .
    image: ghcr.io/mastodon/mastodon:latest
    restart: always
    env_file: .env.production
    command: bash -c "rm -f /mastodon/tmp/pids/server.pid; bundle exec rails s -p 3000"
    networks:
      - external_network
      - internal_network
    healthcheck:
      # prettier-ignore
      test: ['CMD-SHELL', 'wget -q --spider --proxy=off localhost:3000/health || exit 1']
    ports:
      - '127.0.0.1:3000:3000'
    depends_on:
      - db
      - redis
      - es
    volumes:
      - ./public/system:/mastodon/public/system

  streaming:
    build: .
    image: ghcr.io/mastodon/mastodon:latest
    restart: always
    env_file: .env.production
    command: node ./streaming
    networks:
      - external_network
      - internal_network
    healthcheck:
      # prettier-ignore
      test: ['CMD-SHELL', 'wget -q --spider --proxy=off localhost:4000/api/v1/streaming/health || exit 1']
    ports:
      - '127.0.0.1:4000:4000'
    depends_on:
      - db
      - redis

  sidekiq:
    build: .
    image: ghcr.io/mastodon/mastodon:latest
    restart: always
    env_file: .env.production
    command: bundle exec sidekiq
    depends_on:
      - db
      - redis
    networks:
      - external_network
      - internal_network
    volumes:
      - ./public/system:/mastodon/public/system
    healthcheck:
      test: ['CMD-SHELL', "ps aux | grep '[s]idekiq\ 6' || false"]

  ## Uncomment to enable federation with tor instances along with adding the following ENV variables
  ## http_proxy=http://privoxy:8118
  ## ALLOW_ACCESS_TO_HIDDEN_SERVICE=true
  # tor:
  #   image: sirboops/tor
  #   networks:
  #      - external_network
  #      - internal_network
  #
  # privoxy:
  #   image: sirboops/privoxy
  #   volumes:
  #     - ./priv-config:/opt/config
  #   networks:
  #     - external_network
  #     - internal_network

networks:
  external_network:
  internal_network:
    internal: true
```

## 初始化 PostgreSQL 数据库

刚才 `docker-compose.yml` 文件中，数据库 (`db`) 部分的地址为 `./postgres14:/var/lib/postgresql/data`，因此你的数据库绝对地址为 `/home/mastodon/mastodon/postgres14`。

### 执行以下命令

``` bash
docker run --name postgres14 -v /home/mastodon/mastodon/postgres14:/var/lib/postgresql/data -e POSTGRES_PASSWORD=[数据库管理员密码] --rm -d postgres:14-alpine
```

执行完后，检查 `/home/mastodon/mastodon/postgres14`，应该出现 PostgreSQL 相关的多个文件/文件夹。

### 然后执行以下命令

``` bash
docker exec -it postgres14 psql -U postgres
```

### 然后创建 mastodon 用户并退出 PostgreSQL

``` sql
CREATE USER mastodon WITH PASSWORD '[数据库密码（最好和数据库管理员密码不一样）]' CREATEDB;
\q
```

{% note color:yellow 提示 你可以使用 `openssl rand -base64 32` 命令来生成随机密码 %}

### 最后停止 PostgreSQL 容器

``` bash
docker stop postgres14
```

## 配置 Mastodon

### 配置文件

在 `/home/mastodon/mastodon` 目录下创建 `.env.production` 文件

``` bash
touch .env.production
```

### 交互式配置

执行以下命令使用 Mastodon 的交互式配置工具

``` bash
docker compose run --rm web bundle exec rake mastodon:setup
```

然后按照以下提示进行配置

{% note color:yellow 提示 以下选项带有 `⭐️` 表示则意味着你必须自行填写其内容，其他的你可以直接使用默认值 %}

- ⭐️ Domain name: **[你的域名]**
- ⭐️ Single user mode disables registrations and redirects the landing page to your public profile.
    Do you want to enable single user mode? **yes**
    {% note color:yellow 提示 如果你只希望自己一个人使用这个实例，并将这个实例当作一个类似于你个人内容更新站的话，填写 `yes`。如果你希望公开运营这个服务器，或者与朋友共用这个实例，填写 `no`。 %}
- ⭐️ Are you using Docker to run Mastodon? **Yes**
    {% note color:yellow 提示 此处必须填写 `Yes` %}
- PostgreSQL host: db
- PostgreSQL port: 5432
- ⭐️ Name of PostgreSQL database: mastodon
- Name of PostgreSQL user: postgres
- ⭐️ Password of PostgreSQL user: [你设置好的数据库密码]
    {% note color:yellow 提示 此处密码输入/粘贴后不会显示，如果一切正常会出现 `Database configuration works! 🎆` 提示 %}
- Redis host: redis
- Redis port: 6379
- Redis password: 此处没有密码，直接回车。如果一切正常会出现 `Redis configuration works! 🎆` 提示
- ⭐️ Do you want to store uploaded files on the cloud? **No**
    {% note color:yellow 提示 如果你希望将上传到 Mastodon 实例的文件存储到云端，填写 `Yes`，然后按照提示进行配置（本文不涉及这部分配置）。如果你希望将上传到 Mastodon 实例的文件存储到本地，填写 `No`。 %}
- ⭐️ Do you want to send e-mails from localhost? **No**
    {% note color:yellow 提示 如果你希望使用本地的 SMTP 服务器发送邮件，填写 `Yes`，然后按照提示进行配置（本文不涉及这部分配置）。如果你希望使用第三方 SMTP 服务器发送邮件，填写 `No`。 %}
- ⭐️ SMTP server: [你的 SMTP 服务器地址]
- ⭐️ SMTP port: [你的 SMTP 服务器端口]
- ⭐️ SMTP username: [你的 SMTP 服务器用户名 (邮箱地址)]
- ⭐️ SMTP password: [你的 SMTP 服务器密码（邮箱密码）]
    {% note color:yellow 提示 此处密码输入/粘贴后不会显示 %}
- ⭐️ SMTP authentication: [你的 SMTP 服务器认证方式]
    {% note color:yellow 提示 此处的认证方式根据你的 SMTP 提供商而定，以 Office365 为例，这里应该使用 `login` %}
- ⭐️ SMTP OpenSSL verify mode: (使用上下选择)
    {% note color:yellow 提示 此处的认证模式根据你的 SMTP 提供商而定，以 Office365 为例，这里应该使用 `none` %}
- Enable STARTTLS: auto
- E-mail address to send e-mails "from": [你的邮箱昵称 <你的邮箱地址>]
    {% note color:yellow 提示 你可以尝试使用默认配置，或者进行部分修改 %}
- ⭐️ Send a test e-mail with this configuration right now? **Yes**
- ⭐️ Send test e-mail to: [你的个人邮箱地址]
    {% note color:yellow 提示 此处会测试你的 SMTP 配置是否正确，如果你的个人邮箱收到了一封测试邮件，则一切正常 %}
- ⭐️ This configuration will be written to .env.production
    Save configuration? (Y/n) **Yes**
- Below is your configuration, save it to an .env.production file outside Docker:

然后会出现 .env.production 配置，复制下来并将内容粘贴到 `/home/mastodon/mastodon/.env.production` 文件中。

## 为相应文件夹赋权

``` bash
chown 991:991 -R ./public
chown -R 70:70 ./postgres14
```

## 启动 Mastodon

``` bash
docker-compose down
docker-compose up -d
```

## 配置 Nginx

在进行这一步之前，你需要确保你的域名已经解析到了你的服务器 IP 上，并且你的服务器已经安装了 Nginx。

### 创建 SSL 证书

``` bash
# 安装 certbot (在此之前，请确保你的服务器已经安装了 snapd)
sudo snap install --classic certbot
# 创建软链接
sudo ln -s /snap/bin/certbot /usr/bin/certbot
# 生成证书
sudo certbot certonly --nginx -d [你的域名]
```

### 创建 Nginx 配置文件

``` bash
touch /etc/nginx/conf.d/[你的域名].conf
```

### 编辑 Nginx 配置文件

``` bash
vim /etc/nginx/nginx.conf
```

在 `http` 块中添加以下内容（或确保以下内容已经存在）

``` nginx
include /etc/nginx/conf.d/*.conf;
```

``` bash
vim /etc/nginx/conf.d/[你的域名].conf
```

按照以下模板进行配置

{% folding child:codeblock Mastodon Nginx 配置模板 %}

``` nginx
map $http_upgrade $connection_upgrade {
  default upgrade;
  ''      close;
}

upstream backend {
    server 127.0.0.1:3000 fail_timeout=0;
}

upstream streaming {
    server 127.0.0.1:4000 fail_timeout=0;
}

proxy_cache_path /var/cache/nginx levels=1:2 keys_zone=CACHE:10m inactive=7d max_size=1g;

server {
  listen 80;
  listen [::]:80;
  server_name [你的域名];
  root /home/mastodon/mastodon/public;
  location /.well-known/acme-challenge/ { allow all; }
  location / { return 301 https://$host$request_uri; }
}

server {
  listen 443 ssl http2;
  listen [::]:443 ssl http2;
  server_name [你的域名];

  ssl_protocols TLSv1.2 TLSv1.3;
  ssl_ciphers HIGH:!MEDIUM:!LOW:!aNULL:!NULL:!SHA;
  ssl_prefer_server_ciphers on;
  ssl_session_cache shared:SSL:10m;
  ssl_session_tickets off;

  ssl_certificate     /etc/letsencrypt/live/[你的域名]/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/[你的域名]/privkey.pem;

  keepalive_timeout    70;
  sendfile             on;
  client_max_body_size 99m;

  root /home/mastodon/mastodon/public;

  gzip on;
  gzip_disable "msie6";
  gzip_vary on;
  gzip_proxied any;
  gzip_comp_level 6;
  gzip_buffers 16 8k;
  gzip_http_version 1.1;
  gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript image/svg+xml image/x-icon;

  location / {
    try_files $uri @proxy;
  }

  # If Docker is used for deployment and Rails serves static files,
  # then needed must replace line `try_files $uri =404;` with `try_files $uri @proxy;`.
  location = /sw.js {
    add_header Cache-Control "public, max-age=604800, must-revalidate";
    add_header Strict-Transport-Security "max-age=63072000; includeSubDomains";
    try_files $uri @proxy;
  }

  location ~ ^/assets/ {
    add_header Cache-Control "public, max-age=2419200, must-revalidate";
    add_header Strict-Transport-Security "max-age=63072000; includeSubDomains";
    try_files $uri @proxy;
  }

  location ~ ^/avatars/ {
    add_header Cache-Control "public, max-age=2419200, must-revalidate";
    add_header Strict-Transport-Security "max-age=63072000; includeSubDomains";
    try_files $uri @proxy;
  }

  location ~ ^/emoji/ {
    add_header Cache-Control "public, max-age=2419200, must-revalidate";
    add_header Strict-Transport-Security "max-age=63072000; includeSubDomains";
    try_files $uri @proxy;
  }

  location ~ ^/headers/ {
    add_header Cache-Control "public, max-age=2419200, must-revalidate";
    add_header Strict-Transport-Security "max-age=63072000; includeSubDomains";
    try_files $uri @proxy;
  }

  location ~ ^/packs/ {
    add_header Cache-Control "public, max-age=2419200, must-revalidate";
    add_header Strict-Transport-Security "max-age=63072000; includeSubDomains";
    try_files $uri @proxy;
  }

  location ~ ^/shortcuts/ {
    add_header Cache-Control "public, max-age=2419200, must-revalidate";
    add_header Strict-Transport-Security "max-age=63072000; includeSubDomains";
    try_files $uri @proxy;
  }

  location ~ ^/sounds/ {
    add_header Cache-Control "public, max-age=2419200, must-revalidate";
    add_header Strict-Transport-Security "max-age=63072000; includeSubDomains";
    try_files $uri @proxy;
  }

  location ~ ^/system/ {
    add_header Cache-Control "public, max-age=2419200, immutable";
    add_header Strict-Transport-Security "max-age=63072000; includeSubDomains";
    try_files $uri @proxy;
  }

  location ^~ /api/v1/streaming {
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header Proxy "";

    proxy_pass http://streaming;
    proxy_buffering off;
    proxy_redirect off;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection $connection_upgrade;

    add_header Strict-Transport-Security "max-age=63072000; includeSubDomains";

    tcp_nodelay on;
  }

  location @proxy {
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header Proxy "";
    proxy_pass_header Server;

    proxy_pass http://backend;
    proxy_buffering on;
    proxy_redirect off;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection $connection_upgrade;

    proxy_cache CACHE;
    proxy_cache_valid 200 7d;
    proxy_cache_valid 410 24h;
    proxy_cache_use_stale error timeout updating http_500 http_502 http_503 http_504;
    add_header X-Cached $upstream_cache_status;

    tcp_nodelay on;
  }

  error_page 404 500 501 502 503 504 /500.html;
}
```

{% endfolding %}

在配置完成后，执行 `sudo nginx -t` 检查配置文件是否正确，如果正确则执行 `sudo systemctl restart nginx` 重启 Nginx 服务。

{% quot "到现在，你的 Mastodon 实例就启动完成了！访问你的域名来试试吧！" el:h2 %}

## 修改配置文件

如果你想修改 Mastodon 的配置文件，可以使用 `vim /home/mastodon/mastodon/.env.production` 命令来编辑配置文件

修改完成后，使用以下命令重启 Mastodon 服务即可：

```bash
docker-compose down
docker-compose up -d
```

## 使用 toolctl 管理 Mastodon

### 进入 Docker 环境中操作

```bash
cd /home/mastodon/mastodon
docker exec -it web /bin/bash # 此处的 web 是容器名，如果你的容器名不是 web，请自行修改
```

### 在 `/home/mastodon/mastodon` 目录下操作

```bash
cd /home/mastodon/mastodon
docker-compose run --rm web bin/tootctl 具体命令
```

### 在任意位置操作

```bash
docker exec web tootctl 具体命令
```

## 升级

如果需要升级 Mastodon，可以使用以下命令：

```bash
cd /home/mastodon/mastodon
docker-compose down
docker pull ghcr.io/mastodon/mastodon:latest
```

如果你的 `docker-compose.yml` 文件中制定了具体版本号，那么你就需要更改版本号。如果你填写的是 `latest`，那么就不需要更改。

然后使用以下命令重启 Mastodon 服务即可：

```bash
docker-compose up -d
```

如果官方升级提升要求你执行 `docker-compose run --rm web rails db:migrate` 之类的命令，那么你可以在启动后执行。

在确认升级没有问题后，可以执行 `docker image prune -a` 来清理旧的镜像。

## 如果在操作过程中出现了任何问题...

如果你在操作过程中出现了任何问题，而且没有对站点进行魔改，那么在 Docker 环境外通过以下命令重新搭建容器即可：

```bash
cd /home/mastodon/mastodon
docker-compose down
docker-compose up -d
```

## 开启全文搜索

如果你想开启全文搜索，可以使用以下命令：

```bash
cd /home/mastodon/mastodon
vim docker-compose.yml
```

将 `docker-compose.yml` 文件中的 `es` 部分前面的注释符号 `#` 去掉

然后编辑 `.env.production` 文件，加上：

```.env.production
ES_ENABLED=true
ES_HOST=es
ES_PORT=9200
```

然后重启：

```bash
docker-compose down
docker-compose up -d
```

等 Mastodon 路径下出现 `elasticsearch` 文件夹后，赋权并再次重启：

```bash
chown 1000:1000 -R elasticsearch
docker-compose down
docker-compose up -d
```

全文搜索就开启了！

你可以使用 `docker-compose run --rm web bin/tootctl search deploy` 建立对之前嘟文的搜索索引。

{% quot "那么，关于 Mastodon 部署的内容就到这里了！GL&HF！" el:h2 %}
