# dev-memory — Claude Code 适配

## 安装

### 方式一：Slash Command（推荐）

```bash
mkdir -p ~/.claude/commands
cp SKILL.md ~/.claude/commands/dev-memory.md
```

安装后在 Claude Code 中输入 `/dev-memory` 即可手动触发记忆保存。

### 方式二：CLAUDE.md 引用

在项目根目录的 `CLAUDE.md` 中追加：

```markdown
## 开发记忆

本项目使用 dev-memory 管理开发记忆。请在对话结束时按 docs/dev-memory/SKILL.md 的流程更新记忆。
```

然后将 SKILL.md 复制到 `docs/dev-memory/SKILL.md`。

### 方式三：Skills 系统（Claude Code 2.1+）

Claude Code 2026 年 4 月已将 commands 合并到 skills 系统：

```bash
mkdir -p ~/.claude/skills/dev-memory
cp SKILL.md ~/.claude/skills/dev-memory/SKILL.md
```

## 触发机制

- 方式一：手动 `/dev-memory` 触发
- 方式二：CLAUDE.md 在每次对话加载时自动读取，AI 会按指令执行
- 方式三：Skills 系统自动加载（如果支持自动触发）

## 特殊说明

- Claude Code 的 Auto Memory 功能与 dev-memory 不冲突，两者互补
- Claude Code 的 CLAUDE.md 是项目级配置，dev-memory 的记忆更细粒度
- 如同时使用 Claude Code 的 Dream 模式，dev-memory 的 Step 7.5 会与其互补
