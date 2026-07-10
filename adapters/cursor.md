# dev-memory — Cursor 适配

## 安装

### 方式一：.cursor/rules/ 目录（推荐，Cursor 0.45+）

```bash
mkdir -p .cursor/rules
cp SKILL.md .cursor/rules/dev-memory.mdc
```

MDC 文件头部需要添加元数据：

```
---
description: dev-memory 项目级 AI 记忆系统
globs: ["**/*"]
alwaysApply: true
---
```

### 方式二：.cursorrules 文件（旧版兼容）

在项目根目录创建 `.cursorrules` 文件，追加 SKILL.md 的内容。

## 触发机制

Cursor 的 Rules 系统会根据 `globs` 和 `alwaysApply` 配置自动加载规则。设置 `alwaysApply: true` 可确保每次对话都加载 dev-memory 指令。

## 特殊说明

- Cursor 的 MDC 格式支持条件触发（通过 globs 匹配文件类型）
- dev-memory 需要始终生效，建议 `alwaysApply: true`
- 如同时使用 Cursor 的其他规则，dev-memory 可与其他规则共存
