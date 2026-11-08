---
name: project
description: >-
  {{PROJECT_NAME}}的范围、文档依据和工程约定。在本仓库写代码、改需求或提交之前读取正文。
---

# {{PROJECT_NAME}}

{{STACK}}

## 范围

当前只做 `{{SCOPE_DOC}}`。文档里「本期不做」的能力，不要设计、不要加入口、不要写存储。

没有单独的需求文档时，不要自行扩大范围。用户给出范围后，先写入 `docs/需求/`，再按 `.agents/skills/iteration/SKILL.md` 更新本 skill 和 `AGENTS.md`。

需求有变时，先改需求文档，再按 `.agents/skills/iteration/SKILL.md` 同步 skill 和说明，然后改代码。

## 依据

{{AUTHORITIES}}

## Git

- 分支以 `BRANCHING.md` 为准：功能 `feat/*`、修复 `fix/*`，从 `dev` 切出，`--no-ff` 合回 `dev` 后删除；阶段完成再由 `dev` 合入 `main`。
- 代为 `git commit` 时，标题和正文用简体中文，格式为 `<type>: <简述>`。`type` 取 `feat` / `fix` / `docs` / `chore` / `refactor`。
- 不提交 `.gitignore` 已排除的文件。
{{GIT_NOTES}}

## 协作

- 用户能看到的文案用简体中文。
{{STABLE_CONSTRAINTS}}
