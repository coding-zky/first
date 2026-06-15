# 技术选型决策记录 (ADR)

## 1. Maven 多模块结构

**决策**: 父子多模块 — `common` / `dao` / `service` / `api`，统一包名 `com.imooc.app`

**理由**: 四层结构是 SpringBoot 经典分层模式，职责清晰，依赖单向（api → service → dao → common）

**替代方案**: 单模块简单但不利扩展；DDD 四层对脚手架过度设计

## 2. 前端构建工具

**决策**: Vite（非 Create React App）

**理由**: CRA 已停止维护。Vite 冷启动快、HMR 即时生效，TypeScript 开箱即用

**替代方案**: Next.js 引入 SSR 复杂性，通用脚手架目标过高

## 3. UI 组件库

**决策**: Ant Design 5

**理由**: 国内最主流 React 企业级 UI 库，Table/Form/Modal 开箱即用，中文支持完善

## 4. 状态管理

**决策**: Zustand

**理由**: ~1KB 轻量级，API 简洁，基于 Hook，避免 Redux 模板代码

## 5. 数据库方案

**决策**: H2 内存数据库（开发）+ MySQL（生产），Spring Profile 切换

**理由**: H2 零配置启动，开发体验好；生产 MySQL 性能可靠

## 6. ORM

**决策**: MyBatis-Plus

**理由**: BaseMapper 通用 CRUD + 分页插件 + 条件构造器，开发效率高，SQL 可控

## 7. 前后端联调

**决策**: Vite proxy（首选）+ CORS 配置（备用）

**理由**: Vite proxy 最干净；CORS 处理 Swagger UI 等非代理场景

## 风险

| 风险 | 缓解 |
|------|------|
| H2 与 MySQL 方言差异 | 使用标准 SQL + MyBatis-Plus 方法 |
| JDK 17+ 要求 | README 明确版本要求 |
| React SPA 首屏性能 | 路由懒加载 + 代码分割 |
