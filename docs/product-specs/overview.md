# 产品概述

## 项目定位

全栈开发脚手架 — 一套标准化的 SpringBoot + React 项目模板，clone 即可用，让团队聚焦业务开发而非项目搭建。

## 核心能力

### 后端 (SpringBoot 3.x)

- Maven 多模块项目结构（common / dao / service / api 四层）
- 统一响应封装 `Result<T>` + 全局异常处理
- CORS 跨域配置 + Jakarta Bean Validation 参数校验
- MyBatis-Plus ORM + H2（开发）/ MySQL（生产）双环境
- Knife4j Swagger 接口文档（`/doc.html`）
- Logback 日志配置（控制台 + 文件滚动）

### 前端 (React 18 + Vite)

- Vite + TypeScript 构建，HMR 即时生效
- React Router v6 路由（懒加载）
- Axios HTTP 客户端（响应拦截器自动解包）
- Ant Design 5 中文国际化 UI 组件库
- Zustand 轻量级状态管理
- Vite proxy 开发联调（`/api` → `localhost:8080`）

### 示例模块 — 用户管理

完整的用户 CRUD 示例，演示前后端联调流程：
- 分页列表 + 关键词搜索
- 新增 / 编辑 Modal 表单
- 删除确认 + 参数校验

## 非目标

- 不实现用户认证与权限管理（脚手架不含安全模块）
- 不实现 CI/CD 流水线
- 不实现 Docker 容器化
- 不实现微服务架构（保持单体结构）

## 技术选型决策

详见 [技术选型 ADR](../design-docs/tech-selection-adr.md)
