## 1. 后端 Maven 工程骨架搭建

- [x] 1.1 创建父 POM (`pom.xml`)，配置 SpringBoot 3.x、MyBatis-Plus、SpringDoc OpenAPI、H2、MySQL 等依赖版本
- [x] 1.2 创建 `common` 子模块，配置基础依赖
- [x] 1.3 创建 `dao` 子模块，依赖 `common`，配置 MyBatis-Plus 依赖
- [x] 1.4 创建 `service` 子模块，依赖 `dao`
- [x] 1.5 创建 `api` 子模块，依赖 `service`，配置 SpringBoot 主启动类和 Web 依赖
- [x] 1.6 验证 `mvn compile` 全模块编译通过

## 2. 后端基础设施 — 统一响应与异常处理

- [x] 2.1 在 `common` 模块创建 `Result<T>` 统一响应类，包含 `code`、`message`、`data` 字段及静态工厂方法
- [x] 2.2 在 `common` 模块创建 `BusinessException` 业务异常类
- [x] 2.3 在 `api` 模块创建 `GlobalExceptionHandler` 全局异常处理器，捕获 `BusinessException` 和 `Exception`
- [x] 2.4 验证异常处理：启动应用，访问不存在的接口返回统一 `Result` 格式

## 3. 后端基础设施 — CORS 与参数校验

- [x] 3.1 在 `api` 模块创建 `CorsConfig` 全局跨域配置类，允许 `localhost:5173` 来源
- [x] 3.2 在 `api` 模块启用 Jakarta Bean Validation，配置 `@Valid` 校验失败时的统一错误响应
- [x] 3.3 验证 CORS 配置：从 `localhost:5173` 发起跨域请求能正常响应

## 4. 数据访问层 — MyBatis-Plus 与数据库配置

- [x] 4.1 创建 `application.yml` 默认配置文件（H2 数据源、H2 Console 启用）
- [x] 4.2 创建 `application-prod.yml` 生产环境配置文件（MySQL 数据源）
- [x] 4.3 在 `dao` 模块配置 MyBatis-Plus 分页插件
- [x] 4.4 创建 `schema.sql` 和 `data.sql` 初始化建表和示例数据
- [x] 4.5 验证 H2 启动：以 dev Profile 启动，能通过 `/h2-console` 访问数据库

## 5. 后端 — Swagger 文档与日志

- [x] 5.1 在 `api` 模块配置 SpringDoc OpenAPI（Knife4j），Swagger UI 路径 `/doc.html`
- [x] 5.2 创建 `logback-spring.xml` 日志配置，dev 环境输出 MyBatis SQL 日志
- [x] 5.3 验证：启动后访问 `/doc.html` 能看到 Swagger UI 页面

## 6. 用户管理 — 后端 CRUD 示例

- [x] 6.1 在 `dao` 模块创建 `User` 实体类和 `users` 数据表初始化 SQL
- [x] 6.2 在 `dao` 模块创建 `UserMapper` 接口，继承 `BaseMapper<User>`
- [x] 6.3 在 `common` 模块创建 `UserDTO` 请求/响应 DTO 类
- [x] 6.4 在 `service` 模块创建 `UserService`，实现分页查询、新增、更新、删除业务逻辑
- [x] 6.5 在 `api` 模块创建 `UserController`，暴露 `/api/users` REST 接口
- [x] 6.6 验证：通过 Swagger UI 或 curl 测试用户 CRUD 接口

## 7. 前端工程骨架搭建

- [x] 7.1 使用 Vite 创建 React + TypeScript 前端工程（`frontend/` 目录）
- [x] 7.2 安装 React Router v6、Axios、Ant Design 5、Zustand 依赖
- [x] 7.3 配置 `vite.config.ts` 代理 `/api` 到 `http://localhost:8080`
- [x] 7.4 创建目录结构：`components/`、`pages/`、`api/`、`router/`、`store/`、`utils/`
- [x] 7.5 配置 TypeScript 路径别名（`@/` → `src/`）
- [x] 7.6 验证：`npm run dev` 启动前端开发服务器

## 8. 前端基础设施 — 路由、HTTP 客户端、UI、状态管理

- [x] 8.1 创建路由配置，首页 `/` 和 404 页面，使用 `React.lazy` 懒加载
- [x] 8.2 封装 Axios 实例，配置 `baseURL`、请求拦截器、响应拦截器（自动解包 `data` 字段）
- [x] 8.3 全局配置 Ant Design 中文国际化（ConfigProvider、App 组件）
- [x] 8.4 创建 Zustand Store，提供全局用户信息状态
- [x] 8.5 验证：访问首页正常渲染，API 代理正常工作

## 9. 用户管理 — 前端 CRUD 示例

- [x] 9.1 创建 `src/api/user.ts` 用户 API 封装方法（list、create、update、remove）
- [x] 9.2 创建 `UserListPage` 页面，使用 Ant Design `Table` 展示用户分页列表
- [x] 9.3 实现关键词搜索功能（Search 组件 + 分页参数联动）
- [x] 9.4 实现新增用户 Modal 表单（Ant Design `Form` + `Modal`）
- [x] 9.5 实现编辑用户 Modal 表单（预填当前行数据）
- [x] 9.6 实现删除确认对话框（`Popconfirm` + 删除后刷新列表）
- [x] 9.7 添加 `/users` 路由到路由配置
- [x] 9.8 验证：前后端联调，用户 CRUD 全流程正常

## 10. 收尾与验证

- [x] 10.1 更新项目 README，新增技术栈说明和快速启动指南
- [x] 10.2 验证全栈启动流程：后端 `mvn spring-boot:run` + 前端 `npm run dev`，确认联调正常
- [x] 10.3 清理占位文件（`src/.gitkeep`、`tests/.gitkeep`），确保 `.gitignore` 覆盖 Java 和 Node 产物
