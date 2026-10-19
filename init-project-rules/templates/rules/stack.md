本文件是分层规则的形状，不是成品。写入目标仓库前删掉这一段说明，扩展名用 `.mdc`，文件名用该层职责。只保留一个代码例子，例子必须来自目标仓库；没有合适例子就删掉代码块。

占位符：`{{STACK_DESCRIPTION}}` 选择器中的一句话；`{{GLOBS}}` 该层真实文件，多个用逗号分隔；`{{STACK_TITLE}}` 正文标题；`{{STACK_BODY}}` 管什么、代码放哪、不允许的跨层调用；`{{EXAMPLE}}` 目标仓库里的短例子。

```markdown
---
description: {{STACK_DESCRIPTION}}
globs: {{GLOBS}}
alwaysApply: false
---

# {{STACK_TITLE}}

{{STACK_BODY}}
```
