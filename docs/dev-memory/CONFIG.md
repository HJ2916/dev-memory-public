# dev-memory 项目记忆配置

> 本项目使用 [dev-memory](https://github.com/HJ2916/dev-memory) 管理开发记忆。
> 记忆归项目所有，不绑任何平台，随 Git 版本管理。

## 安装 Skill

dev-memory 是一个纯 Markdown 指令文件，无脚本无依赖。根据你使用的 AI 工具选择安装方式：

| 工具 | 安装方式 |
|------|---------|
| **TRAE** | 复制 SKILL.md 到 `~/.trae-cn/skills/dev-memory/SKILL.md` |
| **Claude Code** | 复制到 `~/.claude/commands/dev-memory.md`，或在项目 CLAUDE.md 中引用 |
| **Codex CLI** | 在项目 `AGENTS.md` 中追加引用 |
| **Cursor** | 复制到 `.cursor/rules/dev-memory.mdc`（需添加 MDC 元数据头） |
| **GitHub Copilot** | 在 `.github/copilot-instructions.md` 中追加引用 |
| **Windsurf** | 在 `.windsurfrules` 中追加引用 |
| **Cline** | 在 `.clinerules` 中追加引用，或复制到 `.clinerules/dev-memory.md` |
| **CodeBuddy** | 复制到 `.codebuddy/rules/dev-memory.md` |
| **OpenCode / MiMoCode** | 在项目 `AGENTS.md` 中追加引用（同 Codex） |
| **其他工具** | 将 SKILL.md 内容粘贴到工具的自定义指令配置中 |

下载地址：https://github.com/HJ2916/dev-memory

详细适配说明见 `adapters/` 目录。

## 隐私设置

本项目的记忆文件（DEV_MEMORY.md 和 archive/）默认 **不纳入 Git**，保护开发隐私。

| 仓库类型 | 推荐设置 | 原因 |
|---------|---------|------|
| 公开仓库 | 记忆文件 gitignore（默认） | 保护开发隐私，避免泄露内部决策和踩坑细节 |
| 私有仓库 | 记忆文件纳入 Git | 便于多设备同步、团队协作、项目交接 |
| 个人本地 | 任意 | 无远程仓库，不涉及隐私问题 |

**修改方式**：编辑项目根目录的 `.gitignore`，注释/取消注释以下行：

```gitignore
# dev-memory 记忆文件
# 如需追踪记忆（如私有仓库），注释掉以下两行
docs/dev-memory/DEV_MEMORY.md
docs/dev-memory/archive/
```

## 记忆文件说明

| 文件 | 说明 | 是否纳入 Git |
|------|------|-------------|
| `CONFIG.md` | 本文件，配置和引导 | 始终纳入 |
| `DEV_MEMORY.md` | 主记忆文件 | 默认不纳入 |
| `archive/*.md` | 按周归档 | 默认不纳入 |
| `SKILL.md` | 维护指令（如项目内放置） | 始终纳入 |

## 快速开始

1. 安装 Skill（见上方"安装 Skill"章节）
2. 开始正常开发，AI 会在对话结束时自动提取和保存记忆
3. 查看记忆：打开 `docs/dev-memory/DEV_MEMORY.md`
4. 如需手动触发：告诉 AI "保存记忆"或"更新项目记忆"
