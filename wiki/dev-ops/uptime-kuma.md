# Uptime Kuma 部署与更新教程

## 这个项目是做什么的

Uptime Kuma 是一个非常好用的服务监控工具。
你可以用它监控网站、接口、服务器端口是否正常，并生成一个状态页。

## 这篇教程适合谁

这篇教程适合：

- 第一次部署 Docker 项目的新手
- 想给自己服务器加一个监控面板的人
- 想练习最基础 Docker Compose 部署的人

## 部署前你需要准备什么

- 一台已经安装好 Docker 的服务器
- 建议已经安装好 Docker Compose
- 一个你准备用来访问它的域名，例如 `status.your-domain.com`
- 建议先准备好数据目录，例如：`/opt/docker/uptime-kuma`

## 部署前先检查

先执行下面两条命令，确认 Docker 正常：

```bash
docker -v
docker compose version
```

如果命令能正常输出版本号，就可以继续。

## 建议目录结构

```bash
/opt/docker/uptime-kuma/
├── compose.yml
└── data/
```

## 第一步：创建项目目录

```bash
mkdir -p /opt/docker/uptime-kuma/data
cd /opt/docker/uptime-kuma
```

## 第二步：创建 compose 文件

新建 `compose.yml` 文件，内容如下：

```yaml
services:
  uptime-kuma:
    image: louislam/uptime-kuma:2
    container_name: uptime-kuma
    restart: unless-stopped
    ports:
      - "3001:3001"
    volumes:
      - ./data:/app/data
```

## 第三步：检查 compose 配置有没有写错

```bash
docker compose config
```

如果没有报错，说明配置格式基本没问题。

## 第四步：启动容器

```bash
docker compose up -d
```

## 第五步：确认容器是否真的启动成功

```bash
docker ps
```

你应该能看到一个名字叫 `uptime-kuma` 的容器。

## 第六步：打开网页

浏览器访问：

```text
http://服务器IP:3001
```

如果你做了反向代理，也可以直接访问你自己的域名：

```text
https://status.your-domain.com
```

## 第七步：第一次进入后你要做什么

第一次打开页面后，通常要完成初始化设置，例如：

- 创建管理员账号
- 设置密码
- 创建第一个监控项目
- 如果需要，创建状态页

## 数据保存在哪里

- 容器内目录：`/app/data`
- 宿主机目录：`/opt/docker/uptime-kuma/data`

这个目录里通常会保存：

- 监控配置
- 用户信息
- 状态页配置
- 数据库文件

## 如何备份

建议直接备份宿主机数据目录：

```bash
tar -czvf uptime-kuma-backup.tar.gz /opt/docker/uptime-kuma/data
```

## 如何更新

更新前建议先备份数据目录，然后执行：

```bash
cd /opt/docker/uptime-kuma
docker compose pull
docker compose up -d
```

## 如何查看日志

```bash
docker logs -f uptime-kuma
```

## 如何停止和重启

```bash
docker compose stop
docker compose start
docker compose restart
```

## 如果你想接入域名和 HTTPS

推荐配合 `Nginx Proxy Manager` 使用。
你可以把：

- 域名 `status.your-domain.com`
- 转发到 `服务器IP:3001`

然后再申请 HTTPS 证书。

## 常见问题

### 1. 3001 端口打不开

请先检查：

- 服务器防火墙是否放行 `3001`
- 云服务器安全组是否放行 `3001`
- 容器是否真的启动成功

### 2. 页面打不开，但容器在运行

先看日志：

```bash
docker logs -f uptime-kuma
```

### 3. 更新后页面异常

如果是配置问题，不要直接删除 `data` 目录，因为里面有你的监控数据。

### 4. 我想迁移到新服务器怎么办

最核心的是复制这两个内容：

- `compose.yml`
- `data` 数据目录

然后在新服务器里重新执行：

```bash
docker compose up -d
```
