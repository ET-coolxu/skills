# 项目规则

{{PROJECT_NAME}}。{{STACK}}

## 红线

{{RED_LINES}}

## 先加载 skill，再动手

skill 在 `.agents/skills/`（项目根，一层目录）。目录每轮只注入名称和 description，任务命中就先用 `skill` 加载正文：

- 新需求或功能 → `agent-prompts`：先出 `docs/需求/需求V{n}.md`，确认后再整理提示词；没说实现就不写代码
- 优化、架构，且不改本期范围 → `agent-prompts`
- 需求文档确认后要改本期范围 → `iteration`
- 写代码、提交 → `project`
{{LAYER_ROUTES}}

## 文档依据

{{AUTHORITIES}}

范围、依据和工程约定的正文在 `.agents/skills/project/SKILL.md`。本文件只保留红线和路由。
