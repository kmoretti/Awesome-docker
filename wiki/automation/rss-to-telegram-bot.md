# RSS-to-Telegram-Bot 部署与更新教程

## 这个项目是做什么的

RSS-to-Telegram-Bot 是一个把 RSS 内容自动转发到 Telegram 的机器人。

## 这篇教程适合谁

- 想把 RSS 自动推送到 Telegram 的人
- 想做频道自动通知的人
- 想学习无 Web 页面项目的 Docker 部署方式的人

## 你需要准备什么

- 已安装 Docker 的服务器
- 一个 Telegram Bot Token
- 你的 Telegram 管理员账号 ID
- 建议目录：`/opt/docker/rss-to-telegram-bot`

## 部署前先检查

```bash
docker -v
docker compose version
```

## 建议目录结构

```bash
/opt/docker/rss-to-telegram-bot/
├── compose.yml
└── data/
    └── config/
```

## 第一步：创建项目目录

```bash
mkdir -p /opt/docker/rss-to-telegram-bot/data/config
cd /opt/docker/rss-to-telegram-bot
```

## 第二步：创建 compose 文件

```yaml
services:
  rsstt:
    image: rongronggg9/rss-to-telegram:dev
    container_name: rsstt
    restart: unless-stopped
    volumes:
      - ./data/config:/app/config
    environment:
      - TOKEN=1234567890:your-bot-token
      - MANAGER=123456789
      - TELEGRAPH_TOKEN=your-telegraph-token
```

## 第三步：检查配置

```bash
docker compose config
```

## 第四步：启动

```bash
docker compose up -d
```

## 第五步：确认是否运行正常

```bash
docker ps
docker logs -f rsstt
```

## 数据保存在哪里

- 配置目录：`/opt/docker/rss-to-telegram-bot/data/config`

## 如何备份

```bash
tar -czvf rsstt-backup.tar.gz /opt/docker/rss-to-telegram-bot/data/config
```

## 如何更新

```bash
cd /opt/docker/rss-to-telegram-bot
docker compose pull
docker compose up -d
```

## 注意事项

这个项目通常不需要对外开放网页端口。
主要依赖 Bot Token 和配置文件正常工作。

## 常见问题

### 1. 机器人没有推送消息

请检查：

- Token 是否正确
- 管理员 ID 是否正确
- RSS 源是否可访问
- Telegram 相关配置是否正常

### 2. 容器一直重启

通常说明环境变量不完整，或者配置文件有问题。
请先看日志再排查。
