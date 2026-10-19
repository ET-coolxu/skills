---
name: init-project-rules
description: >-
  Initializes Cursor rules, an architecture note, and the docs/agent prompt library
  for the current repository from its real stack and requirement docs. Use when the
  user asks to initialize project rules, 初始化规则, 新项目规则, 补齐 rules,
  or to apply docs/skills/init-project-rules in another project.
---

# 初始化项目规则

在**当前工作区**生成协作规则和提示词库。模板在本 `SKILL.md` 同级的 `templates/`。把占位符换成从目标仓库读到的事实，不要把模板原文、占位符或其它项目的约定写进目标仓库。

## 怎么用

在新项目里任选一种：

- 把本目录复制到目标项目的 `.cursor/skills/init-project-rules/`，然后说「初始化项目规则」。
- 或直接说：读取本 `SKILL.md` 的路径，按它给当前项目初始化规则。

不要把来源项目的 `.cursor/rules` 复制过去。

## 已有规则时

目标仓库已有 `.cursor/rules/project.mdc` 时，先读现有规则，只补缺失文件。用户明确要求重做时才整份替换。

## 步骤

1. 读目标仓库：README、依赖清单、`docs/`、已有 `.cursor/rules`、入口配置。记下项目名、技术栈、当前需求文档、API 或协议文档、已经存在的工程边界。
2. 读不到的事实只问一轮：项目名、当前范围文档、外部 API 文档放哪、必须守住的工程边界。仓库里已经有的不要问。
3. 按下面的清单写文件。规则用简体中文，一件事一条，正文 50 行以内。
4. 回复里列出写了哪些文件、每条规则约束什么，以及仍然空着的事实。

## 写入清单

| 目标 | 模板 | 何时写 |
| --- | --- | --- |
| `.cursor/rules/project.mdc` | `templates/rules/project.md` | 总是 |
| `.cursor/rules/iteration.mdc` | `templates/rules/iteration.md` | 总是 |
| `.cursor/rules/agent-prompts.mdc` | `templates/rules/agent-prompts.md` | 总是 |
| `.cursor/rules/<分层>.mdc` | `templates/rules/stack.md` | 仓库里已有清晰分层时，一层一条 |
| `docs/agent/新需求.md`、`优化.md`、`架构.md` | `templates/agent/` | 总是 |
| `docs/agent/use Prompt/README.md` | `templates/agent/use-prompt-readme.md` | 总是 |
| `.cursor/skills/agent-prompts/` | `templates/skills/agent-prompts/` | 总是，安装后供整理提示词使用 |
| `docs/架构/项目架构.md` | `templates/docs/架构.md` | 文件不存在或为空时新建；已有内容则只补协作规则表 |
| `README.md` | 无 | 已有正文不改写，只补规则和提示词库的入口 |

`templates/rules/` 里的文件写入目标仓库时扩展名改为 `.mdc`。

## 填空

| 占位符 | 换成 |
| --- | --- |
| `{{PROJECT_NAME}}` | 仓库或产品名 |
| `{{STACK}}` | 一句话技术栈，含运行环境与开发机的差别（若有） |
| `{{SCOPE_DOC}}` | 当前需求文件的仓库相对路径 |
| `{{SCOPE_LABEL}}` | 该文档的短标题 |
| `{{AUTHORITIES}}` | 依据列表。只列仓库里存在的文档 |
| `{{STABLE_CONSTRAINTS}}` | 跨版本不变的工程约定，每条一行 `- ` |
| `{{INVARIANTS}}` | 同一批约定，写成一句 |
| `{{SCOPED_RULES}}` | 本次实际写出的分层规则文件名；没有则写「暂无」 |
| `{{PROJECT_SOURCES}}` | 整理提示词时要读的路径，每条一行 `- ` |
| `{{TREE}}` | 只含已有目录和本次会创建的 `docs/agent/`、`.cursor/rules/` |
| `{{RULE_ROWS}}` | 分层规则的表格行；没有分层规则就删掉这一行 |
| `{{STACK_DESCRIPTION}}` 等 | 只在写出分层规则时替换，含义见 `templates/rules/stack.md` 顶部说明 |

有当前需求文档时，删掉 `project.md` 里「没有单独的需求文档时」那一句。没有需求文档时，保留那一句，`{{SCOPE_DOC}}` 写成「待补充」，不要编一份需求。

`{{AUTHORITIES}}` 至少包括：

- 产品行为以 `docs/需求/` 为准（目录还不存在就先写这句话，不要虚构文件）
- 目录和构建以 `docs/架构/项目架构.md` 为准
- 仓库里若有第三方 API、协议或本地文档副本，写明路径，并写「文档没写的能力不要当成官方 API」

`{{STABLE_CONSTRAINTS}}` 只写仓库里已经成立的事实，每条以 `- ` 开头。简体中文已经写在 `project.md` 里，不要再写一遍。某一期才禁止的功能写进 `project.mdc` 的范围，不写进 `docs/agent/` 的类型模板。

## 分层规则

一层一条，文件名用该层的职责，例如 `frontend.mdc`、`api.mdc`。`globs` 对应该层真实文件。没有第二层就不要凑一条空规则。

每条包含：这条规则管什么、代码放哪、不允许的跨层调用，以及一个来自本仓库的短例子。例子不要从其它项目搬。

`iteration.md` 的「改哪里」表要列出这些真实文件名，不要留着模板里的占位符。

## 架构说明

新建时按 `templates/docs/架构.md` 填。目录树只列仓库里已有的路径，外加本次要创建的 `docs/agent/` 和 `.cursor/rules/`。不要提前发明尚未存在的业务目录。没有需求文档时，删掉首句里的链接，改成「当前还没有需求文档」。

已有架构说明时，不要覆盖正文。把本次规则补进「协作规则」表；没有该表就在文末加上。

## 完成之后

- 需求换版：目标仓库里的 `.cursor/rules/iteration.mdc`
- 整理提示词：目标仓库里的 `.cursor/skills/agent-prompts/SKILL.md`
- 类型模板要改：先给出「提示词库建议」，用户确认后再写

## 例子

输入：仓库有 `package.json`（React + Vite）和 `docs/需求/需求V1.md`，没有 `.cursor/rules`，界面文件在 `src/**/*.tsx`。

写出：

- `project.mdc` 的范围指向 `docs/需求/需求V1.md`，技术栈写成 React + TypeScript + Vite
- `iteration.mdc`、`agent-prompts.mdc`
- 一条 `frontend.mdc`，`globs` 为 `src/**/*.{tsx,css}`，内容只写这个仓库已经存在的界面约定
- `docs/agent/` 三份模板的前置约束只包含从本仓库读到的稳定约定
- `.cursor/skills/agent-prompts/`
- 架构说明的规则表只列出实际写过的规则

不写出：别的项目的窗口参数、关键词、preload 路径或产品名。
