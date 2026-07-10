# dev-memory — Windsurf 适配

## 安装

### 方式一：.windsurfrules 文件

在项目根目录创建或编辑 `.windsurfrules` 文件，追加以下内容：

```
## 开发记忆

本项目使用 dev-memory 管理开发记忆。
- 记忆文件: docs/dev-memory/DEV_MEMORY.md
- 维护指令: docs/dev-memory/SKILL.md
- 安装指引: https://github.com/HJ2916/dev-memory

请在对话开始时读取 DEV_MEMORY.md 的"当前状态概览"了解项目现状。
请在对话结束时按 SKILL.md 的流程更新记忆。
```

### 方式二：Windsurf Rules 配置

在 Windsurf IDE 中：Settings → AI Rules → Workspace Rules，粘贴上述内容。

## 触发机制

Windsurf 的 Cascade 系统自动加载 `.windsurfrules` 文件。支持 Global AI Rules 和 Workspace AI Rules 两个层级，建议在 Workspace 层级配置（随项目走）。

## 特殊说明

- Windsurf 的 Rules + Workflows 体系中，dev-memory 属于 Rules 层
- Global Rules 适合安装一次全局生效，但不如 Workspace Rules 可移植
- 建议用 Workspace Rules（`.windsurfrules`），确保随项目走
