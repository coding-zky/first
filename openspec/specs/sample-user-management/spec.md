## ADDED Requirements

### Requirement: 用户列表分页查询

系统 SHALL 提供 `/api/users` GET 接口，支持分页参数（`page`、`size`）和可选的关键词搜索参数（`keyword`），返回分页用户列表。

#### Scenario: 默认分页查询

- **WHEN** 前端请求 `GET /api/users?page=1&size=10`
- **THEN** 系统 SHALL 返回第一页的 10 条用户记录，包含 `total` 总数

#### Scenario: 关键词搜索

- **WHEN** 前端请求 `GET /api/users?page=1&size=10&keyword=张三`
- **THEN** 系统 SHALL 返回姓名包含"张三"的用户分页列表

#### Scenario: 空结果

- **WHEN** 查询关键词无匹配结果
- **THEN** 系统 SHALL 返回 `total` 为 `0`、`records` 为空数组的分页结果

---

### Requirement: 新增用户

系统 SHALL 提供 `/api/users` POST 接口，接收包含 `name`、`email`、`age` 的 JSON 请求体，校验通过后新增用户记录。

#### Scenario: 成功新增

- **WHEN** 前端 POST `{"name": "李四", "email": "lisi@example.com", "age": 25}` 到 `/api/users`
- **THEN** 系统 SHALL 返回 `code` 为 `200`，`data` 包含新增用户的 ID

#### Scenario: 邮箱重复

- **WHEN** 前端 POST 已存在的 `email` 地址
- **THEN** 系统 SHALL 返回 `code` 为 `400`，`message` 为 `"邮箱已被使用"`

#### Scenario: 参数校验失败

- **WHEN** 前端 POST 缺少必填字段 `name` 的请求体
- **THEN** 系统 SHALL 返回 `code` 为 `400`，`message` 包含字段校验错误信息

---

### Requirement: 编辑用户

系统 SHALL 提供 `/api/users/{id}` PUT 接口，接收包含更新字段的 JSON 请求体，更新指定 ID 的用户信息。

#### Scenario: 成功更新

- **WHEN** 前端 PUT `{"name": "李四修改"}` 到 `/api/users/1`
- **THEN** 用户 ID 为 1 的 `name` SHALL 更新为 `"李四修改"`

#### Scenario: 用户不存在

- **WHEN** 前端 PUT 请求到不存在的用户 ID `/api/users/999`
- **THEN** 系统 SHALL 返回 `code` 为 `400`，`message` 为 `"用户不存在"`

---

### Requirement: 删除用户

系统 SHALL 提供 `/api/users/{id}` DELETE 接口，删除指定 ID 的用户记录。

#### Scenario: 成功删除

- **WHEN** 前端 DELETE `/api/users/1`
- **THEN** 系统 SHALL 删除 ID 为 1 的用户，返回 `code` 为 `200`

#### Scenario: 删除不存在的用户

- **WHEN** 前端 DELETE `/api/users/999`
- **THEN** 系统 SHALL 返回 `code` 为 `400`，`message` 为 `"用户不存在"`

---

### Requirement: 前端用户管理页面

系统 SHALL 提供用户管理页面（路由 `/users`），使用 Ant Design `Table` 组件展示用户列表，支持分页、关键词搜索、新增、编辑、删除操作。

#### Scenario: 页面加载

- **WHEN** 用户访问 `/users` 路由
- **THEN** SHALL 加载并展示用户分页列表，包含序号、姓名、邮箱、年龄、操作列

#### Scenario: 搜索功能

- **WHEN** 用户在搜索框输入关键词并点击搜索
- **THEN** 列表 SHALL 重新加载，仅显示匹配的用户记录

#### Scenario: 新增用户弹窗

- **WHEN** 用户点击"新增"按钮
- **THEN** SHALL 弹出 Modal 表单，包含姓名、邮箱、年龄三个输入项

#### Scenario: 编辑用户弹窗

- **WHEN** 用户点击某行的"编辑"按钮
- **THEN** SHALL 弹出 Modal 表单，表单预填当前用户的姓名、邮箱、年龄

#### Scenario: 删除确认

- **WHEN** 用户点击某行的"删除"按钮
- **THEN** SHALL 弹出确认对话框，确认后执行删除操作并刷新列表
