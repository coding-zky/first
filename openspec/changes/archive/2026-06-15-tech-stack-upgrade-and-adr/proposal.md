## Why

项目已完成前端框架迁移（Vite → Next.js 15 + React 19）和版本升级（Ant Design 5→6、全量依赖升级至最新稳定版），同时确立了技术选型决策（前端 Next.js / 后端 SpringBoot）并写入 AGENTS.md。这些变更已通过 git 提交落地，但缺乏 OpenSpec 规范记录和 ADR 决策文档归档，导致变更追溯性和决策可审计性不足。

## What Changes

- **BREAKING** 前端框架从 React 18 + Vite 6 迁移至 Next.js 15 (App Router) + React 19
- **BREAKING** Ant Design 5 升级至 Ant Design 6，@ant-design/icons 5→6
- **BREAKING** 路由方案从 react-router-dom 切换为 Next.js 文件路由 (app/ 目录)
- 新增 @ant-design/nextjs-registry 解决 Ant Design SSR 样式闪烁
- API 代理从 Vite proxy 切换为 Next.js rewrites
- 项目升级为 AIWorkSpace，frontend/backend 以 git submodule 引入
- 技术选型决策（Next.js vs Nuxt、SpringBoot vs Next.js API Routes）写入 AGENTS.md
- 选型红线约束写入 AGENTS.md（禁止用 API Routes 替 SpringBoot、禁止非 React 框架等）
- 前端目录结构从 src/ 重构为 app/ + components/ + lib/
- 开发端口从 5173 变更为 3000

## Capabilities

### New Capabilities

- `nextjs-app-router`: Next.js 15 App Router 工程结构（文件路由、Server Components、AntdRegistry、rewrites 代理）
- `tech-selection-adr`: 技术选型决策记录（前端 Next.js vs Nuxt 五维评估、后端 SpringBoot vs API Routes 六维评估、选型红线）

### Modified Capabilities

- `react-spa-scaffold`: 前端工程从 Vite + React 18 架构变更为 Next.js 15 + React 19 架构，路由、代理、目录结构均有 breaking changes

## Impact

- **frontend/ (submodule)**: 全部源码重构（package.json、目录结构、页面组件、布局组件）
- **AGENTS.md**: 新增技术选型约束章节、更新技术栈描述、更新端口和目录说明
- **根仓库**: submodule 引用更新、.gitmodules 新增
- **openspec/specs/react-spa-scaffold**: 需要更新以反映新架构
- **docs/design-docs/**: 需补充 ADR 决策文档
