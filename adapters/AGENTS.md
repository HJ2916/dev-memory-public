# dev-memory — Codex CLI / OpenCode / MiMoCode 适配

这三个工具都使用 AGENTS.md 作为自定义指令标准。

## 安装

在项目根目录创建或编辑 `AGENTS.md`，追加以下内容：

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

AGENTS.md 是 OpenAI + Google + Cursor + Sourcegraph 联合推出的标准，GitHub 上超过 6 万个开源项目在用。Codex CLI、OpenCode、MiMoCode 会在每次会话启动时自动读取项目根目录的 AGENTS.md。

## 各工具差异

| 工具 | AGENTS.md 位置 | 全局配置 |
|------|---------------|---------|
| Codex CLI | 项目根目录 | `~/.codex/AGENTS.md` |
| OpenCode | 项目根目录 | `~/.opencode/AGENTS.md` |
| MiMoCode | 项目根目录 | `~/.mimo/AGENTS.md` |

如需全局生效，将上述引用内容也追加到全局 AGENTS.md 中。

## 特殊说明

- AGENTS.md 建议控制在 200 行以内（"地图而非手册"原则）
- dev-memory 的引用只需 5-8 行，不占用过多篇幅
- MiMoCode 基于 OpenCode 开发，适配方式完全一致
