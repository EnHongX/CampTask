# 本地启动方式

## 该不该用 Docker Compose

第一阶段先不用。

## 为什么

当前还没有业务代码和依赖服务，直接上 Docker Compose 会提前引入环境维护成本。先用本机依赖和分服务启动，更适合快速验证 MVP。

## 怎么做

### 本机依赖

- `Node.js`
- `pnpm`
- `PostgreSQL`

具体版本在技术栈最终确认后再固定。

### 后端服务

后端服务端口：

- `identity-service`：`5301`
- `camp-service`：`5302`
- `notification-service`：`5303`

后续实现后，每个服务在自己的目录内启动。

```bash
pnpm install
pnpm dev
```

### 前端应用

前端只保留 4 个端：

- `student-web`
- `assistant-web`
- `instructor-web`
- `admin-web`

后续实现后，每个端在自己的目录内启动。

```bash
pnpm install
pnpm dev
```

### 数据库

第一阶段使用本机 PostgreSQL。

环境变量命名后续统一，建议预留：

```text
DB_HOST
DB_PORT
DB_NAME
DB_USER
DB_PASSWORD
```

## 暂不做

- ⚠️ 技术过度设计：不写 `docker-compose.yml`。
- ⚠️ 技术过度设计：不引入容器编排。
- ⚠️ 收益不匹配：不为还没确定的依赖服务维护启动脚本。
