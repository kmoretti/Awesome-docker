# 消息与自动化

## 服务列表

### 1. RSS-to-Telegram-Bot

- **项目地址**: https://github.com/Rongronggg9/RSS-to-Telegram-Bot
- **定位**: RSS 转 Telegram 推送机器人
- **适用场景**: 订阅消息转发、自动通知
- **Docker Compose**:
  ```yaml
  services:
    rsstt:
      image: rongronggg9/rss-to-telegram:dev
      container_name: rsstt
      restart: unless-stopped
      volumes:
        - ./data/rsstt/config:/app/config
      environment:
        - TOKEN=1234567890:your-bot-token
        - MANAGER=123456789
        - TELEGRAPH_TOKEN=your-telegraph-token
  ```
- **访问端口**: 无需对外开放固定 Web 端口
- **绑定域名**: 无
- **数据目录**: `./data/rsstt/config -> /app/config`

### 2. BroadcastChannel

- **项目地址**: https://github.com/miantiao-me/BroadcastChannel
- **定位**: 频道广播/消息分发服务
- **适用场景**: 消息聚合广播、频道内容同步
- **Docker Compose**:
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
- **访问端口**: `4321`
- **绑定域名**: `channel.your-domain.com`
- **数据目录**: 无强制持久化目录，主要依赖环境变量配置
