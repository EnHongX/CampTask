# 技术栈选择

## 该不该用 Node.js 做后端

可以。

## 为什么

CampTask 第一阶段是典型业务后台，核心风险在权限、状态流转和数据模型，不在高并发或复杂计算。使用 Node.js 可以让前后端统一 TypeScript 技术栈，减少类型和接口维护成本。

## 推荐方案

### 后端

- 运行时：`Node.js`
- 语言：`TypeScript`
- 框架：`NestJS`
- ORM：`Prisma`
- 数据库：`PostgreSQL`
- API 文档：`OpenAPI 3`
- 包管理：`pnpm`

### 前端

- 框架：`Vue 3`
- 语言：`TypeScript`
- 构建：`Vite`
- 路由：`Vue Router`
- 状态：`Pinia`
- UI：`Element Plus`
- 包管理：`pnpm`

## 为什么选 NestJS

- 适合多模块业务后台。
- 权限、拦截器、校验、OpenAPI 支持成熟。
- 目录结构清晰，适合后续把 `camp-service` 内部领域模块拆开。

## 为什么选 Prisma

- TypeScript 类型体验好。
- 数据模型和迁移集中管理。
- 适合 MVP 阶段快速建立 PostgreSQL 数据结构。

## 暂不引入

- ⚠️ 技术过度设计：不引入微服务框架。
- ⚠️ 技术过度设计：不引入消息队列。
- ⚠️ 技术过度设计：不引入 GraphQL。
- ⚠️ 收益不匹配：不为第一版引入复杂 monorepo 构建工具。
