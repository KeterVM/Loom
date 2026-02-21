# loom — Claude Code 工作指南

loom 是一套「纯文件夹 + 约定文件结构」的个人产品开发工作流，用 AI Agent Skills 串联构思 → 设计 → 开发 → 测试全流程。

## 项目结构

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

## Skill 文件约定

- `SKILL.md` — 阶段编排：pre-checks、执行步骤、checkpoint 规则、completion
- `references/*.md` — 原子执行单元，header 声明 Layer 和 Output 类型
- 输出类型：`transient`（中间结果，不落文件）/ `persistent`（通过 infra/write-file 写入）

## Workflow

```
/ideation → /design → /architecture → /plan → /build（循环）
```

- `/plan` 读 `architecture.md`，生成 `.loom/features/[NN]-[name].md`
- `/build` 每次执行一个 feature，完成后停止等待
- `/feature:spec` 新建后续 feature，再跑 `/build`

## 工作方式

- 遇到设计决策先问，不要自己假设
- 改动 Skill 文件后告诉我，等确认再继续
- 随时可以说「这里不对」，讨论后再改
