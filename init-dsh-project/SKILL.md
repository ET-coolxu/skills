---
name: init-dsh-project
description: >-
  按当前仓库的真实技术栈和需求文档，生成 DSH 会加载的 AGENTS.md、.agents/skills
  （project、iteration、agent-prompts 和分层 skill）、docs/agent 提示词库，以及 BRANCHING.md 分支与提交规范。
  在用户要求初始化项目、初始化规则、新项目、补齐 skill，
  或按 init-dsh-project 给当前项目建协作约定时使用。
---

# 初始化 DSH 项目

在**当前工作区**生成 DSH 会读到的协作约定和提示词库。模板在本 `SKILL.md` 同级的 `templates/`。把占位符换成从目标仓库读到的事实，不要把模板原文、占位符或其它项目的约定写进目标仓库。

DSH 在会话基线里注入项目根的 `AGENTS.md`（`@deepseek-ai/dsh-agent-instructions`，候选文件是 `AGENTS.md` / `CLAUDE.md`）。skill 只把 `name` 和 `description` 放进目录，正文要模型调用 `skill` 才加载。所以短路由写在 `AGENTS.md`，长约定写在 `.agents/skills/`。

## 和 Cursor 规则的对应

| Cursor 初始化产物 | 本 skill 写出 | 原因 |
| --- | --- | --- |
| `.cursor/rules/project.mdc` | `AGENTS.md` 的路由 + `.agents/skills/project/SKILL.md` | 常驻的是 `AGENTS.md`，范围正文按需加载 |
| `.cursor/rules/iteration.mdc` | `.agents/skills/iteration/SKILL.md` | 换版流程不常驻 |
| `.cursor/rules/agent-prompts.mdc` | `AGENTS.md` 里的路由行 + `.agents/skills/agent-prompts/` | 触发写在常驻文件，流程放 skill |
| `.cursor/rules/<分层>.mdc`（`globs`） | `.agents/skills/<分层>/SKILL.md` | skill 没有 `globs`，`description` 写明何时加载 |
| `.cursor/skills/agent-prompts/` | `.agents/skills/agent-prompts/` | DSH 扫描项目根下的 `.agents/skills` |

不要写 `.cursor/rules`、`.cursor/skills`、`CLAUDE.md`。不要写 `alwaysApply`、`globs`。默认不写 `.dsh/skills`（rank 100，只有 DSH 扫描）。写到 `.agents/skills`（rank 200）：DSH 会读；别的工具读不读，取决于那个工具，不要另写一份专供某个工具的副本。

## 怎么用

在新项目里任选一种：

- 把本目录复制到目标项目的 `.agents/skills/init-dsh-project/`，然后说「初始化项目」。DSH 只扫描 `.agents/skills` 的下一层；本目录放在仓库根时不会出现在 skill 目录里。
- 或直接说：读取本 `SKILL.md` 的路径，按它给当前项目初始化。

不要把来源项目的 `AGENTS.md` 或 `.agents/skills` 复制过去。

## 已有约定时

目标仓库已有 `AGENTS.md` 或承担范围职责的 skill 时，先读现有文件，只补缺失。用户明确要求重做时才整份替换。

已有范围 skill 不叫 `project` 时，沿用它的目录名。后面写出的 `AGENTS.md`、`iteration`、提示词模板和架构说明都指向这个名字，不要再平行建一个 `project`。

目标仓库里的 `.cursor/rules` 保持不动。需要的事实从中摘出来写进 `AGENTS.md` 和 skill；用户没要求迁移时，不要改那些 `.mdc`。

## 步骤

1. 读目标仓库：README、依赖清单、`docs/`、已有 `AGENTS.md`、已有 `.agents/skills`、入口配置，以及 `BRANCHING.md`（或其它分支文档）、`.gitignore`、`.gitattributes`、近期提交标题。记下项目名、技术栈、当前需求文档、仓库内已有的 API 或协议文档、已经存在的工程边界和分支模型。项目根是最近的含 `.git` 的目录。单个产品时，`AGENTS.md`、`.agents/skills` 和 `BRANCHING.md` 都放这一层。仓库若是多个独立子项目，项目名用工作区，公共文件仍放仓库根，子项目约定按「多个独立子项目」放进各自目录。不要把其中一个子项目写成整个仓库。
2. 读不到的事实只问一轮：项目名、当前范围文档、必须守住的工程边界。仓库里已经有的不要问。不要为了补依据去问仓库外文档放在哪。
3. 按下面的清单写文件。规则用简体中文，一件事一条。行尾用 LF。
4. 回复里列出写了哪些文件、每个 skill 约束什么，以及仍然空着的事实。仍然空着的事实只写初始化还缺、且仓库里找不到的项。仓库外文件不在仓库内，不算缺口。

