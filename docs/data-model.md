# 数据模型草案

## 该不该现在细化到字段

该做草案，不做最终迁移。

## 为什么

作业提交、退回、重提、批改统计都依赖数据模型。先把核心实体和关系定住，可以减少后续状态和权限返工。

## 核心实体

### 用户与权限

#### `users`

用户账号。

核心字段：

- `id`
- `name`
- `email`
- `phone`
- `password_hash`
- `status`
- `created_at`
- `updated_at`

#### `roles`

系统角色。

核心字段：

- `id`
- `code`
- `name`

角色代码：

- `STUDENT`
- `ASSISTANT`
- `INSTRUCTOR`
- `ADMIN`

#### `user_roles`

用户与角色关系。

核心字段：

- `id`
- `user_id`
- `role_id`
- `created_at`

### 课程与班级

#### `courses`

课程。

核心字段：

- `id`
- `name`
- `description`
- `status`
- `created_by`
- `created_at`
- `updated_at`

#### `classes`

班级。

核心字段：

- `id`
- `course_id`
- `name`
- `start_at`
- `end_at`
- `status`
- `created_at`
- `updated_at`

#### `class_members`

班级成员。

核心字段：

- `id`
- `class_id`
- `user_id`
- `role_code`
- `created_at`

说明：

- 学员、助教、讲师和班级的关系都可以先放这里。
- `role_code` 用于描述用户在该班级内的身份。

### 作业与规则

#### `assignments`

作业。

核心字段：

- `id`
- `course_id`
- `class_id`
- `title`
- `description`
- `status`
- `publish_at`
- `deadline_at`
- `created_by`
- `created_at`
- `updated_at`

作业状态：

- `draft`
- `published`
- `closed`
- `archived`

#### `assignment_rules`

作业规则。

核心字段：

- `id`
- `assignment_id`
- `allow_resubmit`
- `max_resubmit_count`
- `late_submit_allowed`
- `late_penalty_policy`
- `score_max`
- `created_at`
- `updated_at`

说明：

- 第一版只做简单字段，不做规则引擎。
- 复杂评分规则后置。

### 提交与批改

#### `submissions`

提交主记录。

核心字段：

- `id`
- `assignment_id`
- `student_id`
- `status`
- `current_version_id`
- `created_at`
- `updated_at`

提交状态：

- `not_submitted`
- `submitted`
- `reviewing`
- `returned`
- `accepted`

#### `submission_versions`

提交版本。

核心字段：

- `id`
- `submission_id`
- `version_no`
- `content_text`
- `content_link`
- `file_refs`
- `submitted_at`
- `is_late`

说明：

- 每次提交或重提都新增版本。
- 重提不创建新的 `assignment`。
- 批改反馈绑定到具体提交版本。

#### `reviews`

批改记录。

核心字段：

- `id`
- `submission_id`
- `submission_version_id`
- `reviewer_id`
- `score`
- `result`
- `comment`
- `reviewed_at`
- `created_at`

批改结论：

- `approved`
- `returned`

#### `review_comments`

批改评论。

核心字段：

- `id`
- `review_id`
- `user_id`
- `content`
- `created_at`

说明：

- 第一版如果只需要一条总评，可以先只用 `reviews.comment`。
- 行内评论、附件评论后置。

### 通知

#### `notifications`

通知记录。

核心字段：

- `id`
- `receiver_id`
- `type`
- `title`
- `content`
- `read_at`
- `created_at`

#### `notification_templates`

通知模板。

核心字段：

- `id`
- `code`
- `title_template`
- `content_template`
- `enabled`
- `created_at`
- `updated_at`

## 统计策略

第一版不单独建统计服务表。

统计默认从以下数据实时计算：

- `assignments`
- `submissions`
- `submission_versions`
- `reviews`
- `class_members`

必要时再增加汇总表。

## 关键约束

- `submissions` 对 `assignment_id + student_id` 建唯一约束。
- `submission_versions` 对 `submission_id + version_no` 建唯一约束。
- `reviews` 必须绑定 `submission_version_id`。
- 学员不能修改已经批改通过的提交版本。
- 退回后重提只新增提交版本。

## 暂不做

- ⚠️ 时间黑洞：不设计复杂评分规则表。
- ⚠️ 技术过度设计：不做事件溯源。
- ⚠️ 收益不匹配：不先建统计宽表。
