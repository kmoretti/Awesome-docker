# 问题记录

这个文件用于记录我在 Docker 部署过程中遇到的问题，以及最终解决方式，方便以后排查和复用。

---

## 1. Cloudflare ImgBed 使用 Telegram 作为存储源时连接失败

### 现象

在部署 `Cloudflare ImgBed` 时，添加 `Telegram` 作为存储源之一，连接失败。

### 同类问题

`Uptime Kuma` 添加 `Telegram` 通知时，也出现连接失败。

### 原因判断

服务器访问 `api.telegram.org` 存在问题，可能是 DNS 解析异常，或者默认解析结果不可用。

### 解决方式

修改服务器的 `hosts` 文件，手动添加下面内容：

```text
149.154.167.220 api.telegram.org
149.154.167.197 api.telegram.org
```

### 说明

添加后，`Cloudflare ImgBed` 使用 Telegram 存储源恢复正常，`Uptime Kuma` 的 Telegram 通知也可正常连接。

### 补充建议

如果后续 Telegram 再次无法连接，可以优先检查：

- `hosts` 是否还在
- 服务器是否能访问 Telegram
- DNS 解析是否异常

---

## 2. 拉取不到 Docker 镜像

### 现象

执行 `docker pull` 或 `docker compose up -d` 时，出现镜像拉取失败。

### 原因判断

大概率是默认 Docker 镜像源访问不稳定，或者网络环境导致拉取失败。

### 解决方式

给 Docker 配置 `daemon.json`，添加镜像加速配置。

常见路径：

```text
/etc/docker/daemon.json
```

### 处理思路

1. 新建或修改 `daemon.json`
2. 配置可用的镜像加速地址
3. 重启 Docker 服务

### 常见操作步骤

```bash
sudo mkdir -p /etc/docker
sudo nano /etc/docker/daemon.json
```

示例内容可以按你自己的可用镜像源填写，例如：

```json
{
  "registry-mirrors": [
    "https://你的镜像加速地址"
  ]
}
```

保存后执行：

```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```

然后再次尝试：

```bash
docker pull 镜像名
```

### 补充建议

如果还是拉取失败，可以继续检查：

- 镜像源地址是否可用
- 服务器网络是否正常
- Docker 服务是否已经重启成功

---

## 后续可继续补充

以后还可以继续在这里记录：

- 端口冲突问题
- 域名解析问题
- HTTPS 证书申请失败
- 容器重启循环
- 数据目录权限问题
