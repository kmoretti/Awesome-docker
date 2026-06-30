# dox 部署与更新教程

## 这个项目是做什么的

`dox` 是一个自托管 Todo 应用。
它既可以作为终端里的待办工具使用，也可以作为轻量的个人任务管理服务来部署。

## 这篇教程适合谁

- 想搭建自己的待办系统的人
- 喜欢终端工作流的人
- 想部署一个轻量自托管任务管理服务的人

## 你需要准备什么

- 已安装 Docker 的服务器
- 建议目录：`/opt/docker/dox`
- 端口 `6278`
- 可选域名：`dox.your-domain.com`

## 部署前先检查

```bash
docker -v
docker compose version
```

## 建议目录结构

```bash
/opt/docker/dox/
├── compose.yml
└── data/
```

## 第一步：创建项目目录

```bash
mkdir -p /opt/docker/dox/data
cd /opt/docker/dox
```

## 第二步：创建 compose 文件

```yaml
services:
  dox:
    image: sn0wl1n/dox:latest
    container_name: dox
    restart: unless-stopped
    ports:
      - "6278:6278"
    volumes:
      - ./data:/app/data
```

## 第三步：检查配置

```bash
docker compose config
```

## 第四步：启动

```bash
docker compose up -d
```

## 第五步：访问

浏览器或客户端连接到：

```text
http://服务器IP:6278
```

如果你做了反向代理，也可以用：

```text
https://dox.your-domain.com
```

## 数据目录

- 数据目录：`/opt/docker/dox/data`

官方说明里提到这是一个“一个容器 + 一个 SQLite 文件”的结构，所以这个目录非常重要。

## 如何备份

因为底层是 SQLite，最简单的方式就是直接备份数据目录：

```bash
tar -czvf dox-backup.tar.gz /opt/docker/dox/data
```

## 如何更新

```bash
cd /opt/docker/dox
docker compose pull
docker compose up -d
```

## 如何查看日志

```bash
docker logs -f dox
```

## 常见问题

### 1. 页面打不开

先检查：

- 容器是否成功启动
- `6278` 端口是否已放行
- 安全组和防火墙是否允许访问

### 2. 数据丢失

请确认是否正确挂载了：

```yaml
- ./data:/app/data
```

如果没有挂载，容器重建后数据可能会丢失。

### 3. 我想用域名访问

推荐使用 `Nginx Proxy Manager` 把域名转发到 `6278` 端口。
