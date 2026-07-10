# dev-memory — Cline 适配

## 安装

### 方式一：.clinerules 文件（推荐）

在项目根目录创建 `.clinerules` 文件，追加 SKILL.md 的核心内容（或引用）：

```markdown
## 开发记忆

本项目使用 dev-memory 管理开发记忆。
- 记忆文件: docs/dev-memory/DEV_MEMORY.md
- 维护指令: docs/dev-memory/SKILL.md
- 安装指引: https://github.com/HJ2916/dev-memory

请在对话开始时读取 DEV_MEMORY.md 的"当前状态概览"了解项目现状。
请在对话结束时按 SKILL.md 的流程更新记忆。
```

### 方式二：.clinerules/ 目录

Cline 支持将 `.clinerules` 作为目录使用，内部放多个规则文件：

```bash
mkdir -p .clinerules
cp SKILL.md .clinerules/dev-memory.md
```

### 方式三：VSCode 扩展设置

Cline 扩展设置 → custom instructions，粘贴 SKILL.md 内容。

## 触发机制

Cline 通过系统提示词模板引擎将 `.clinerules` 直接集成到 AI 上下文中。Cline 会自动识别任务与规则的相关性，按规则执行。目录模式下会读取目录中所有文件并注入。

## 特殊说明

- Cline 是 VSCode 插件，开源免费
- Cline 有自己的 Memory Bank 概念，与 dev-memory 互补：Memory Bank 存项目结构，dev-memory 存开发过程
- Cline 支持 diff 预览，与 dev-memory 的增量写入机制天然兼容
