# loom

一个「纯文件夹 + 约定文件结构」的个人产品开发工作流，用 AI Agent Skills 串联构思 → 设计 → 开发 → 测试全流程。

---

## 工作原理

loom 不是一个运行时工具，而是一套 **Skill 定义文件**，安装到目标项目后，由 Claude Code 读取并执行。

每个阶段对应一个 Skill 文件（`skills/[stage]/SKILL.md`），Claude 按其指令编排子任务、写入产物、管理状态。

---

## Workflow

```
Phase 1 — Foundation（只跑一次）
  /ideation      → .loom/ideation/        构思、需求、用户故事
  /design        → .loom/design/          系统设计、用户流、屏幕规格
  /architecture  → .loom/architecture/   技术架构、风险分析  ⏸ 确认 stack

Phase 2 — Planning（只跑一次）
  /plan          → .loom/features/        按功能切片生成 N 个 feature 文件  ⏸ 审查确认

Phase 3 — Build Loop（每个 feature 跑一次）
  /build         → 实现当前 feature，填写 Done 块，标记完成  ⏸ 停止等待

Phase 4 — 后续加功能（循环）
  /feature:spec  → 创建新 feature 文件  ⏸ 确认
  /build         → 执行
```

---

## Feature 文件格式

每个 feature 是 `.loom/features/[NN]-[name].md`，包含：

```markdown
---
status: pending | in-progress | done
depends-on: []
priority: 1
---

# Feature Name

## What
这个 feature 交付什么（一段话）

## Changes
哪些模块/API/表会变化

## Tasks
- [ ] Task 1
  - Acceptance: 可验证条件
- [ ] Task 2
  - Acceptance: 可验证条件

## Done
（/build 完成后填写）
```

---

## .loom/ 目录结构

```
.loom/
  ideation/       brief.md, research.md, requirements.md
  design/         system.md, flows.md, screens/
  architecture/   architecture.md
  features/       01-foundation.md, 02-auth.md, ...
  state.md
  context.md
```

---

## Skills 目录结构

```
skills/
  ideation/       构思阶段
  design/         设计阶段
  architecture/   架构阶段
  plan/           从 architecture.md 生成 feature 文件
  build/          逐个执行 feature
  feature/        手动新建 feature spec
```

每个 skill 下有 `SKILL.md`（编排逻辑）和 `references/`（原子执行步骤）。

---

## 安装到项目

把 `INSTALL.md` 交给目标项目中的 agent，让它读取并自动执行安装步骤。安装完成后运行 `/ideation` 开始。

---

## Skill 覆盖

在目标项目中创建 `.skills/stage/[stage-name].md` 可覆盖对应阶段的编排逻辑，不影响 loom 库本身。
