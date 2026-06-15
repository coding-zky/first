# Sprint 1 — 全栈脚手架搭建

> 状态: 已完成

## 目标

搭建 SpringBoot + React 全栈开发脚手架，包含后端四层架构、前端 SPA 工程、用户管理 CRUD 示例。

## 任务清单

### 1. 后端 Maven 工程骨架 ✅
- 父 POM + common/dao/service/api 四模块
- `mvn compile` 全模块编译通过

### 2. 统一响应与异常处理 ✅
- `Result<T>` + `BusinessException` + `GlobalExceptionHandler`

### 3. CORS 与参数校验 ✅
- `CorsConfig` + Jakarta Bean Validation + `@Valid`

### 4. 数据访问层 ✅
- MyBatis-Plus 分页插件 + H2/MySQL 双 Profile + schema.sql/data.sql

### 5. Swagger 文档与日志 ✅
- Knife4j `/doc.html` + Logback 日志配置

### 6. 用户管理后端 CRUD ✅
- User 实体 + UserMapper + UserService + UserController

### 7. 前端工程骨架 ✅
- Vite + React 18 + TypeScript + React Router + Axios + Ant Design 5 + Zustand

### 8. 前端基础设施 ✅
- 路由懒加载 + Axios 封装 + Ant Design 中文 + Zustand Store

### 9. 用户管理前端 CRUD ✅
- 用户列表 Table + 搜索 + 新增/编辑 Modal + 删除确认

### 10. 收尾 ✅
- README 更新 + 全栈联调验证 + 清理占位文件
