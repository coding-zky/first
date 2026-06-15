# 用户管理 API 规范

## 端点

| 方法 | 路径 | 说明 |
|------|------|------|
| GET | `/api/users` | 分页查询用户列表 |
| POST | `/api/users` | 新增用户 |
| PUT | `/api/users/{id}` | 编辑用户 |
| DELETE | `/api/users/{id}` | 删除用户 |

## 统一响应格式

```json
{
  "code": 200,
  "message": "success",
  "data": { ... }
}
```

## 接口详情

### GET /api/users — 分页查询

**请求参数（Query）：**

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| current | Integer | 否 | 当前页码，默认 1 |
| size | Integer | 否 | 每页条数，默认 10 |
| keyword | String | 否 | 按姓名关键词搜索 |

**响应 data：**

```json
{
  "records": [
    { "id": 1, "name": "张三", "email": "zhangsan@example.com", "age": 25 }
  ],
  "total": 5,
  "current": 1,
  "size": 10,
  "pages": 1
}
```

### POST /api/users — 新增用户

**请求体：**

```json
{
  "name": "张三",       // @NotBlank
  "email": "a@b.com",   // @NotBlank @Email
  "age": 25             // @NotNull
}
```

**错误响应：**

```json
{ "code": 400, "message": "姓名不能为空", "data": null }
```

### PUT /api/users/{id} — 编辑用户

请求体与新增相同。

### DELETE /api/users/{id} — 删除用户

返回：`{ "code": 200, "message": "success", "data": null }`

## 错误码

| code | 说明 |
|------|------|
| 200 | 成功 |
| 400 | 业务异常 / 参数校验失败 |
| 500 | 系统内部错误 |
