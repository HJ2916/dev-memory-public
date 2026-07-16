# dev-memory — Windsurf 适配

## 优先级分析

Windsurf 将持久化上下文拆为三个面向：Memories（自动记录）、Rules（手动撰写）、Workflows（命令触发）。

Rules 的三种触发模式：
- `always_on`：始终加载到上下文（推荐用于 dev-memory）
- `model_decision`：AI 根据相关性决定是否加载
- `glob`：匹配文件路径模式时加载

dev-memory 触发规则使用 `always_on` 模式，确保每轮对话自动生效。

注意：Windsurf 有 12,000 字符的上下文限制，过多 `always_on` 规则会过早耗尽配额。dev-memory 的规则仅 8 行，影响可忽略。

## 安装 Skill（推荐）

> **重要**：SKILL.md 文件顶部包含 YAML front matter（`---name/description---`）。Windsurf 的规则文件使用自己的 front matter 格式（`trigger: always_on`）。

```bash
mkdir -p .windsurf/rules
cp SKILL.md .windsurf/rules/dev-memory-skill.md
```

### 安装验证

1. 重启 Windsurf
2. 检查 `.windsurf/rules/dev-memory.md` 是否在 Rules 面板中显示且状态为 `always_on`
3. 在对话中输入 "保存记忆"，检查 AI 是否执行保存流程

## 注入触发规则（必须）

### 项目级注入

创建 `.windsurf/rules/dev-memory.md`：

```yaml
---
trigger: always_on
description: dev-memory 项目级 AI 记忆系统
---

## 开发记忆规则（强制）

1. 对话开始时：在生成任何回复之前，必须先静默读取 dev-memory/DEV_MEMORY.md 的 metadata 行和"当前状态概览"，
   检查时效性和会话去重。如文件不存在，标记"本轮需初始化"。
   同步检查 dev-memory/USER_PROFILE.md，如当前设备不在列表中则追加。
   扫描 [有效期:] 标记，过期条目标记 [~⏰]。
   读取最近一次 sessions/YYYY-MM-DD.dream.md 摘要（如存在）。
   读取 dev-memory/CONFIG.md 路径-记忆映射表（如存在），按当前操作路径加载匹配记忆。
2. 任务完成时（代码推送/文件创建/测试通过等）：按 dev-memory/SKILL.md 执行记忆保存流程，
   包括六维提取、对比检查、增量写入、即时追加消息级 JSONL（不得批量补录）、会话摘要更新。
3. 话题切换时：同上。
4. 分诊过滤：纯概念问答、单纯文件查看、闲聊未落地 → 跳过保存。
5. 显式遗忘：如用户说"忘记 [关键词]"，搜索匹配条目标记 [~💀]，下次 Dream 归档。
```

### 全局注入（可选）

在 `~/.windsurf/rules/dev-memory.md` 中添加同样的规则，前面加一行：
```
注意：以下规则适用于所有项目。如项目内有 dev-memory/ 目录则自动生效，否则跳过。
```

## 与原生记忆系统的关系

Windsurf 的 Memories（自动记录对话重点）与 dev-memory 互补：
- Memories 自动记录对话重点，不需要手动维护
- dev-memory 结构化保存开发过程（六维 + 消息级 JSONL）
- Rules 优先级高于 Memories，dev-memory 触发规则始终生效

Windsurf 也支持目录级 AGENTS.md，父层说明自动被子层继承。
