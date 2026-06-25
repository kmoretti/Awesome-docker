# moments 部署与更新教程

## 这个项目是做什么的

`moments` 是一个类似朋友圈的个人动态内容展示项目。
适合用来记录日常、发布碎片内容，或者做一个轻量的个人生活流页面。

## 这篇教程适合谁

- 想搭建个人动态站的人
- 想做轻量内容展示页的人
- 想部署一个带本地数据目录的内容服务的人

## 你需要准备什么

- 已安装 Docker 的服务器
- 建议目录：`/opt/docker/moments`
- 端口 `3000`
- 可选域名：`moments.your-domain.com`
- 一个自定义的 `JWT_KEY`

## 部署前先检查

```bash
docker -v
docker compose version
```

## 建议目录结构

```bash
/opt/docker/moments/
├── compose.yml
└── data/
```

## 第一步：创建项目目录

```bash
mkdir -p /opt/docker/moments/data
cd /opt/docker/moments
```

## 第二步：创建 compose 文件

```yaml
services:
  moments:
    image: kingwrcy/moments:latest
    container_name: moments
    restart: always
    environment:
      PORT: 3000
      JWT_KEY: change-me
    ports:
      - "3000:3000"
    volumes:
      - ./data:/app/data
```

## 第三步：修改关键配置

请至少把下面这个值改成你自己的：

```text
JWT_KEY
```

不要长期使用示例值。

## 第四步：检查配置

```bash
docker compose config
```

## 第五步：启动

```bash
docker compose up -d
```

## 第六步：访问

```text
http://服务器IP:3000
```

如果你做了反向代理，也可以通过域名访问：

```text
https://moments.your-domain.com
```

## 数据目录

- 数据目录：`/opt/docker/moments/data`

这个目录建议保留好，因为后续内容数据通常都会放在这里。

## 如何备份

```bash
tar -czvf moments-backup.tar.gz /opt/docker/moments/data
```

## 如何更新

```bash
cd /opt/docker/moments
docker compose pull
docker compose up -d
```

## 如何查看日志

```bash
docker logs -f moments
```

## 常见问题

### 1. 页面打不开

先检查：

- 容器是否正常启动
- `3000` 端口是否已经放行
- 防火墙和安全组是否允许访问

### 2. 登录或发布异常

优先检查 `JWT_KEY` 是否配置正确。

### 3. 我想用域名访问

推荐用 `Nginx Proxy Manager` 把域名转发到 `3000` 端口。
