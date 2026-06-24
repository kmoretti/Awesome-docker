# busuanzi 部署与更新教程

## 这个项目是做什么的

busuanzi 是一个访问统计组件，可以统计站点访问量、页面浏览量等信息。

## 这篇教程适合谁

- 想给网站增加访问统计的人
- 想练习多服务 compose 的人
- 想学习 Redis 依赖项目部署的人

## 你需要准备什么

- 已安装 Docker 的服务器
- 建议目录：`/opt/docker/busuanzi`
- 端口 `8080`
- Redis

## 部署前先检查

```bash
docker -v
docker compose version
```

## 建议目录结构

```bash
/opt/docker/busuanzi/
├── compose.yml
├── config.yaml
└── data/
    └── redis/
```

## 第一步：创建项目目录

```bash
mkdir -p /opt/docker/busuanzi/data/redis
cd /opt/docker/busuanzi
```

## 第二步：创建 compose 文件

```yaml
services:
  busuanzi:
    image: soxft/busuanzi:latest
    container_name: busuanzi
    restart: unless-stopped
    ports:
      - "8080:8080"
    volumes:
      - ./config.yaml:/app/config.yaml
    depends_on:
      - redis

  redis:
    image: redis:7-alpine
    container_name: busuanzi-redis
    restart: unless-stopped
    volumes:
      - ./data/redis:/data
```

## 第三步：准备配置文件

把项目要求的内容写进 `config.yaml`。

## 第四步：检查配置

```bash
docker compose config
```

## 第五步：启动

```bash
docker compose up -d
```

## 数据目录

- Redis 数据：`/opt/docker/busuanzi/data/redis`
- 配置文件：`/opt/docker/busuanzi/config.yaml`

## 如何更新

```bash
cd /opt/docker/busuanzi
docker compose pull
docker compose up -d
```

## 如何查看日志

```bash
docker logs -f busuanzi
docker logs -f busuanzi-redis
```

## 常见问题

### 1. 统计不生效

请检查配置文件和 Redis 是否正常运行。

### 2. 页面打不开

先确认 `8080` 端口是否放行。
