本文件是分层 skill 的形状，不是成品。写入目标仓库前删掉这一段说明。目录名用该层职责，`name` 与目录名一致。只保留一个代码例子，例子必须来自目标仓库；没有合适例子就删掉代码块。

占位符：`{{LAYER_NAME}}` kebab-case 目录名；`{{LAYER_FILES}}` 该层真实文件；`{{LAYER_SUMMARY}}` description 里的一句约束；`{{STACK_TITLE}}` 正文标题；`{{STACK_BODY}}` 管什么、代码放哪、不允许的跨层调用；`{{EXAMPLE}}` 目标仓库里的短例子。

```markdown
---
name: {{LAYER_NAME}}
description: >-
  编写或修改 {{LAYER_FILES}} 时使用。{{LAYER_SUMMARY}}
---

# {{STACK_TITLE}}

{{STACK_BODY}}
```
