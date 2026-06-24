# OpenList 部署与更新教程

## 这个项目是做什么的

OpenList 是一个文件列表和网盘聚合管理服务，适合挂载多个存储并统一管理。

## 这篇教程适合谁

- 想做个人网盘入口的人
- 想挂载多个云盘或本地目录的人
- 想学习数据持久化目录的重要性的人

## 你需要准备什么

- 已安装 Docker 的服务器
- 建议目录：`/opt/docker/openlist`
- 端口 `5244`
- 可选域名：`list.your-domain.com`

## 部署前先检查

```bash
docker -v
docker compose version
```

## 建议目录结构

```bash
/opt/docker/openlist/
├── compose.yml
└── data/
```

## 第一步：创建项目目录

```bash
mkdir -p /opt/docker/openlist/data
cd /opt/docker/openlist
```

## 第二步：创建 compose 文件

```yaml
services:
  openlist:
    image: openlistteam/openlist:latest
    container_name: openlist
    restart: always
    ports:
      - "5244:5244"
      - "5245:5245"
    environment:
      - UMASK=022
      - TZ=Asia/Shanghai
    volumes:
      - ./data:/opt/openlist/data
```

## 第三步：检查配置

```bash
docker compose config
```

## 第四步：启动项目

```bash
docker compose up -d
```

## 第五步：访问页面

```text
http://服务器IP:5244
```

如果你做了反向代理，也可以使用：

```text
https://list.your-domain.com
```

## 第六步：第一次进入后建议做什么

首次使用建议先完成：

1. 初始化管理员账号
2. 修改默认密码
3. 添加第一个存储
4. 测试文件访问是否正常
5. 如有需要，开启公开分享

## 数据保存在哪里

- 数据目录：`/opt/docker/openlist/data`

这个目录非常重要，通常会保存：

- 配置文件
- 存储挂载信息
- 账号信息
- 运行数据

## 如何备份

```bash
tar -czvf openlist-backup.tar.gz /opt/docker/openlist/data
```

## 如何更新

```bash
cd /opt/docker/openlist
docker compose pull
docker compose up -d
```

## 如何查看日志

```bash
docker logs -f openlist
```

## 常见问题

### 1. 页面打不开

先确认：

- 容器是否运行中
- `5244` 端口是否已开放
- 防火墙和安全组是否放行

### 2. 存储挂载失败

常见原因：

- 存储配置填错
- 认证信息失效
- 服务器无法访问目标存储

### 3. 更新后数据丢失怎么办

如果你正确挂载了 `./data:/opt/openlist/data`，一般不会丢。
如果没挂载，重建容器时就可能丢配置。
