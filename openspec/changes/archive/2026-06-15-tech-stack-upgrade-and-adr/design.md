## Context

项目从初始的 React 18 + Vite 6 + react-router-dom 前端架构，演进为 Next.js 15 (App Router) + React 19 架构。后端保持 SpringBoot 3 四层架构不变。项目已升级为 AIWorkSpace，frontend/backend 通过 git submodule 独立管理。

变更驱动力：
1. 团队 3 人全员 React 经验，AI 辅助开发为主
2. 需要生产级长期可迭代的后端架构
3. 前端需要 Vercel 零配置部署能力
4. 前端框架训练数据量直接影响 AI 代码生成质量

## Goals / Non-Goals

**Goals:**
- 前端迁移至 Next.js 15 App Router，实现文件路由和 SSR 支持
- 所有依赖升级至最新稳定版（React 19、Ant Design 6、Next.js 15）
- 建立技术选型 ADR，记录决策过程和约束红线
- 前端可通过 Vercel 一键部署

**Non-Goals:**
- 后端架构变更（SpringBoot 保持不变）
- 引入 Next.js API Routes 替代后端
- 数据库迁移或数据模型变更
- CI/CD 流水线搭建

## Decisions

### D1: 前端框架选 Next.js 而非 Nuxt

**选择**: Next.js 14+ (App Router)
**替代方案**: Nuxt 3
**理由**:
- 团队全员 React 经验，Nuxt 需转 Vue（学习成本 3/10 vs 8/10）
- Next.js LLM 训练语料最多，AI 生成质量 9/10 vs 6/10
- Vercel 原生支持，部署敏捷度 10/10 vs 7/10
- 生态成熟度 9/10 vs 7/10

### D2: 后端框架选 SpringBoot 而非 Next.js API Routes

**选择**: SpringBoot 3
**替代方案**: Next.js Route Handlers
**理由**:
- 团队 SpringBoot 经验丰富，学习成本 9/10
- 分层架构（Controller/Service/Repository）约定强，3 人写出一致代码
- 长期可维护性 10/10（事务/权限/缓存/监控全套企业级方案）
- 生产可观测性（Actuator + Micrometer + Prometheus）
- API Routes 约定太弱（6/10），复杂业务后代码组织混乱

### D3: Next.js 15 + React 19 版本策略

**选择**: 升级至最新稳定版而非保守 LTS
**理由**:
- Next.js 15 是首个正式支持 React 19 的稳定版
- React 19 是当前最新稳定版（19.2.7），非 RC
- Ant Design 6 peerDeps 声明 `>=18.0.0`，兼容 React 19
- 新项目无需兼容旧版本负担

### D4: 前端目录结构从 src/ 迁移至 Next.js 约定

**选择**: app/ + components/ + lib/ 三级结构
**替代方案**: 保留 src/ 前缀（src/app/、src/components/）
**理由**:
- Next.js 官方脚手架和文档均使用 app/ 根目录结构
- AI 训练数据中 Next.js 项目多为 app/ 根结构，提升生成准确率
- 更扁平的路径减少嵌套层级

## Risks / Trade-offs

- **[Ant Design 6 Breaking Changes]** → 已通过 build 验证确认现有组件兼容，但后续使用新组件时需关注 API 变更
- **[Next.js 15 不再支持 React 18]** → 已全面升级至 React 19，无回退需求
- **[双仓库部署复杂度]** → 前端 Vercel + 后端 Docker 需分别配置，联调依赖环境变量正确设置
- **[SSR 兼容性]** → 所有使用 hooks/浏览器 API 的组件已标记 `'use client'`，AntdRegistry 解决样式闪烁