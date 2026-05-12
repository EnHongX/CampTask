# CampTask

CampTask 是一个面向训练营和课程作业场景的多角色作业协同平台。

项目重点不是简单“交作业”，而是把作业发布、提交、批改、退回修改、成绩统计、课程管理串成一条可运营的教学流程。

## 核心流程

讲师发布任务 -> 学员提交 -> 助教批改/退回 -> 学员重提 -> 讲师查看统计 -> 管理员维护课程配置

## 角色

- 学员：查看任务、提交作业、查看反馈、重提。
- 助教：批改、评论、退回、评分、查看待处理任务。
- 讲师：发布作业、查看班级完成情况、查看成绩与统计。
- 管理员：维护课程、班级、角色、时间规则、评分配置。

## 端入口

第一阶段只保留 4 个端，不再按功能继续拆端。

- `apps/student-web`：学员端。
- `apps/assistant-web`：助教端。
- `apps/instructor-web`：讲师端。
- `apps/admin-web`：管理后台。

管理类能力统一进入 `admin-web`，统计查看统一进入 `instructor-web`，不单独做课程端、统计端、配置端。

## 后端服务

服务端口从 `5301` 开始递增。

- `services/identity-service`：身份与权限服务，端口 `5301`。
- `services/camp-service`：训练营业务服务，端口 `5302`。
- `services/notification-service`：通知提醒服务，端口 `5303`。

课程、班级、作业、提交、批改、规则、统计先作为 `camp-service` 内部模块，不在第一阶段拆成多个独立服务。

## 当前阶段

当前只整理项目结构、文件结构和文档边界，不实现具体业务功能。

## 文档

- [产品范围](docs/product-scope.md)
- [技术栈选择](docs/tech-stack.md)
- [开发计划](docs/development-plan.md)
- [数据模型草案](docs/data-model.md)
- [权限模型](docs/permission-model.md)
- [API 规范](docs/api-guidelines.md)
- [角色与权限](docs/roles.md)
- [端侧划分](docs/apps.md)
- [业务流程](docs/workflows.md)
- [服务划分](docs/services.md)
- [领域模块](docs/domain-modules.md)
- [架构检查](docs/architecture-review.md)
- [端口约定](docs/ports.md)
- [项目结构](docs/project-structure.md)
- [开发约定](docs/development.md)
- [本地启动方式](docs/local-development.md)
