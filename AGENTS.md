# AGENTS.md — AIWorkSpace 顶层导航

> 本仓库是一个 AIWorkSpace：以 AI 编码代理为核心协作者的全栈项目工作空间。
> `frontend/` 和 `backend/` 以 git submodule 方式引入，各自独立版本管理。

## 仓库结构

| 目录 | 类型 | 仓库地址 | 说明 |
|------|------|----------|------|
| `frontend/` | **submodule** | `https://github.com/coding-zky/first_frontend.git` | Next.js 14 (App Router) + TypeScript 前端 SPA |
| `backend/` | **submodule** | `https://github.com/coding-zky/first_backend.git` | SpringBoot 3.4 后端四层架构（common/dao/service/api） |
| `docs/product-specs/` | 本地 | — | 产品需求文档 |
| `docs/api-specs/` | 本地 | — | API 接口规范 |
| `docs/design-docs/` | 本地 | — | 架构设计与技术选型 ADR |
| `docs/ui-specs/` | 本地 | — | UI 设计规范 |
| `docs/exec-plans/` | 本地 | — | Sprint 执行计划 |
| `knowledge/` | 本地 | — | 编码规范 + 经验教训 |
| `.harness/` | 本地 | — | Agent 规则 + 模板 |
| `openspec/` | 本地 | — | OpenSpec 规范驱动工作流 |

## Submodule 操作指南

```bash
# 首次 clone（含 submodule）
git clone --recurse-submodules https://github.com/coding-zky/first.git

# 已有仓库，拉取 submodule
git submodule update --init --recursive

# 更新 submodule 到远程最新
git submodule update --remote frontend
git submodule update --remote backend
```

### frontend/ — Next.js 前端

- **技术栈**: Next.js 14 (App Router) / React 18 / TypeScript / Ant Design 5 / Zustand
- **包管理**: npm (`package.json`)
- **启动**: `cd frontend && npm install && npm run dev`
- **端口**: 3000，`/api` 通过 rewrites 代理到后端 8080
- **目录**: `app/` (页面) / `components/` (组件) / `lib/` (工具+API)

### backend/ — SpringBoot 后端

- **技术栈**: Java 17+ / SpringBoot 3.4.1 / MyBatis-Plus 3.5.5 / H2 + MySQL
- **包名**: `com.imooc.app` (common / dao / service / api)
- **构建**: Maven 多模块（pom.xml 在 backend/ 根目录）
- **启动**: `cd backend && mvn install -DskipTests && mvn spring-boot:run -pl api`
- **端口**: 8080，Swagger 文档 `/doc.html`

## 快速启动

```bash
# 1. clone AIWorkSpace
git clone --recurse-submodules https://github.com/coding-zky/first.git
cd first

# 2. 启动后端
cd backend && mvn install -DskipTests && mvn spring-boot:run -pl api

# 3. 启动前端（新终端）
cd frontend && npm install && npm run dev
```

| 地址 | 说明 |
|------|------|
| http://localhost:3000 | 前端开发服务器 (Next.js) |
| http://localhost:8080/doc.html | Swagger API 文档 |
| http://localhost:8080/h2-console | H2 数据库控制台 |

## 技术选型约束

> 以下约束是所有开发决策的硬边界，AI 代理和团队成员必须遵守。

### 团队与项目约束

| 约束 | 值 | 影响 |
|------|------|------|
| 团队规模 | 3 人 | 代码约定必须强到"3 人写出像 1 人" |
| 前端经验 | 全员 React | 前端框架必须基于 React 生态 |
| 后端经验 | SpringBoot 经验丰富 | 后端可选 SpringBoot，学习成本为零 |
| MVP 周期 | 8 周 | 排除需要长学习曲线的方案 |
| 迭代目标 | 线上可持续运行迭代 | 架构可维护性 > 短期开发速度 |
| 部署方式 | 前端 Vercel / 后端 Docker | 前端 serverless，后端容器化 |
| 开发模式 | AI 辅助开发为主 | 框架训练数据量决定 AI 代码质量 |

### 选型决策

**前端：Next.js 14 (App Router)** — AI 生成质量最高 + 团队 React 经验直接复用 + Vercel 零配置部署

| 维度 | Next.js 14 | Nuxt 3 |
|------|-----------|--------|
| AI 生成质量 | **9/10** | 6/10 |
| 框架约定强度 | **8/10** | 8/10 |
| 生态成熟度 | **9/10** | 7/10 |
| 部署敏捷度 | **10/10** | 7/10 |
| 团队学习成本 | **8/10**（全员 React） | 3/10（需转 Vue） |

**后端：SpringBoot 3** — 长期可维护 + 企业级生态 + 团队经验匹配

| 维度 | SpringBoot 3 | Next.js API Routes |
|------|-------------|---------------------|
| AI 生成质量 | 7/10 | **9/10** |
| 框架约定强度 | **9/10** | 6/10 |
| 生态成熟度 | **10/10** | 6/10 |
| 部署敏捷度 | 5/10 | **10/10** |
| 团队学习成本 | **9/10**（有经验） | 9/10 |
| 长期可维护性 | **10/10** | 4/10 |

### 选型红线

- **禁止** 用 Next.js API Routes 替代 SpringBoot 做后端（约定太弱，长期不可维护）
- **禁止** 前端选用非 React 生态框架（团队无经验，学习成本不可接受）
- **禁止** 后端引入 Kotlin / Scala 等非 Java 语言（团队 Java 经验为核心资产）
- 如需引入新框架/库，必须在 `docs/design-docs/` 中补充 ADR 决策记录

## 规则来源

- `.harness/rules.yml` — Agent 行为规范
- `knowledge/conventions.md` — 编码规范
- `knowledge/lessons-learned.md` — 经验教训
- `.cursorrules` — 命名 + 禁止 API 约束
