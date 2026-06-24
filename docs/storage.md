# 文件与存储

## 服务列表

### 1. OpenList

- **项目地址**: https://github.com/OpenListTeam/OpenList
- **定位**: 文件列表与网盘聚合管理
- **适用场景**: 多存储挂载、文件管理、目录分享
- **Docker Compose**:
  ```yaml
  services:
    openlist:
      image: openlistteam/openlist:latest
      container_name: openlist
      restart: always
      ports:
        - "5244:5244"
        - "5245:5245"
      environment:
        - UMASK=022
        - TZ=Asia/Shanghai
      volumes:
        - ./data/openlist:/opt/openlist/data
  ```
- **访问端口**: `5244`（Web） / `5245`（附加服务端口）
- **绑定域名**: `list.your-domain.com`
- **数据目录**: `./data/openlist -> /opt/openlist/data`
