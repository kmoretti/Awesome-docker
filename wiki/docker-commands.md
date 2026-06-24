# Docker 常用命令手册

这份文档用于记录常见 Docker 和 Docker Compose 命令，尽量用最容易理解的方式整理。

## 1. 查看 Docker 是否安装成功

```bash
docker -v
docker compose version
```

## 2. 查看正在运行的容器

```bash
docker ps
```

## 3. 查看所有容器

```bash
docker ps -a
```

## 4. 查看本机镜像

```bash
docker images
```

## 5. 启动当前目录下的 compose 项目

```bash
docker compose up -d
```

## 6. 停止当前目录下的 compose 项目

```bash
docker compose stop
```

## 7. 重启当前目录下的 compose 项目

```bash
docker compose restart
```

## 8. 拉取最新镜像

```bash
docker compose pull
```

## 9. 更新项目

```bash
docker compose pull
docker compose up -d
```

## 10. 查看容器日志

```bash
docker logs -f 容器名
```

例如：

```bash
docker logs -f uptime-kuma
```

## 11. 进入容器内部

```bash
docker exec -it 容器名 sh
```

如果容器支持 bash，也可以：

```bash
docker exec -it 容器名 bash
```

## 12. 删除停止状态的容器

```bash
docker container prune
```

## 13. 删除无用镜像

```bash
docker image prune -a
```

## 14. 查看容器资源占用

```bash
docker stats
```

## 15. 查看某个容器的详细信息

```bash
docker inspect 容器名
```

## 16. 手动启动单个容器

```bash
docker start 容器名
```

## 17. 手动停止单个容器

```bash
docker stop 容器名
```

## 18. 手动重启单个容器

```bash
docker restart 容器名
```

## 19. 删除单个容器

```bash
docker rm 容器名
```

## 20. 删除单个镜像

```bash
docker rmi 镜像名
```

## 21. 查看 compose 配置是否正确

```bash
docker compose config
```

## 22. 查看 compose 项目中的服务

```bash
docker compose ps
```

## 23. 停止并删除当前 compose 项目容器

```bash
docker compose down
```

## 24. 停止并删除容器、网络、匿名卷

```bash
docker compose down -v
```

## 25. 常见更新流程

```bash
cd /opt/docker/项目名
docker compose pull
docker compose up -d
docker image prune -a
```

## 26. 备份数据的基本思路

备份前先找到容器的数据目录，然后直接备份宿主机挂载目录。

例如：

```bash
tar -czvf uptime-kuma-backup.tar.gz /opt/docker/uptime-kuma/data
```

## 27. 查看端口占用

Linux 常用：

```bash
ss -tlnp
```

或者：

```bash
netstat -tlnp
```

## 28. 常见排错顺序

建议按这个顺序排查：

1. 看容器是否启动成功
2. 看日志是否报错
3. 看端口是否监听
4. 看防火墙是否放行
5. 看反向代理是否正确
6. 看域名解析是否正常