## skill 格式

每个 skill 是 `.agents/skills/<name>/SKILL.md`，只认这一层，不要再套分类目录。

- frontmatter 必填 `name` 和 `description`。`name` 用 kebab-case，且与目录名一致。缺了这两项，DSH 会丢掉整个 skill，目录里看不到。
- `description` 写清何时使用。目录只注入这一句，并截断到大约 500 字。
- 正文不要依赖常驻注入。`AGENTS.md` 里要有对应路由，让模型在命中任务时先加载正文。

## 写入清单

| 目标 | 模板 | 何时写 |
| --- | --- | --- |
| `AGENTS.md` | `templates/AGENTS.md` | 总是。只放红线和路由，正文大约 40 行 |
| `.agents/skills/project/SKILL.md` | `templates/skills/project/SKILL.md` | 总是。已有范围 skill 则改指向，不新建 |
| `.agents/skills/iteration/SKILL.md` | `templates/skills/iteration/SKILL.md` | 总是 |
| `.agents/skills/agent-prompts/` | `templates/skills/agent-prompts/` | 总是 |
| `.agents/skills/<分层>/SKILL.md` | `templates/skills/stack/SKILL.md` | 单个产品仓库，且已有清晰分层时，一层一个。多个独立子项目时改放到该子项目目录 |
| `docs/agent/新需求.md`、`优化.md`、`架构.md` | `templates/agent/` | 总是 |
| `docs/agent/use Prompt/README.md` | `templates/agent/use-prompt-readme.md` | 总是 |
| `docs/架构/项目架构.md` | `templates/docs/架构.md` | 文件不存在或为空时新建；已有内容则只补缺失的 skill 表和 Git 节 |
| `BRANCHING.md` | `templates/BRANCHING.md` | 仓库还没有分支文档时写到根目录。已有则不覆盖 |
| `README.md` | 无 | 已有正文不改写。只补协作入口：`AGENTS.md`、`.agents/skills/`、`docs/架构/项目架构.md`、`docs/agent/`、`BRANCHING.md` |

## 填空

| 占位符 | 换成 |
| --- | --- |
| `{{PROJECT_NAME}}` | 仓库或产品名 |
| `{{STACK}}` | 一句话技术栈，含运行环境与开发机的差别（若有） |
| `{{SCOPE_DOC}}` | 当前需求文件的仓库相对路径 |
| `{{SCOPE_LABEL}}` | 该文档的短标题 |
| `{{AUTHORITIES}}` | 依据列表。只列仓库里存在的文档，每条一行 `- ` |
| `{{RED_LINES}}` | 不加载 skill 也会踩坑的事实，每条一行 `- `。没有就写「写代码、改需求或提交前，先加载 `.agents/skills/project/SKILL.md`」 |
| `{{LAYER_ROUTES}}` | `AGENTS.md` 里的分层路由，每条一行 `- 编辑 <路径> → \`<skill 名>\``。没有分层就删掉这一行 |
| `{{STABLE_CONSTRAINTS}}` | 跨版本不变的工程约定，每条一行 `- ` |
| `{{INVARIANTS}}` | 除分支和提交以外的不变约定，写成以句号结尾的一句。没有就删掉「工程约定保持不动」那一行 |
| `{{LAYER_SKILLS}}` | 本次实际写出的分层 skill 路径；没有则写「暂无」 |
| `{{PROJECT_SOURCES}}` | 整理提示词时要读的路径，每条一行 `- ` |
| `{{FALLBACK_SCOPE}}` | 还没有已确认需求文档时，用来界定范围的设计文档。没有这份文档就删掉所在句子 |
| `{{TREE}}` | 只含已有目录，外加本次会创建的 `docs/agent/`、`.agents/skills/`、`AGENTS.md`、`BRANCHING.md` |
| `{{SKILL_ROWS}}` | 分层 skill 的表格行；没有就删掉这一行 |
| `{{GIT_NOTES}}` | 仓库特有的 Git 注意，每条一行 `- `。没有就删掉这一行。不要写里程碑分支表 |
| `{{LAYER_NAME}}` 等 | 只在写出分层 skill 时替换，含义见 `templates/skills/stack/SKILL.md` 顶部说明 |

