## Why

每次启动一个新的全栈项目，都需要从零搭建项目结构、配置 SpringBoot、配置前端工程、处理跨域、统一响应格式、异常处理等基础设施 — 这些重复工作耗时且容易遗漏。我们需要一个标准化的全栈开发脚手架，clone 即可用，让团队聚焦业务开发而非项目搭建。

## What Changes

- 新增 SpringBoot 3.x 后端工程骨架，包含 Maven 多模块项目结构、统一响应封装、全局异常处理、CORS 跨域配置、参数校验、MyBatis-Plus 集成、Swagger 接口文档、Logback 日志配置
- 新增 React 18+ 前端工程骨架，包含 Vite + TypeScript 构建、React Router 路由、Axios HTTP 客户端、Ant Design UI 组件库、Zustand 状态管理、前后端代理配置
- 新增用户管理示例模块，完整演示前后端 CRUD 联调流程（列表查询、新增、编辑、删除）
- 开发期使用 H2 内存数据库实现零配置启动，生产环境切换 MySQL

## Capabilities

### New Capabilities

- `springboot-api-scaffold`: SpringBoot 后端工程骨架，包含项目结构、统一响应、异常处理、CORS、校验、ORM、文档、日志等基础设施
- `react-spa-scaffold`: React 前端工程骨架，包含 Vite 构建、路由、HTTP 客户端、UI 组件库、状态管理、代理配置
- `sample-user-management`: 用户管理 CRUD 示例模块，演示前后端完整联调流程

### Modified Capabilities

<!-- 新项目，无现有规范需要修改 -->

## Impact

- **项目结构**: 在现有 `src/` 和 `tests/` 占位目录基础上，建立完整的 Maven + React 前后端分离项目结构
- **依赖**: 新增 SpringBoot、MyBatis-Plus、Swagger、React、Vite、Ant Design 等核心依赖
- **构建**: Maven 作为后端构建工具，Vite 作为前端构建工具，支持独立构建和部署
- **开发体验**: 前后端均可独立启动开发服务器，通过 Vite proxy 实现开发期前后端联调
