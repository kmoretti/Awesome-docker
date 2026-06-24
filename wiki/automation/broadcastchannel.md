# BroadcastChannel 部署与更新教程

## 这个项目是做什么的

BroadcastChannel 是一个消息广播和频道分发服务。
适合做消息同步、频道广播等场景。

## 这篇教程适合谁

- 想做消息广播的人
- 想同步频道内容的人
- 想部署一个轻量自动化服务的人

## 你需要准备什么

- 已安装 Docker 的服务器
- 建议目录：`/opt/docker/broadcastchannel`
- 端口 `4321`
- 可选域名：`channel.your-domain.com`

## 部署前先检查

```bash
docker -v
docker compose version
```

## 建议目录结构

```bash
/opt/docker/broadcastchannel/
└── compose.yml
```

## 第一步：创建项目目录

```bash
mkdir -p /opt/docker/broadcastchannel
cd /opt/docker/broadcastchannel
```

## 第二步：创建 compose 文件

```yaml
services:
  broadcastchannel:
    image: ghcr.io/miantiao-me/broadcastchannel:main
    container_name: broadcastchannel
    restart: unless-stopped
    ports:
      - "4321:4321"
    environment:
      - CHANNEL=your_channel_name
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
http://服务器IP:4321
```

如果做了反向代理，也可以绑定域名。

## 数据目录

这个项目通常通过环境变量控制运行，不一定需要本地持久化目录。

## 如何更新

```bash
cd /opt/docker/broadcastchannel
docker compose pull
docker compose up -d
```

## 如何查看日志

```bash
docker logs -f broadcastchannel
```

## 常见问题

### 1. 页面打不开

先确认 `4321` 端口是否已放行。

### 2. 功能不生效

请重点检查 `CHANNEL` 等环境变量是否填写正确。
