# ChronoFrame 部署与更新教程

## 这个项目是做什么的

ChronoFrame 是一个自托管照片展示与管理应用。
适合用来管理照片、按时间线浏览图片，并展示图片相关信息。

## 这篇教程适合谁

- 想搭建自己的照片展示站的人
- 想学习带 `.env` 配置的 Docker 项目部署方式的人
- 想给照片加时间线展示的人

## 部署前你需要准备什么

- 一台已经安装好 Docker 的服务器
- 建议目录：`/opt/docker/chronoframe`
- 一个访问域名，例如 `photo.your-domain.com`
- 按项目要求准备 `.env` 配置文件

## 部署前先检查

```bash
docker -v
docker compose version
```

## 建议目录结构

```bash
/opt/docker/chronoframe/
├── compose.yml
├── .env
└── data/
```

## 第一步：创建项目目录

```bash
mkdir -p /opt/docker/chronoframe/data
cd /opt/docker/chronoframe
```

## 第二步：创建 compose 文件

```yaml
services:
  chronoframe:
    image: ghcr.io/hoshinosuzumi/chronoframe:latest
    container_name: chronoframe
    restart: unless-stopped
    ports:
      - "3000:3000"
    env_file:
      - .env
    volumes:
      - ./data:/app/data
```

## 第三步：准备 `.env`

把项目官方文档要求的环境变量写进 `.env` 文件。
如果你使用数据库、对象存储或地图功能，也要按官方说明填写。

## 第四步：检查配置

```bash
docker compose config
```

## 第五步：启动容器

```bash
docker compose up -d
```

## 第六步：访问页面

```text
http://服务器IP:3000
```

或：

```text
https://photo.your-domain.com
```

## 数据保存在哪里

- 容器内目录：`/app/data`
- 宿主机目录：`/opt/docker/chronoframe/data`

## 如何备份

```bash
tar -czvf chronoframe-backup.tar.gz /opt/docker/chronoframe/data
```

如果 `.env` 里有重要配置，也建议一并备份。

## 如何更新

```bash
cd /opt/docker/chronoframe
docker compose pull
docker compose up -d
```

## 如何查看日志

```bash
docker logs -f chronoframe
```

## 常见问题

### 1. 启动失败

大概率是 `.env` 配置不完整，请对照官方 README 补齐。

### 2. 图片不显示

请检查：

- 挂载目录是否正确
- 文件权限是否正常
- 图片是否真的存在于数据目录中

### 3. 我想用域名访问

推荐使用 `Nginx Proxy Manager` 做反向代理，把域名转发到 `3000` 端口。
