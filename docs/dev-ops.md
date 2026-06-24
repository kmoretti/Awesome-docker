# 开发与运维

## 服务列表

### 1. Uptime Kuma

- **项目地址**: https://github.com/louislam/uptime-kuma
- **定位**: 开源服务监控与状态页
- **适用场景**: 监控网站、API、服务器与容器服务可用性
- **Docker Compose**:
  ```yaml
  services:
    uptime-kuma:
      image: louislam/uptime-kuma:2
      container_name: uptime-kuma
      restart: unless-stopped
      ports:
        - "3001:3001"
      volumes:
        - ./data/uptime-kuma:/app/data
  ```
- **访问端口**: `3001`
- **绑定域名**: `status.your-domain.com`
- **数据目录**: `./data/uptime-kuma -> /app/data`

### 2. Nginx Proxy Manager

- **项目地址**: https://github.com/NginxProxyManager/nginx-proxy-manager
- **定位**: 可视化反向代理与 SSL 证书管理
- **适用场景**: 域名转发、HTTPS 证书申请、统一入口管理
- **Docker Compose**:
  ```yaml
  services:
    nginx-proxy-manager:
      image: jc21/nginx-proxy-manager:latest
      container_name: nginx-proxy-manager
      restart: unless-stopped
      ports:
        - "80:80"
        - "81:81"
        - "443:443"
      volumes:
        - ./data/nginx-proxy-manager/data:/data
        - ./data/nginx-proxy-manager/letsencrypt:/etc/letsencrypt
  ```
- **访问端口**: `80` / `81` / `443`
- **绑定域名**: `npm.your-domain.com`
- **数据目录**: `./data/nginx-proxy-manager/data -> /data`，`./data/nginx-proxy-manager/letsencrypt -> /etc/letsencrypt`

### 3. ChronoFrame

- **项目地址**: https://github.com/HoshinoSuzumi/chronoframe
- **定位**: 自托管照片展示与管理应用
- **适用场景**: 相册、图片整理、地图探索、EXIF 展示
- **Docker Compose**:
  ```yaml
  services:
    chronoframe:
      image: ghcr.io/hoshinosuzumi/chronoframe:latest
      container_name: chronoframe
      restart: unless-stopped
      ports:
        - "3000:3000"
      env_file:
        - .env
      volumes:
        - ./data/chronoframe:/app/data
  ```
- **访问端口**: `3000`
- **绑定域名**: `photo.your-domain.com`
- **数据目录**: `./data/chronoframe -> /app/data`
