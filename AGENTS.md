# AGENTS.md — AIWorkSpace 顶层导航

> 本仓库是一个 AIWorkSpace：以 AI 编码代理为核心协作者的全栈项目工作空间。
> `frontend/` 和 `backend/` 以 git submodule 方式引入，各自独立版本管理。

## 仓库结构

| 目录 | 类型 | 仓库地址 | 说明 |
|------|------|----------|------|
| `frontend/` | **submodule** | `https://github.com/coding-zky/first_frontend.git` | React 18 + Vite + TypeScript 前端 SPA |
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

### frontend/ — React 前端

- **技术栈**: React 18 / Vite 6 / TypeScript / Ant Design 5 / Zustand
- **包管理**: npm (`package.json`)
- **启动**: `cd frontend && npm install && npm run dev`
- **端口**: 5173，`/api` 代理到后端 8080

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
| http://localhost:5173 | 前端开发服务器 |
| http://localhost:8080/doc.html | Swagger API 文档 |
| http://localhost:8080/h2-console | H2 数据库控制台 |

## 规则来源

- `.harness/rules.yml` — Agent 行为规范
- `knowledge/conventions.md` — 编码规范
- `knowledge/lessons-learned.md` — 经验教训
- `.cursorrules` — 命名 + 禁止 API 约束
