# OpsDock

在 Linux 上快速启动一组常用基础服务（仅 Docker 部署）。

## 目录

- [服务列表](#服务列表)
- [启动](#启动)
- [常用命令](#常用命令)
- [配置说明](#配置说明)
- [项目结构](#项目结构)

---

## 服务列表

| 服务 | 端口 | 说明 |
|------|------|------|
| PostgreSQL (pgvector, PG16) | 5432 | 数据库，启用 `vector` 扩展 |
| Redis | 6379 | 缓存服务（启用密码） |
| NATS | 4222 / 8222 | 消息队列（启用 JetStream 持久化） |
| MinIO | 9000 / 9001 | S3 兼容的分布式对象存储服务 |
| Nginx | 80 | 反向代理（前端页面与API网关转发） |

---

## 启动

### 1. 准备环境变量

```bash
cp .env.example .env
```

### 2. 启动服务

```bash
./init.sh
# 或者手动执行
docker compose up -d
```

也可以直接运行初始化脚本（会自动选择 `docker compose` 或 `docker-compose`）：

```bash
./init.sh
```

### 3. 验证与访问

```bash
docker compose ps
```

- NATS：`nats://localhost:${NATS_CLIENT_PORT:-4222}`
- NATS 监控：`http://localhost:${NATS_MONITOR_PORT:-8222}`

---

## 常用命令

```bash
docker compose up -d
docker compose down
docker compose restart
docker compose logs -f
docker compose ps
docker compose down -v  # 清理并删除数据卷
```

---

## 配置说明

编辑 `.env`：

```bash
POSTGRES_USER=zephyr
POSTGRES_PASSWORD=postgres
POSTGRES_DB=zephyrdb
DATA_DIR=./data
NGINX_HTTP_PORT=80
REDIS_PASSWORD=change_me
NATS_CLIENT_PORT=4222
NATS_MONITOR_PORT=8222
MINIO_ROOT_USER=admin
MINIO_ROOT_PASSWORD=password
MINIO_API_PORT=9000
MINIO_CONSOLE_PORT=9001
```

修改后执行：

```bash
docker compose up -d
```

---

## 项目结构

```text
opsdock/
├── docker-compose.yml
├── .env.example
├── init.sh
├── README.md
├── pgsql/
│   └── 01-extensions.sql
└── nginx/
    └── conf.d/
        └── default.conf
```
