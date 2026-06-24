# Ech0 部署与更新教程

## 这个项目是做什么的

Ech0 是一个轻量的个人发布平台，可以用来写博客、发动态或者做个人主页。

## 这篇教程适合谁

- 想做个人博客或主页的人
- 想自己托管轻量发布平台的人
- 想学习带数据目录和备份目录项目部署的人

## 你需要准备什么

- 已安装 Docker 的服务器
- 建议目录：`/opt/docker/ech0`
- 端口 `6277`
- 可选域名：`echo.your-domain.com`

## 部署前先检查

```bash
docker -v
docker compose version
```

## 建议目录结构

```bash
/opt/docker/ech0/
├── compose.yml
└── data/
    ├── data/
    └── backup/
```

## 第一步：创建项目目录

```bash
mkdir -p /opt/docker/ech0/data/backup
mkdir -p /opt/docker/ech0/data/data
cd /opt/docker/ech0
```

## 第二步：创建 compose 文件

```yaml
services:
  ech0:
    image: sn0wl1n/ech0:latest
    container_name: ech0
    restart: unless-stopped
    ports:
      - "6277:6277"
    environment:
      - JWT_SECRET=change-me
    volumes:
      - ./data/backup:/app/backup
      - ./data/data:/app/data
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
http://服务器IP:6277
```

如果做了反向代理，也可以访问域名。

## 数据目录

- 内容数据：`/opt/docker/ech0/data/data`
- 备份目录：`/opt/docker/ech0/data/backup`

## 如何备份

```bash
tar -czvf ech0-backup.tar.gz /opt/docker/ech0/data
```

## 如何更新

```bash
cd /opt/docker/ech0
docker compose pull
docker compose up -d
```

## 如何查看日志

```bash
docker logs -f ech0
```

## 常见问题

### 1. 页面打不开

先确认 `6277` 端口是否放行。

### 2. 登录异常

请检查 `JWT_SECRET` 是否设置正常。
