# 搜索与导航

## 服务列表

### 1. SearXNG

- **项目地址**: https://github.com/searxng/searxng
- **定位**: 开源聚合搜索引擎
- **适用场景**: 自建搜索入口、隐私友好搜索
- **Docker Compose**:
  ```yaml
  services:
    searxng:
      image: searxng/searxng:latest
      container_name: searxng
      restart: unless-stopped
      ports:
        - "8080:8080"
      volumes:
        - ./data/searxng:/etc/searxng
      environment:
        - SEARXNG_BASE_URL=https://search.your-domain.com/
    redis:
      image: redis:7-alpine
      container_name: searxng-redis
      restart: unless-stopped
      command: redis-server --save "" --appendonly "no"
  ```
- **访问端口**: `8080`
- **绑定域名**: `search.your-domain.com`
- **数据目录**: `./data/searxng -> /etc/searxng`

### 2. pansou-web

- **项目地址**: https://github.com/fish2018/pansou-web
- **定位**: 网盘资源搜索前端
- **适用场景**: 资源导航、网盘检索
- **Docker Compose**:
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
        - ./data/pansou/data:/app/data
        - ./data/pansou/logs:/app/logs
  ```
- **访问端口**: `80`
- **绑定域名**: `pan.your-domain.com`
- **数据目录**: `./data/pansou/data -> /app/data`，`./data/pansou/logs -> /app/logs`
