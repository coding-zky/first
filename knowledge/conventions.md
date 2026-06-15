# 编码规范

## 后端 (Java / SpringBoot)

### 包结构
- `com.imooc.app.common` — 统一响应、异常、DTO、工具类
- `com.imooc.app.dao` — 实体类、MyBatis Mapper
- `com.imooc.app.service` — 业务接口与实现
- `com.imooc.app.api` — REST Controller、配置类、启动类

### 命名约定
- 包名全小写：`com.imooc.app.{module}`
- 类名大驼峰：`UserController`、`UserServiceImpl`
- 方法名小驼峰：`getUserById`、`findPage`
- 常量全大写：`MAX_PAGE_SIZE`
- 数据库表名小写下划线：`sys_user`
- REST 路径小写短横线：`/api/users`

### 代码风格
- 不使用 Lombok（兼容 Java 25+），手写 getter/setter
- DTO 使用 Jakarta Validation 注解校验
- Controller 统一返回 `Result<T>`，不直接返回业务对象
- Service 层抛出 `BusinessException`，不在 Controller 中 try-catch
- 异常统一由 `GlobalExceptionHandler` 处理

### 分层职责
- **api** — 接收请求、参数校验、调用 Service、返回 Result
- **service** — 业务逻辑、数据组装、事务管理
- **dao** — 数据库操作（MyBatis-Plus Mapper）
- **common** — 无业务依赖的公共代码

## 前端 (React / TypeScript)

### 目录结构
```
frontend/src/
├── api/         # HTTP 请求封装
├── components/  # 公共组件
├── pages/       # 页面组件
├── router/      # 路由配置
├── store/       # Zustand 全局状态
└── utils/       # 工具函数
```

### 命名约定
- 组件文件大驼峰：`UserListPage.tsx`
- 工具/API 文件小驼峰：`request.ts`、`user.ts`
- Hook 以 use 开头：`useAuth`、`useStore`
- CSS 类名短横线或 CSS Modules

### 编码约定
- 函数组件 + Hooks（不使用 Class 组件）
- 路由使用 `React.lazy` 懒加载
- API 层统一使用 Axios 封装实例
- 全局状态使用 Zustand（非 Redux）
- UI 组件优先使用 Ant Design 5
