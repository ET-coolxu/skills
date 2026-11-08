# 开发分支规范

## 分支模型

```
main   ← 稳定可用，只接收验证通过的合并
 ▲
 │ merge (只从 dev)
 │
dev    ← 集成分支，所有功能合并到这里
 ▲
 │ merge --no-ff，合并后删除 feat / fix 分支
 │
feat/* ← 单个功能，从 dev 切出
fix/*  ← 缺陷修复，从 dev 切出
```

## 规则

| 分支 | 用途 | 从哪切 | 合到哪 | 删不删 |
| --- | --- | --- | --- | --- |
| `main` | 稳定版本，随时可用可回滚 | — | — | 否 |
| `dev` | 集成分支，日常开发主线 | `main` | `main`（阶段完成后） | 否 |
| `feat/<功能>` | 单个功能或开发阶段 | `dev` | `dev` | 合并后删除 |
| `fix/<问题>` | 缺陷修复 | `dev` | `dev` | 合并后删除 |

## 工作流

```powershell
git checkout dev
git checkout -b feat/<模块>-<简述>

# ... 开发、提交 ...

git checkout dev
git merge --no-ff feat/<模块>-<简述>
git branch -d feat/<模块>-<简述>

# 一个阶段完成，dev 合并到 main
git checkout main
git merge --no-ff dev
```

## 命名约定

- 功能分支：`feat/<模块>-<简述>`
- 修复分支：`fix/<简述>`
- 提交信息：`<type>: <简体中文简述>`。`type` 取 `feat` / `fix` / `docs` / `chore` / `refactor`，保持英文关键字
- 标题和正文都用简体中文。代为 `git commit` 时同样遵守

## 注意

- 不提交 `.gitignore` 已排除的文件。
{{GIT_NOTES}}