有当前需求文档时，删掉 `project` 模板里「没有单独的需求文档时」那一句。没有需求文档时，保留那一句，`{{SCOPE_DOC}}` 写成「待补充」，不要编一份需求。

模板正文里写死的 `docs/需求/需求V{n}.md`，只在目标仓库的需求真的放在这个目录时保留。需求在子项目目录或其它路径时，`iteration`、`agent-prompts` 和三份提示词模板都改成那条真实路径，删掉模板里的 `docs/需求/`。

`{{AUTHORITIES}}` 只列目标仓库里真实存在的文件，每条用仓库相对路径。至少包括：

- 产品行为以仓库里实际存在的需求文档为准，路径按读到的相对路径写。需求本来就在 `docs/需求/`，或仓库还没有任何需求文档时，才写「产品行为以 `docs/需求/` 为准」。需求文件已经在别的路径时，不要再平行写一个不存在的 `docs/需求/`
- 多个独立子项目时，写「每个子项目的产品行为以该子项目目录里的需求文档为准」，并列出已经存在的那几份。不要把其中一份写成整个仓库的唯一需求
- 目录和构建以 `docs/架构/项目架构.md` 为准
- 仓库里若有第三方 API、协议或本地文档副本，写明该副本的仓库相对路径，并写「文档没写的能力不要当成官方 API」。没有这份副本时，整条都不写
- 分支与提交以 `BRANCHING.md` 为准

## 仓库外路径

需求、注释、README 里出现的绝对路径，只要不在目标仓库内，就不是本仓库的依据。

- 依据、红线、稳定约定、架构说明、提示词模板和完成回复都只写仓库相对路径。
- 不要写「本仓库没有某某本地副本」。不要写「需求里提到的某路径不在本仓库，没有写成依据」。
- 不要把这种句子放进 `AGENTS.md`、skill、架构说明，也不要放进完成回复的「仍然空着的事实」。
- 有仓库内副本时，只写那份副本的相对路径。

反例：需求文档引用了仓库外的方案文件，初始化结果却写成「本仓库没有 DSH Host API 的本地副本。需求里提到的该绝对路径也不在本仓库，没有写成依据。」这是错的。仓库外路径不进入产物，也不进入完成回复。

`{{PROJECT_SOURCES}}` 至少包括：已确认的需求文档（若有）、`AGENTS.md`、`docs/架构/项目架构.md`、`BRANCHING.md`、范围 skill。仓库里存在的分层 skill 和入口代码也列上。不要列不存在的文件。

`{{STABLE_CONSTRAINTS}}` 只写仓库里已经成立的事实。简体中文、分支模型和提交格式已经写在模板里，不要再写一遍。某一期才禁止的功能写进范围 skill 的「范围」，不写进 `docs/agent/` 的类型模板。

`{{INVARIANTS}}` 不要再重复分支模型和提交格式；模板末尾已经写了它们以 `BRANCHING.md` 为准。

## Git

分支和提交说明是初始化的一部分，不靠 `{{STABLE_CONSTRAINTS}}` 顺便带上。

- 目标仓库没有分支文档时，用 `templates/BRANCHING.md` 写到仓库根。默认是 `main` / `dev` / `feat/*` / `fix/*`，提交 `<type>: <简体中文简述>`。
- 已有 `BRANCHING.md` 或其它分支文档时不覆盖。范围 skill 的「Git」、架构说明和提示词模板改成跟那份文档一致，不要留着模板里的 `main` / `dev`。
- 近期提交标题已经是另一种格式时，以仓库为准，改掉模板里的提交那一句。
- `.gitattributes` 声明 `eol=lf` 时，把「行尾一律 LF」写入 `{{RED_LINES}}` 和 `{{STABLE_CONSTRAINTS}}`。
- 切分支会改到正在运行的程序时，把这件事写入 `{{RED_LINES}}` 和 `{{GIT_NOTES}}`。
- 不要从别的仓库抄里程碑分支名，也不要在 `BRANCHING.md` 里预写尚未存在的阶段表。

`{{RED_LINES}}` 不要把范围 skill 的正文再抄进 `AGENTS.md`。项目指令链默认大约 64KiB。同一条链里，更具体的嵌套 `AGENTS.md` 优先保留；根上这份过长时，旁边还有更具体的文件就会整份被丢掉，只有它自己时则从末尾截断。

