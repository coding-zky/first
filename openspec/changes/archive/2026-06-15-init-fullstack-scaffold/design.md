## Context

当前项目是一个基于 OpenSpec + Superpowers 方法论的 AI Coding 项目模板，仅有 Node.js 工具链和占位目录结构。需要在此基础上搭建 Java SpringBoot + React 的全栈开发脚手架，作为后续项目的起点。

约束条件：
- 后端使用 Java + SpringBoot 3.x + Maven
- 前端使用 React SPA，前后端分离
- 开发体验优先：clone 后 5 分钟内可启动全栈应用
- 开发期零配置（H2 内存数据库），生产环境可切换 MySQL

## Goals / Non-Goals

**Goals:**
- 建立标准化 Maven 多模块后端项目结构，统一依赖版本管理
- 提供开箱即用的后端基础设施：统一响应、异常处理、CORS、校验、ORM、文档、日志
- 建立 React + Vite + TypeScript 前端工程，集成路由、HTTP 客户端、UI 组件库、状态管理
- 提供用户管理 CRUD 完整示例，演示前后端联调流程
- H2 开发 / MySQL 生产双环境数据库配置

**Non-Goals:**
- 不实现用户认证与权限管理（脚手架不包含安全模块，后续项目按需集成）
- 不实现 CI/CD 流水线配置
- 不实现 Docker 容器化
- 不实现微服务架构（保持单体应用结构，后续可拆分）

## Decisions

### 1. Maven 多模块结构

**决策**: 采用父子多模块结构，分为 `common`、`api`、`service`、`dao` 四层，统一基础包名 `com.imooc.app`。

```
my-first-project/
├── pom.xml                    (父 POM，依赖版本管理)
├── common/                    (公共模块：Result、异常、工具类)
│   └── com.imooc.app.common
├── api/                       (控制器层：REST Controller)
│   └── com.imooc.app.api
├── service/                   (业务逻辑层)
│   └── com.imooc.app.service
└── dao/                       (数据访问层：Entity、Mapper)
    └── com.imooc.app.dao
```

各模块包结构：
- `com.imooc.app.common` — `result/`（Result<T>）、`exception/`（BusinessException）、`dto/`（数据传输对象）
- `com.imooc.app.api` — `controller/`（REST Controller）、`config/`（CORS、Swagger、校验配置）
- `com.imooc.app.service` — `service/`（业务接口）、`service/impl/`（业务实现）
- `com.imooc.app.dao` — `entity/`（实体类）、`mapper/`（MyBatis Mapper）

**理由**: 四层结构是 SpringBoot 项目的经典分层模式，职责清晰，模块间依赖单向（api → service → dao → common）。相比单模块，多模块更有利于后续功能扩展和团队分工。

**替代方案**: 单模块结构更简单，但不利于大型项目演进；DDD 四层结构（interfaces/application/domain/infrastructure）更复杂，对脚手架来说过度设计。

### 2. React 前端脚手架工具

**决策**: 使用 Vite（而非 Create React App）作为构建工具。

**理由**: Vite 是 Vue/Ract 社区推荐的现代化构建工具，CRA 已停止维护。Vite 冷启动快、HMR 即时生效，开发体验显著优于 Webpack。TypeScript 开箱即用。

**替代方案**: Create React App 已不被推荐；Next.js 功能强大但引入了 SSR 复杂性，作为通用脚手架目标过高。

### 3. UI 组件库

**决策**: 使用 Ant Design 5。

**理由**: Ant Design 是国内最主流的 React 企业级 UI 组件库，提供 Table、Form、Modal 等开箱即用的组件，中文支持完善，适合管理后台类应用。

**替代方案**: Material UI 国际化设计风格与国内审美差距大；Arco Design 较新但生态不如 Ant Design。

### 4. 状态管理

**决策**: 使用 Zustand。

**理由**: Zustand 是一个轻量级状态管理库（~1KB），API 简洁，基于 Hook，避免了 Redux 的模板代码。对于脚手架级别的全局状态（用户信息、认证状态）足够使用。

**替代方案**: Redux Toolkit 功能更强但模板代码多；Jotai 更底层；React Context 简单场景足够但性能不如 Zustand。

### 5. 数据库方案

**决策**: 开发环境使用 H2 内存数据库，生产环境使用 MySQL，通过 Spring Profile 切换。

**理由**: H2 内存数据库无需安装配置，SpringBoot 自动装配，应用启动即自动建表，真正实现"零配置启动"。生产环境 MySQL 配置文件独立，切换只需激活 `prod` Profile。

**替代方案**: 开发也连 MySQL 需要本地安装数据库，违背"零配置"目标；纯 H2 在生产环境性能不足。

### 6. ORM 选择

**决策**: MyBatis-Plus。

**理由**: MyBatis-Plus 在国内 SpringBoot 项目中使用广泛，基于 MyBatis 增强，提供 `BaseMapper` 通用 CRUD、分页插件、条件构造器，开发效率高。相比 JPA 更灵活，SQL 可控。

**替代方案**: Spring Data JPA 更面向对象，但复杂查询不够灵活；纯 MyBatis 需要手写 XML，效率低。

### 7. 前后端开发联调方案

**决策**: 双管齐下 — Vite proxy 代理（开发首选）+ SpringBoot CORS 配置（备用）。

**理由**: Vite proxy 让前端开发时的 `/api` 请求直接代理到后端，避免跨域问题，是最干净的开发联调方案。CORS 配置作为备选，处理非代理场景（如 Swagger UI 直接调用、其他工具测试）。

## Risks / Trade-offs

| 风险 | 缓解措施 |
|------|---------|
| H2 与 MySQL SQL 方言差异导致开发/生产行为不一致 | 使用标准 SQL + MyBatis-Plus 内置方法，避免数据库特定语法 |
| SpringBoot 3.x 要求 JDK 17+，部分开发者环境可能不匹配 | 在 README 中明确 JDK 版本要求，配置 Maven 编译插件指定 Java 版本 |
| React SPA 首屏加载性能 | 路由懒加载 + Vite 代码分割，脚手架阶段数据量小不构成问题 |
| 多模块 Maven 构建增加复杂度 | 父 POM 统一管理，子模块极简配置，新人容易理解 |
