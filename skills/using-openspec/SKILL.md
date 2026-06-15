---
name: using-openspec
description: 当需要管理规范文档、创建变更提案或归档已完成功能时使用。提供 OpenSpec 斜杠命令和 CLI 的完整参考。
---

# 使用 OpenSpec 规范驱动开发

OpenSpec 是项目的规范驱动开发（SDD）框架。在编写代码之前，通过规范文档对齐需求。

## 何时使用

- 开始一个新功能 → 使用 `/opsx:propose <变更名>`
- 已有变更需要实现 → 使用 `/opsx:apply`
- 功能完成需要归档 → 使用 `/opsx:archive`
- 需要查看所有变更 → 使用 `openspec list`
- 需要验证规范格式 → 使用 `openspec validate`

## AI 斜杠命令

在对话中直接使用 `/opsx:` 前缀命令，AI 代理会执行对应操作：

### 默认工作流（core 配置）

```
/opsx:propose ──→ /opsx:apply ──→ /opsx:sync ──→ /opsx:archive
```

### 命令详解

| 命令 | 说明 |
|------|------|
| `/opsx:propose <名称>` | 创建变更提案。自动生成 proposal.md、design.md、tasks.md 和 delta specs |
| `/opsx:apply` | 按 tasks.md 任务清单逐步实现变更 |
| `/opsx:sync` | 将 delta 规范同步合并到 `openspec/specs/` 主规范 |
| `/opsx:archive` | 归档已完成变更到 `openspec/changes/archive/` |
| `/opsx:new <名称>` | 仅创建变更文件夹骨架（扩展工作流） |
| `/opsx:ff` | 快速填充变更内容 |
| `/opsx:continue` | 增量继续填充下一个未完成的 artifact |
| `/opsx:verify` | 验证变更完整性 |

### Delta 规范格式

在变更的 specs 目录中使用以下标记描述需求变更：

```markdown
# Delta for <领域名>

## ADDED Requirements

### Requirement: <需求名称>
<需求描述>

#### Scenario: <场景名称>
- GIVEN <前置条件>
- WHEN <触发动作>
- THEN <预期结果>

## MODIFIED Requirements

### Requirement: <需求名称>
<修改后的完整描述>
(Previously: <原描述>)

## REMOVED Requirements

### Requirement: <需求名称>
(移除原因)
```

## CLI 命令参考

在终端中运行 `openspec` 命令：

| 命令 | 用途 |
|------|------|
| `openspec init` | 初始化项目 OpenSpec 结构（首次使用） |
| `openspec init --tools qoder` | 为 Qoder 生成 AI 指令文件 |
| `openspec update` | 升级 CLI 后更新 AI 指令文件 |
| `openspec list` | 列出所有活跃变更 |
| `openspec list --json` | JSON 格式输出（供代理使用） |
| `openspec show <名称>` | 查看变更详情 |
| `openspec validate <名称>` | 验证单个变更的规范格式 |
| `openspec validate --all` | 验证所有变更 |
| `openspec view` | 启动交互式仪表盘 |
| `openspec status` | 查看 artifact 进度 |
| `openspec status --json` | JSON 格式输出 |
| `openspec instructions` | 获取下一步操作指引 |
| `openspec config` | 查看/修改配置 |

## 与 Superpowers 技能的关系

OpenSpec 管理**规范文档**，Superpowers 管理**开发流程**。两者协同工作：

```
/opsx:propose <名称>        ← 创建规范骨架
    ↓
brainstorming 技能          ← 细化需求、审批设计
    ↓
writing-plans 技能          ← 生成实现计划
    ↓
/opsx:apply                 ← 按 tasks.md 实现
    ↓
test-driven-development     ← 红绿重构循环
    ↓
requesting-code-review      ← 审查变更
    ↓
/opsx:sync                  ← 同步规范
    ↓
/opsx:archive               ← 归档变更
    ↓
finishing-a-development-branch ← 分支收尾
```

## 安装与更新

```bash
# 首次安装（全局）
npm install -g @fission-ai/openspec

# 或使用项目本地安装
npm install

# 初始化（为 Qoder 生成指令文件）
npm run openspec:init

# 升级后更新指令文件
npm update @fission-ai/openspec
npm run openspec:update
```