仓库里已有实测边界时，在范围 skill 末尾加「已知边界」，每条写清现象和正确用法。没有实测就不要写这一节。已有实测文档目录或测试说明时，在 `iteration` 的「改哪里」表加一行指向它们。

## 分层 skill

单个产品仓库：一层一个，放在仓库根 `.agents/skills/`，目录名用该层职责，例如 `client`、`host`、`api`。`description` 写明要编辑的真实路径。没有第二层就不要凑一个空 skill。

多个独立子项目：不要在仓库根按子项目追加 skill，也不要把目录名改成 `插件名-host`。分层 skill 放进该子项目的 `.agents/skills/<分层>/`，目录名仍用 `host`、`companion` 这类职责名。详见「多个独立子项目」。

每个包含：这条 skill 管什么、代码放哪、不允许的跨层调用，以及一个来自本仓库的短例子。例子不要从其它项目搬。

单个产品仓库时，`iteration` 的「改哪里」表列出这些真实路径，`AGENTS.md` 的路由列表加上对应的一行。多个独立子项目时，这些路径和路由写在该子项目的 `AGENTS.md`，仓库根只留一行入口。

## 多个独立子项目

仓库是多个独立子项目（每个子目录一个产品或插件）时，工作区根只放公共约定：

- `AGENTS.md`、`BRANCHING.md`、`docs/架构/`、`docs/agent/`
- `.agents/skills/project`、`iteration`、`agent-prompts`

根上的 `.agents/skills/` 不随子项目增加。子项目再多，这里也只保留这几项公共 skill。

每个已经存在的子项目自己负责：

- `<子项目>/AGENTS.md`：该子项目的红线和路由
- `<子项目>/.agents/skills/project/SKILL.md`：该子项目的范围、实现约定、已知边界
- `<子项目>/.agents/skills/<分层>/SKILL.md`：分层名用职责，例如 `host`、`companion`

DSH 每轮只扫描仓库根 `.agents/skills` 的下一层，子项目里的 skill 不会出现在这层目录。根 `AGENTS.md` 对每个子项目只写一行：编辑该目录时先读它的 `AGENTS.md`。子项目 `AGENTS.md` 写明要读取的 `SKILL.md` 路径。不要把子项目 skill 复制到仓库根。

## 架构说明

新建时按 `templates/docs/架构.md` 填。目录树只列仓库里已有的路径，外加本次要创建的 `docs/agent/`、`.agents/skills/`、`AGENTS.md` 和 `BRANCHING.md`。不要提前发明尚未存在的业务目录。没有需求文档时，删掉首句里的链接，改成「当前还没有需求文档」。

已有架构说明时，不要覆盖正文。缺 skill 表就补上本次 skill；缺 Git 节就按模板补上。

## 完成之后

- 需求换版：目标仓库里的 `.agents/skills/iteration/SKILL.md`
- 整理提示词：目标仓库里的 `.agents/skills/agent-prompts/SKILL.md`
- 类型模板要改：先给出「提示词库建议」，用户确认后再写

## 例子

输入：仓库有 `package.json`（React + Vite）和 `docs/需求/需求V1.md`，没有 `AGENTS.md`，界面文件在 `src/**/*.tsx`，没有第二层。

写出：

- `AGENTS.md` 把新需求指到 `agent-prompts`，把写代码指到 `project`，范围指向 `docs/需求/需求V1.md`
- 还没有分支文档时写出 `BRANCHING.md`；`project` skill 的「Git」和三份提示词模板指向它
- `.agents/skills/project/SKILL.md` 的技术栈写成 React + TypeScript + Vite
- `.agents/skills/iteration/SKILL.md`、`.agents/skills/agent-prompts/`
- `docs/agent/` 三份模板的前置约束只包含从本仓库读到的稳定约定
- 架构说明的 skill 表只列出实际写过的 skill
- 不写分层 skill

输入若是一个已分成 `lib/client.js` 与 `lib/index.js` 的仓库：再写两个分层 skill，`description` 分别写明这两个文件，`AGENTS.md` 各加一行路由。内容只写这个仓库已经存在的约定。

不写出：别的项目的产品名、里程碑范围、热更路径或窗口参数。不写出仓库外绝对路径，也不写出「本仓库没有某份本地副本」或「某外部文件没写成依据」。
