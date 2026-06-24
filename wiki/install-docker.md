# Docker 安装教程

这篇文档适合第一次接触 Docker 的新手。

## 一、先确认你的服务器系统

先登录服务器，执行：

```bash
uname -a
cat /etc/os-release
```

常见系统一般是：

- Ubuntu
- Debian
- CentOS
- Rocky Linux

## 二、最简单的安装思路

如果你是新手，建议优先使用官方文档提供的安装方式。

安装完成后，至少要能执行这两个命令：

```bash
docker -v
docker compose version
```

## 三、Ubuntu / Debian 常见安装方式

### 1. 更新软件包列表

```bash
sudo apt update
```

### 2. 安装基础依赖

```bash
sudo apt install -y ca-certificates curl gnupg
```

### 3. 安装 Docker

很多时候直接用系统源也能安装，但推荐尽量按 Docker 官方方式安装。

安装后检查：

```bash
docker -v
```

### 4. 检查 Docker Compose

```bash
docker compose version
```

## 四、CentOS / Rocky Linux 常见安装思路

一般思路也是：

1. 安装必要依赖
2. 配置 Docker 官方源
3. 安装 Docker Engine
4. 启动 Docker 服务
5. 检查 `docker -v`

## 五、启动 Docker 服务

```bash
sudo systemctl enable docker
sudo systemctl start docker
sudo systemctl status docker
```

## 六、测试 Docker 是否正常工作

```bash
docker run hello-world
```

如果能正常输出欢迎信息，就说明 Docker 已可用。

## 七、为什么推荐使用 Docker Compose

因为后面你部署的大多数项目，都会用到：

```bash
docker compose up -d
```

这会比手写很长一串 `docker run` 命令更容易维护。

## 八、建议你安装后立刻做的事

- 确认 Docker 服务开机自启
- 确认服务器磁盘空间足够
- 确认 80 / 443 等常用端口没有冲突
- 先部署一个简单项目练手，比如 `Uptime Kuma`

## 九、常见问题

### 1. `docker: command not found`

说明 Docker 还没装好，或者环境变量还没生效。

### 2. `docker compose` 不可用

说明 Docker Compose 组件没有正确安装。

### 3. 拉镜像很慢

这通常和网络环境有关，可以后续再考虑镜像加速方案。
