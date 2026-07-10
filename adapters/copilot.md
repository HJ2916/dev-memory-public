# dev-memory — GitHub Copilot 适配

## 安装

在项目根目录创建或编辑 `.github/copilot-instructions.md`，追加以下内容：

```markdown
## 开发记忆

本项目使用 dev-memory 管理开发记忆。

- 记忆文件: docs/dev-memory/DEV_MEMORY.md
- 维护指令: docs/dev-memory/SKILL.md
- 安装指引: https://github.com/HJ2916/dev-memory

请在对话开始时读取 DEV_MEMORY.md 的"当前状态概览"了解项目现状。
请在对话结束时按 SKILL.md 的流程更新记忆。
```

然后将 SKILL.md 复制到 `docs/dev-memory/SKILL.md`。

## 触发机制

GitHub Copilot 会自动读取项目 `.github/copilot-instructions.md` 文件，将其内容注入到 AI 的上下文中。该文件在每次 Copilot 交互时生效。

## 特殊说明

- copilot-instructions.md 已普遍可用（GA），支持 Copilot Pro 用户
- 该文件同时影响 Copilot Chat 和 Copilot 代码评审
- dev-memory 的引用不会干扰其他 Copilot 指令
- 也支持 `.github/instructions/*.instructions.md` 格式的分文件指令
