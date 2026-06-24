# SearXNG 部署与更新教程

## 这个项目是做什么的

SearXNG 是一个聚合搜索引擎，可以把多个搜索源整合到一个搜索入口中。

## 这篇教程适合谁

- 想自建搜索入口的人
- 想减少对单一搜索引擎依赖的人
- 想练习多服务 `docker compose` 部署的人

## 你需要准备什么

- 已安装 Docker 的服务器
- 建议目录：`/opt/docker/searxng`
- 端口 `8080`
- 可选域名：`search.your-domain.com`

## 部署前先检查

```bash
docker -v
docker compose version
```

## 建议目录结构

```bash
/opt/docker/searxng/
├── compose.yml
└── data/
```

## 第一步：创建项目目录

```bash
mkdir -p /opt/docker/searxng/data
cd /opt/docker/searxng
```

## 第二步：创建 compose 文件

```yaml
services:
  searxng:
    image: searxng/searxng:latest
    container_name: searxng
    restart: unless-stopped
    ports:
      - "8080:8080"
    volumes:
      - ./data:/etc/searxng
    environment:
      - SEARXNG_BASE_URL=https://search.your-domain.com/

  redis:
    image: redis:7-alpine
    container_name: searxng-redis
    restart: unless-stopped
    command: redis-server --save "" --appendonly "no"
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

```text
http://服务器IP:8080
```

## 第六步：如果你想用域名访问

推荐使用反向代理，把：

- `search.your-domain.com`
- 转发到 `服务器IP:8080`

## 数据保存在哪里

- 配置目录：`/opt/docker/searxng/data`

## 如何备份

```bash
tar -czvf searxng-backup.tar.gz /opt/docker/searxng/data
```

## 如何更新

```bash
cd /opt/docker/searxng
docker compose pull
docker compose up -d
```

## 如何查看日志

```bash
docker logs -f searxng
docker logs -f searxng-redis
```

## 常见问题

### 1. 页面打不开

先确认 `8080` 端口是否开放。

### 2. 搜索结果为空

常见原因：

- 某些搜索源临时不可用
- 网络环境访问不到对应搜索源
- 默认配置需要调整

### 3. 我想自定义搜索设置

通常需要修改挂载目录里的配置文件，也就是：

```text
/opt/docker/searxng/data
```
