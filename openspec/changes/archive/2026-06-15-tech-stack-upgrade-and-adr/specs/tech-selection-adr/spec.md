## ADDED Requirements

### Requirement: 技术选型决策记录

系统 SHALL 在 AGENTS.md 中维护技术选型约束文档，包含团队与项目硬约束、前后端选型决策及评分对比、选型红线三部分内容。

#### Scenario: 选型决策可追溯

- **WHEN** 新成员或 AI 代理查看 AGENTS.md
- **THEN** SHALL 能看到前端选 Next.js（非 Nuxt）的五维评分对比和后端选 SpringBoot（非 API Routes）的六维评分对比

#### Scenario: 选型红线约束生效

- **WHEN** AI 代理尝试在代码生成中使用 Next.js API Routes 替代 SpringBoot 做后端
- **THEN** 代理 SHALL 识别到 AGENTS.md 中的红线约束并拒绝该操作

---

### Requirement: 前端选型决策

系统 SHALL 明确前端选型为 Next.js (App Router)，基于五维评估：AI 生成质量（9/10）、框架约定强度（8/10）、生态成熟度（9/10）、部署敏捷度（10/10）、团队学习成本（8/10），综合得分高于 Nuxt 3。

#### Scenario: 前端框架选择验证

- **WHEN** 团队评估新前端框架选项
- **THEN** SHALL 参考五维评分表，确认 Next.js 在所有维度均优于或等于 Nuxt

---

### Requirement: 后端选型决策

系统 SHALL 明确后端选型为 SpringBoot 3，基于六维评估：AI 生成质量（7/10）、框架约定强度（9/10）、生态成熟度（10/10）、部署敏捷度（5/10）、团队学习成本（9/10）、长期可维护性（10/10），综合优于 Next.js API Routes。

#### Scenario: 后端框架选择验证

- **WHEN** 团队评估后端架构方案
- **THEN** SHALL 参考六维评分表，确认 SpringBoot 在约定强度、生态成熟度、长期可维护性上显著优于 API Routes

---

### Requirement: 选型红线

系统 SHALL 在 AGENTS.md 中定义选型红线：禁止用 Next.js API Routes 替代 SpringBoot、禁止前端选用非 React 生态框架、禁止后端引入非 Java 语言，变更需补充 ADR。

#### Scenario: 红线违反检测

- **WHEN** 代码审查发现有人引入 Next.js API Route 替代后端 API
- **THEN** 审查 SHALL 拒绝该变更，引用 AGENTS.md 红线约束