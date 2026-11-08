---
name: iteration
description: >-
  需求文档确认为 docs/需求/需求V1、V2、V3 之后，同一轮同步 project、分层 skill、架构说明和 README。
  在新增已确认的需求版本、修改本期要做或本期不做、或发现 skill 仍在禁止已经纳入本期的能力时使用。
---

# 需求迭代

新版本或范围变化落地时，同一轮把 skill 和说明改完。不要只改需求文档，也不要只改代码。

## 何时更新

出现以下任一情况就要更新：

- 新增 `docs/需求/` 下的新版本需求，或用户要求按新版本实现
- 修改现有需求的「本期要做 / 本期不做」
- 实现时发现某条 skill 仍在禁止已经纳入本期的能力

先读 `docs/需求/` 里版本号最大的 `需求V{n}.md`，以它为当前范围。新版本写到 `docs/需求/需求V{n}.md`：n 为已有最大号加 1，一份都没有则为 1。用户确认前不落盘。已确认的旧版不改写。更早版本里「不做」的条目，新需求写了要做，就不再禁止。

## 改哪里

| 变化 | 更新 |
| --- | --- |
| 当前版本、本期不做 | `.agents/skills/project/SKILL.md` 的「范围」 |
| 分层约定 | {{LAYER_SKILLS}} |
| skill 集合或路由变化 | `AGENTS.md` 的路由列表、`docs/架构/项目架构.md` 的 skill 表 |
| 目录、行为、skill 表 | `docs/架构/项目架构.md` |
| 一句话现状 | `README.md` 首段 |

工程约定保持不动：{{INVARIANTS}}

分支模型与提交说明以 `BRANCHING.md` 为准，不随需求版本改写。

`docs/agent/` 的类型模板不在上表里。更新它们要先给出「提示词库建议」，用户确认后再写。见 `.agents/skills/agent-prompts/SKILL.md`。

新主题塞不进现有 skill 时，在 `.agents/skills/<name>/SKILL.md` 另建一个（一层目录、`name` 为 kebab-case、带 `description`），并在架构说明的 skill 表加一行，同时在 `AGENTS.md` 的路由列表加一行。

## 例子

新版本要求一项过去禁止的行为时：删掉对应 skill 里的禁令，改成新行为。`project` 的当前文档改为新的需求文件，并去掉已经纳入本期的「本期不做」条目。
