# CloudFlare ImgBed 部署与更新教程

## 这个项目是做什么的

CloudFlare ImgBed 是一个图床项目，适合用来上传图片并生成外链。

## 这篇教程适合谁

- 想搭建自己的图床的人
- 想给博客或站点提供图片外链的人
- 想学习带平台配置文件项目部署的人

## 你需要准备什么

- 已安装 Docker 的服务器
- Cloudflare 相关配置
- 建议目录：`/opt/docker/cloudflare-imgbed`
- 可选域名：`img.your-domain.com`

## 部署前先检查

```bash
docker -v
docker compose version
```

## 建议目录结构

```bash
/opt/docker/cloudflare-imgbed/
├── compose.yml
├── wrangler.toml
└── data/
```

## 第一步：创建项目目录

```bash
mkdir -p /opt/docker/cloudflare-imgbed/data
cd /opt/docker/cloudflare-imgbed
```

## 第二步：创建 compose 文件

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
      - ./data:/app/data
```

## 第三步：准备 `wrangler.toml`

把项目要求的 Cloudflare 相关配置写进去。
如果这里配置不对，图床通常无法正常工作。

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
http://服务器IP:7658
```

## 数据目录

- `/opt/docker/cloudflare-imgbed/data`

## 如何备份

```bash
tar -czvf cloudflare-imgbed-backup.tar.gz /opt/docker/cloudflare-imgbed/data
```

如果 `wrangler.toml` 很重要，也建议一起备份。

## 更新

```bash
cd /opt/docker/cloudflare-imgbed
docker compose pull
docker compose up -d
```

## 常见问题

### 1. 上传失败

通常是 Cloudflare 配置、权限或 `wrangler.toml` 参数不正确。

### 2. 页面能打开但图片上传失败

请重点检查：

- Cloudflare 账号配置
- Token 权限
- 存储绑定
- 配置文件是否写错
