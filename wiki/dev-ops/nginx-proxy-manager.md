# Nginx Proxy Manager 部署与更新教程

## 这个项目是做什么的

Nginx Proxy Manager 是一个可视化反向代理面板。
你可以用网页方式配置域名转发、HTTPS 证书和反向代理规则。

## 这篇教程适合谁

这篇教程适合：

- 想给多个 Docker 项目绑定域名的人
- 想用可视化方式申请 HTTPS 证书的人
- 想统一管理 80 和 443 端口的人

## 部署前你需要准备什么

- 一台已经安装好 Docker 的服务器
- 80、81、443 端口可用
- 一个已经解析到服务器的域名
- 建议目录：`/opt/docker/nginx-proxy-manager`

## 部署前先检查

```bash
docker -v
docker compose version
```

还建议确认端口没有被占用：

```bash
ss -tlnp
```

## 建议目录结构

```bash
/opt/docker/nginx-proxy-manager/
├── compose.yml
└── data/
    ├── data/
    └── letsencrypt/
```

## 第一步：创建项目目录

```bash
mkdir -p /opt/docker/nginx-proxy-manager/data/data
mkdir -p /opt/docker/nginx-proxy-manager/data/letsencrypt
cd /opt/docker/nginx-proxy-manager
```

## 第二步：创建 compose 文件

```yaml
services:
  nginx-proxy-manager:
    image: jc21/nginx-proxy-manager:latest
    container_name: nginx-proxy-manager
    restart: unless-stopped
    ports:
      - "80:80"
      - "81:81"
      - "443:443"
    volumes:
      - ./data/data:/data
      - ./data/letsencrypt:/etc/letsencrypt
```

## 第三步：检查配置文件

```bash
docker compose config
```

## 第四步：启动容器

```bash
docker compose up -d
```

## 第五步：打开管理后台

浏览器访问：

```text
http://服务器IP:81
```

常见绑定域名：

```text
https://npm.your-domain.com
```

## 第六步：第一次登录后建议先做什么

第一次进入后台后，建议按这个顺序操作：

1. 修改默认登录信息
2. 新建一个 Proxy Host
3. 填写你的域名
4. 把它转发到目标容器端口
5. 申请 SSL 证书
6. 打开强制 HTTPS

## 数据保存在哪里

- 面板数据：`/opt/docker/nginx-proxy-manager/data/data`
- 证书数据：`/opt/docker/nginx-proxy-manager/data/letsencrypt`

## 如何备份

建议备份整个数据目录：

```bash
tar -czvf nginx-proxy-manager-backup.tar.gz /opt/docker/nginx-proxy-manager/data
```

## 如何更新

```bash
cd /opt/docker/nginx-proxy-manager
docker compose pull
docker compose up -d
```

## 如何查看日志

```bash
docker logs -f nginx-proxy-manager
```

## 典型使用方式

比如你已经部署了 Uptime Kuma，端口是 `3001`。
那么你可以在 Nginx Proxy Manager 里：

- 域名填写：`status.your-domain.com`
- Forward Hostname / IP：服务器 IP 或容器可访问地址
- Forward Port：`3001`

然后申请证书，就可以通过 HTTPS 访问。

## 常见问题

### 1. 无法申请证书

请确认：

- 域名已经解析到当前服务器
- 80 和 443 端口可以从公网访问
- 没有其他服务占用 80 和 443
- 反向代理目标没有填错

### 2. 后台打不开

先检查 81 端口是否被占用。

### 3. 证书申请一直失败

常见原因：

- DNS 还没生效
- 使用了 CDN 但配置不对
- 服务器的 80 端口没有真正开放

### 4. 我想迁移服务器怎么办

迁移时最重要的是保留：

- `compose.yml`
- `/opt/docker/nginx-proxy-manager/data`
