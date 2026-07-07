# Karakeep 部署说明

服务器：182.254.171.85（Ubuntu 24.04 内网 10.1.0.3/22，电信出口）

## 部署模式

使用官方 docker-compose 镜像部署（不构建源码）。

```bash
# 1. 复制环境配置
cp .env.sample .env
# 编辑 .env，至少设：
#   DATA_DIR=/home/ubuntu/projects/karakeep/data
#   NEXTAUTH_SECRET=<openssl rand -base64 32>

# 2. 启动
cd docker
docker compose -f docker-compose.yml up -d

# 3. 验证
curl http://localhost:3000
```

## 服务清单

| 服务 | 端口 | 镜像 |
|------|------|------|
| web | 3000 | ghcr.io/karakeep-app/karakeep |
| chrome | 9222 | gcr.io/zenika-hub/alpine-chrome:124 |
| meilisearch | 7700 | getmeili/meilisearch:v1.41.0 |

## 数据卷

- `data` → `/home/ubuntu/projects/karakeep/data`
- `meilisearch` → `/home/ubuntu/projects/karakeep/meili_data`

## 网络注意事项

服务器到 GitHub Container Registry (ghcr.io) 需要走代理。
在 docker daemon 配置里加：
```json
{
  "proxies": {
    "http-proxy": "http://127.0.0.1:7890",
    "https-proxy": "http://127.0.0.1:7890",
    "no-proxy": "localhost,127.0.0.1,10.1.0.0/22"
  }
}
```
