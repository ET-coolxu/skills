# 提示词文件模板

每个 `docs/agent/<类型>.md`（通用类型模板）保持「可直接复制使用」的完整提示词。  
单次整理成品写入 `docs/agent/use Prompt/`，用普通 Markdown（`## 提示词正文`），不要用代码围栏包整段（见 SKILL.md）。

新增类型时，前置约束从现行 `.agents/skills/project/SKILL.md` 和已有类型模板抄稳定约定，不要从其它仓库抄。

```markdown
# <类型名>

将下方整段作为提示词使用。把 `【】` 中的内容替换为本次说明；没有的信息可留空，由 Agent 先追问。

---

你是本仓库（{{PROJECT_NAME}}）的……助手。目标：……

## 本次……

【在此粘贴……】

## 前置约束

- 先澄清假设，不臆造需求
- 改动范围最小化；遵循 `AGENTS.md`、`.agents/skills/project/SKILL.md`、`docs/需求/` 与 `docs/架构/项目架构.md`
- 用户可见文案用简体中文
- 分支遵循 `BRANCHING.md`：功能用 `feat/*`、修复用 `fix/*`，从 `dev` 切出，合回 `dev` 后删除；阶段完成再由 `dev` 合入 `main`
- 代为 `git commit` 时，标题和正文用简体中文，格式为 `<type>: <简述>`。`type` 取 `feat` / `fix` / `docs` / `chore` / `refactor`
- 不提交 `.gitignore` 已排除的文件
- 范围变化时按 `.agents/skills/iteration/SKILL.md` 同步 skill 和说明

## 工作流程

1. …
2. …
3. …
4. 若可沉淀通用经验，回复末尾用「提示词库建议」提出（确认后再改 docs/agent）

## 输出要求

- 先给结论 / 方案，再给细节
- 需要用户确认的决策单独列出
- 用简体中文回复

## 验收标准

- …
```
