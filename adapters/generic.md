# dev-memory — 通用适配

## 适用场景

任何支持自定义指令/系统提示词的 AI 工具，不限于上述已适配的工具。

## 安装

### 方式一：粘贴到自定义指令

将 `SKILL.md` 的全部内容粘贴到 AI 工具的自定义指令/系统提示词配置中。

### 方式二：项目内引用文件

在项目根目录创建 AI 工具支持的指令文件（如 `ai-instructions.md`、`prompt.md` 等），追加：

```markdown
## 开发记忆

本项目使用 dev-memory 管理开发记忆。
- 记忆文件: docs/dev-memory/DEV_MEMORY.md
- 维护指令: docs/dev-memory/SKILL.md
- 安装指引: https://github.com/HJ2916/dev-memory

请在对话开始时读取 DEV_MEMORY.md 的"当前状态概览"了解项目现状。
请在对话结束时按 SKILL.md 的流程更新记忆。
```

### 方式三：对话中手动引用

在对话开始时告诉 AI：

```
请读取 docs/dev-memory/SKILL.md 并按照其中的流程维护项目记忆。
```

## 特殊说明

- dev-memory 的核心是纯 Markdown 指令，任何能读取 Markdown 的 AI 都能执行
- 如果工具不支持自动加载自定义指令，可以用方式三手动触发
- 首次使用时，AI 需要读取完整的 SKILL.md 以理解流程
- 后续使用时，AI 只需读取 DEV_MEMORY.md 的 metadata 和概览即可
