# API 响应模板

## 成功响应
```json
{
  "code": 200,
  "message": "success",
  "data": {
    // 业务数据
  }
}
```

## 列表分页响应
```json
{
  "code": 200,
  "message": "success",
  "data": {
    "records": [],
    "total": 0,
    "current": 1,
    "size": 10,
    "pages": 0
  }
}
```

## 业务异常响应
```json
{
  "code": 400,
  "message": "具体错误描述",
  "data": null
}
```

## 参数校验失败响应
```json
{
  "code": 400,
  "message": "字段A不能为空, 字段B格式不正确",
  "data": null
}
```

## 系统异常响应
```json
{
  "code": 500,
  "message": "系统内部错误",
  "data": null
}
```
