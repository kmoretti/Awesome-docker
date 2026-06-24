# 网站与内容服务

## 服务列表

### 1. 60s

- **项目地址**: https://github.com/vikiboss/60s
- **定位**: 每日资讯/60 秒读懂世界类内容服务
- **适用场景**: 内容展示站、信息聚合页
- **Docker Compose**:
  ```yaml
  services:
    news60s:
      image: vikiboss/60s:latest
      container_name: 60s
      restart: unless-stopped
      ports:
        - "4399:4399"
  ```
- **访问端口**: `4399`
- **绑定域名**: `60s.your-domain.com`
- **数据目录**: 无强制持久化目录，建议单独保留 `./data/60s` 用于后续扩展

### 2. CloudFlare-ImgBed

- **项目地址**: https://github.com/MarSeventh/CloudFlare-ImgBed
- **定位**: 基于 Cloudflare 的图床
- **适用场景**: 图片托管、静态资源外链
- **Docker Compose**:
  ```yaml
  services:
    imgbed:
      image: marseventh/cloudflare-imgbed:latest
      container_name: imgbed
      restart: unless-stopped
      ports:
        - "7658:8080"
      volumes:
        - ./wrangler.toml:/app/wrangler.toml
        - ./data/cloudflare-imgbed:/app/data
  ```
- **访问端口**: `7658`
- **绑定域名**: `img.your-domain.com`
- **数据目录**: `./data/cloudflare-imgbed -> /app/data`

### 3. Ech0

- **项目地址**: https://github.com/lin-snow/Ech0
- **定位**: 面向个人的自托管轻量发布平台
- **适用场景**: 博客、碎碎念、个人主页、联邦发布
- **Docker Compose**:
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
        - ./data/ech0/backup:/app/backup
        - ./data/ech0/data:/app/data
  ```
- **访问端口**: `6277`
- **绑定域名**: `echo.your-domain.com`
- **数据目录**: `./data/ech0/data -> /app/data`，`./data/ech0/backup -> /app/backup`

### 4. busuanzi

- **项目地址**: https://github.com/soxft/busuanzi
- **定位**: 网站访问统计组件
- **适用场景**: 页面访客统计、站点 PV/UV 展示
- **Docker Compose**:
  ```yaml
  services:
    busuanzi:
      image: soxft/busuanzi:latest
      container_name: busuanzi
      restart: unless-stopped
      ports:
        - "8080:8080"
      volumes:
        - ./config.yaml:/app/config.yaml
      depends_on:
        - redis
    redis:
      image: redis:7-alpine
      container_name: busuanzi-redis
      restart: unless-stopped
      volumes:
        - ./data/busuanzi/redis:/data
  ```
- **访问端口**: `8080`
- **绑定域名**: `stats.your-domain.com`
- **数据目录**: `./data/busuanzi/redis -> /data`，配置文件建议单独保存为 `./config.yaml`

### 5. Meting-API

- **项目地址**: https://github.com/metowolf/Meting-API
- **定位**: 音乐 API 服务
- **适用场景**: 音乐播放器接口、站点背景音乐功能
- **Docker Compose**:
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
- **访问端口**: `80`
- **绑定域名**: `music-api.your-domain.com`
- **数据目录**: 无必须持久化目录，建议用环境变量管理配置
