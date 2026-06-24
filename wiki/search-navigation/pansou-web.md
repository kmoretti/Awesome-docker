# pansou-web 部署与更新教程

## 这个项目是做什么的

pansou-web 是一个网盘资源搜索前端项目，适合做资源检索入口。

## 这篇教程适合谁

- 想做资源搜索入口的人
- 想给自己站点增加检索页的人
- 想练习带日志目录和数据目录项目部署的人

## 你需要准备什么

- 已安装 Docker 的服务器
- 建议目录：`/opt/docker/pansou-web`
- 域名：`pan.your-domain.com`

## 部署前先检查

```bash
docker -v
docker compose version
```

## 建议目录结构

```bash
/opt/docker/pansou-web/
├── compose.yml
└── data/
    ├── data/
    └── logs/
```

## 第一步：创建项目目录

```bash
mkdir -p /opt/docker/pansou-web/data/data
mkdir -p /opt/docker/pansou-web/data/logs
cd /opt/docker/pansou-web
```

## 第二步：创建 compose 文件

```yaml
services:
  pansou:
    image: ghcr.io/fish2018/pansou-web:latest
    container_name: pansou
    restart: unless-stopped
    ports:
      - "80:80"
    environment:
      - DOMAIN=pan.your-domain.com
      - PANSOU_PORT=8888
    volumes:
      - ./data/data:/app/data
      - ./data/logs:/app/logs
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

建议通过域名访问。
如果没做反向代理，也可以临时通过端口访问。

## 数据目录

- 数据目录：`/opt/docker/pansou-web/data/data`
- 日志目录：`/opt/docker/pansou-web/data/logs`

## 如何备份

```bash
tar -czvf pansou-web-backup.tar.gz /opt/docker/pansou-web/data
```

## 如何更新

```bash
cd /opt/docker/pansou-web
docker compose pull
docker compose up -d
```

## 如何查看日志

```bash
docker logs -f pansou
```

## 常见问题

### 1. 页面打不开

请检查反向代理是否正常转发。

### 2. 搜索结果异常

请检查项目依赖的数据源或接口配置是否正常。
