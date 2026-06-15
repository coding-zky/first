## ADDED Requirements

### Requirement: Maven 多模块项目结构

系统 SHALL 提供基于 Maven 的父子多模块项目结构，包括 `common`（公共模块）、`api`（控制器层）、`service`（业务层）、`dao`（数据访问层）四个子模块，统一基础包名 `com.imooc.app`，以及统一的父 POM 管理所有依赖版本。

#### Scenario: 项目结构完整性

- **WHEN** 开发者 clone 项目并执行 `mvn compile`
- **THEN** 所有模块 SHALL 编译通过，无错误

#### Scenario: 父 POM 依赖管理

- **WHEN** 子模块引用已在父 POM 中管理的依赖
- **THEN** 子模块无需声明版本号，版本由父 POM 统一控制

---

### Requirement: 统一响应结构

系统 SHALL 提供统一的 API 响应封装类 `Result<T>`，包含 `code`（状态码）、`message`（消息）、`data`（数据）三个字段，所有 Controller 接口返回 `Result<T>` 类型。

#### Scenario: 成功响应

- **WHEN** API 请求处理成功
- **THEN** 返回 `Result<T>`，其中 `code` 为 `200`，`message` 为 `"success"`，`data` 包含业务数据

#### Scenario: 业务异常响应

- **WHEN** API 请求处理过程中发生业务异常
- **THEN** 返回 `Result<T>`，其中 `code` 为对应业务错误码，`message` 为错误描述，`data` 为 `null`

---

### Requirement: 全局异常处理

系统 SHALL 通过 `@RestControllerAdvice` 实现全局异常拦截，捕获业务异常（`BusinessException`）和系统异常（`Exception`），统一转换为 `Result<T>` 格式返回。

#### Scenario: 业务异常处理

- **WHEN** Service 层抛出 `BusinessException("用户不存在")`
- **THEN** 全局异常处理器 SHALL 捕获该异常，并返回 `code` 为 `400`、`message` 为 `"用户不存在"` 的 `Result`

#### Scenario: 未知异常处理

- **WHEN** 系统发生未预期的 `RuntimeException`
- **THEN** 全局异常处理器 SHALL 捕获该异常，返回 `code` 为 `500`、`message` 为 `"系统内部错误"` 的 `Result`，并记录完整堆栈日志

---

### Requirement: CORS 跨域配置

系统 SHALL 配置全局 CORS 支持，允许前端开发服务器（默认 `http://localhost:5173`）的跨域请求，允许携带认证信息。

#### Scenario: 前端开发环境跨域请求

- **WHEN** React 开发服务器（`localhost:5173`）向后端 API 发起跨域请求
- **THEN** 后端 SHALL 返回正确的 CORS 响应头，请求成功执行

#### Scenario: 生产环境 CORS

- **WHEN** 在生产环境中部署
- **THEN** CORS 允许的来源 SHALL 可通过配置文件修改，不硬编码开发地址

---

### Requirement: 请求参数校验

系统 SHALL 集成 Jakarta Bean Validation，Controller 层使用 `@Valid` 注解自动校验请求参数，校验失败时返回 400 错误及具体字段错误信息。

#### Scenario: 必填字段缺失

- **WHEN** 请求 DTO 中 `@NotBlank` 修饰的 `name` 字段为空
- **THEN** 系统 SHALL 返回 `code` 为 `400`、`message` 包含 `"name 不能为空"` 的错误响应

---

### Requirement: MyBatis-Plus 数据访问层

系统 SHALL 集成 MyBatis-Plus，提供 `BaseMapper<T>` 的基础 CRUD 能力，支持分页查询插件配置。

#### Scenario: 基础 CRUD 操作

- **WHEN** 调用 `userMapper.selectById(1L)`
- **THEN** MyBatis-Plus SHALL 自动生成 SQL 并查询 ID 为 1 的用户记录

#### Scenario: 分页查询

- **WHEN** 调用 `userMapper.selectPage(page, wrapper)`
- **THEN** MyBatis-Plus SHALL 返回包含 `total`、`records` 的分页结果

---

### Requirement: 数据库环境切换

系统 SHALL 支持 H2 内存数据库（开发环境）和 MySQL（生产环境）两种数据源，通过 Spring Profile（`dev`/`prod`）切换，开发环境零配置即可启动。

#### Scenario: 开发环境 H2 数据库

- **WHEN** 以 `dev` Profile 启动应用
- **THEN** 系统 SHALL 使用 H2 内存数据库，控制台可通过 `/h2-console` 访问

#### Scenario: 生产环境 MySQL

- **WHEN** 以 `prod` Profile 启动应用
- **THEN** 系统 SHALL 使用 MySQL 数据库，连接信息从 `application-prod.yml` 读取

---

### Requirement: Swagger 接口文档

系统 SHALL 集成 SpringDoc OpenAPI，启动后可通过 `/doc.html` 访问 Swagger UI 接口文档，支持在线调试 API。

#### Scenario: 访问接口文档

- **WHEN** 开发者访问 `http://localhost:8080/doc.html`
- **THEN** SHALL 展示包含所有 Controller 接口的 Swagger UI 页面，可在线发送请求并查看响应

---

### Requirement: 日志配置

系统 SHALL 使用 Logback 进行日志管理，控制台输出 INFO 级别日志，文件按日期滚动存储，MyBatis SQL 日志在 dev 环境下输出。

#### Scenario: SQL 日志输出

- **WHEN** 以 `dev` Profile 运行应用并执行数据库查询
- **THEN** 控制台 SHALL 输出 MyBatis-Plus 生成的 SQL 语句及参数
