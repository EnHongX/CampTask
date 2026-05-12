# 项目结构

```text
CampTask/
  apps/
    student-web/              # 学员端
    assistant-web/            # 助教端
    instructor-web/           # 讲师端
    admin-web/                # 管理后台
  services/
    identity-service/         # 身份与权限服务，5301
    camp-service/             # 训练营业务服务，5302
    notification-service/     # 通知提醒服务，5303
  packages/
    shared-types/             # 跨端共享类型占位
    shared-utils/             # 跨端共享工具占位
  docs/                       # 产品和工程文档
  scripts/                    # 本地脚本占位
  deploy/                     # 部署配置占位
```

## 说明

- `apps` 只保留 4 个端入口，不按功能继续拆端。
- 学员提交、助教批改、讲师统计、后台配置分别归到对应端。
- `services` 表示第一阶段独立运行单元，不等同于所有领域能力。
- 课程、班级、作业、提交、批改、规则、统计先放在 `camp-service` 内部。
- `packages` 只放稳定共享内容，避免过早抽公共包。
- `docs` 记录当前确定的边界，后续实现前先更新文档。
