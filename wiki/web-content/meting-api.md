# Meting API 部署与更新教程

## 这个项目是做什么的

Meting API 是一个音乐接口服务，常见用途是给网站音乐播放器提供数据接口。

## 这篇教程适合谁

- 想给自己网站接音乐播放器的人
- 想搭建音乐 API 接口服务的人
- 想练习环境变量配置项目部署的人

## 你需要准备什么

- 已安装 Docker 的服务器
- 建议目录：`/opt/docker/meting-api`
- 域名：`music-api.your-domain.com`

## 部署前先检查

```bash
docker -v
docker compose version
```

## 建议目录结构

```bash
/opt/docker/meting-api/
└── compose.yml
```

## 第一步：创建项目目录

```bash
mkdir -p /opt/docker/meting-api
cd /opt/docker/meting-api
```

## 第二步：创建 compose 文件

```yaml
services:
  meting-api:
    image: ghcr.io/metowolf/meting-api:latest
    container_name: meting-api
    restart: unless-stopped
    ports:
      - "80:80"
    environment:
      - METING_URL=https://music-api.your-domain.com
      - METING_TOKEN=change-me
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

建议通过反向代理后使用域名访问。

## 数据目录

这个项目一般不强制要求本地数据持久化目录。
配置主要通过环境变量控制。

## 如何更新

```bash
cd /opt/docker/meting-api
docker compose pull
docker compose up -d
```

## 如何查看日志

```bash
docker logs -f meting-api
```

## 常见问题

### 1. 接口请求失败

请检查：

- 域名是否正确
- Token 是否配置正确
- 反向代理是否转发正常
