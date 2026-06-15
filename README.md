# 全栈开发脚手架

> SpringBoot 3.4 + React 18 + TypeScript — clone 即可用的标准化项目模板

## 技术栈

| 层级 | 技术 | 版本 |
|------|------|------|
| 后端框架 | SpringBoot | 3.4.1 |
| ORM | MyBatis-Plus | 3.5.5 |
| 数据库(开发) | H2 | 内置 |
| 数据库(生产) | MySQL | 8.0+ |
| API 文档 | Knife4j | 4.5.0 |
| 前端框架 | React | 18 |
| 构建工具 | Vite | 6 |
| 语言 | TypeScript | 5 |
| UI | Ant Design | 5 |
| 状态管理 | Zustand | 5 |

## 项目结构

```
├── .harness/           # Agent 规则 (rules.yml) + 模板 (templates/)
├── docs/
│   ├── product-specs/  # 产品需求文档
│   ├── api-specs/      # API 接口规范
│   ├── design-docs/    # 架构设计与技术选型 ADR
│   ├── ui-specs/       # UI 设计规范
│   └── exec-plans/     # Sprint 执行计划
├── knowledge/          # 编码规范 + 经验教训
├── frontend/           # React + Vite 前端 SPA
├── backend/            # SpringBoot 后端四层架构
│   ├── pom.xml         # 父 POM（依赖版本管理）
│   ├── common/         # 公共模块：Result、异常、DTO
│   ├── dao/            # 数据层：Entity、Mapper
│   ├── service/        # 业务层：Service 接口与实现
│   └── api/            # 控制层：REST Controller、配置
├── AGENTS.md           # AI Agent 顶层导航
├── .cursorrules        # 命名 + 禁止 API 约束
└── README.md
```

**Java 包名**: `com.imooc.app` (common / dao / service / api)

## 快速开始

### 环境要求

- JDK 17+（已在 Java 25 上验证）
- Maven 3.6+
- Node.js 18+
- npm 9+

### 启动后端

```bash
cd backend
mvn install -DskipTests       # 首次需安装子模块
mvn spring-boot:run -pl api   # 启动 SpringBoot（端口 8080）
```

### 启动前端

```bash
cd frontend
npm install    # 安装依赖
npm run dev    # 启动开发服务器（端口 5173）
```

### 访问

| 地址 | 说明 |
|------|------|
| http://localhost:5173 | 前端开发服务器 |
| http://localhost:8080/doc.html | Swagger API 文档 |
| http://localhost:8080/h2-console | H2 数据库控制台 |

## 关键约定

- 不使用 Lombok（手写 getter/setter，兼容 Java 25）
- Controller 统一返回 `Result<T>`，异常交给 `GlobalExceptionHandler`
- 前端使用函数组件 + Hooks，状态管理用 Zustand
- 详见 `knowledge/conventions.md` 和 `.cursorrules`
