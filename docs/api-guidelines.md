# API 规范

## 该不该先定 API 规范

该做。

## 为什么

CampTask 有 4 个前端和 3 个后端服务。先统一路径、响应、分页、错误码和权限约定，可以减少前后端联调成本。

## 基础约定

### API 前缀

```text
/api/v1
```

### 命名

- 路径使用复数资源名。
- JSON 字段使用 `camelCase`。
- 状态值使用小写下划线。
- 时间字段使用 ISO 8601 字符串。

### 响应格式

成功：

```json
{
  "code": "OK",
  "message": "success",
  "data": {}
}
```

失败：

```json
{
  "code": "VALIDATION_ERROR",
  "message": "参数错误",
  "details": []
}
```

### 分页格式

请求参数：

```text
page=1&pageSize=20
```

响应数据：

```json
{
  "page": 1,
  "pageSize": 20,
  "total": 100,
  "items": []
}
```

## 错误码

| 错误码 | 含义 |
| --- | --- |
| `OK` | 成功 |
| `VALIDATION_ERROR` | 参数错误 |
| `UNAUTHORIZED` | 未登录 |
| `FORBIDDEN` | 无权限 |
| `NOT_FOUND` | 资源不存在 |
| `CONFLICT` | 状态冲突或唯一约束冲突 |
| `BUSINESS_RULE_DENIED` | 业务规则不允许 |
| `INTERNAL_ERROR` | 服务内部错误 |

## 身份接口

所属服务：`identity-service`

```text
GET /api/v1/me
POST /api/v1/auth/login
POST /api/v1/auth/logout
```

## 学员端接口

所属服务：`camp-service`

```text
GET  /api/v1/student/assignments
GET  /api/v1/student/assignments/{assignmentId}
POST /api/v1/student/assignments/{assignmentId}/submissions
GET  /api/v1/student/submissions/{submissionId}
GET  /api/v1/student/submissions/{submissionId}/feedback
POST /api/v1/student/submissions/{submissionId}/resubmit
```

## 助教端接口

所属服务：`camp-service`

```text
GET  /api/v1/assistant/review-tasks
GET  /api/v1/assistant/submission-versions/{versionId}
POST /api/v1/assistant/submission-versions/{versionId}/reviews
POST /api/v1/assistant/submission-versions/{versionId}/return
```

说明：

- `reviews` 用于通过并评分。
- `return` 用于退回修改。

## 讲师端接口

所属服务：`camp-service`

```text
POST /api/v1/instructor/assignments
GET  /api/v1/instructor/assignments
GET  /api/v1/instructor/classes/{classId}/assignment-stats
GET  /api/v1/instructor/assignments/{assignmentId}/stats
```

## 管理后台接口

所属服务：`camp-service`

```text
GET  /api/v1/admin/courses
POST /api/v1/admin/courses
GET  /api/v1/admin/classes
POST /api/v1/admin/classes
POST /api/v1/admin/users/{userId}/roles
GET  /api/v1/admin/time-rules
POST /api/v1/admin/time-rules
GET  /api/v1/admin/score-rules
POST /api/v1/admin/score-rules
```

## 通知接口

所属服务：`notification-service`

```text
GET  /api/v1/notifications
POST /api/v1/notifications/{notificationId}/read
```

第一版通知可以先做站内通知或通知日志。

## 状态冲突处理

以下场景返回 `CONFLICT` 或 `BUSINESS_RULE_DENIED`：

- 作业未发布，学员提交。
- 作业已归档，继续提交。
- 提交已通过，继续重提。
- 助教批改非自己范围内提交。
- 讲师查看非负责班级统计。

## 暂不做

- ⚠️ 技术过度设计：不做 GraphQL。
- ⚠️ 技术过度设计：不做多版本 API 并行。
- ⚠️ 时间黑洞：不在第一版设计复杂批量导出接口。
