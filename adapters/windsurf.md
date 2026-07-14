# dev-memory — Windsurf 适配

## 优先级分析

Windsurf 将持久化上下文拆为三个面向：Memories（自动记录）、Rules（手动撰写）、Workflows（命令触发）。

Rules 的三种触发模式：
- `always_on`：始终加载到上下文（推荐用于 dev-memory）
- `model_decision`：AI 根据相关性决定是否加载
- `glob`：匹配文件路径模式时加载

dev-memory 触发规则使用 `always_on` 模式，确保每轮对话自动生效。

注意：Windsurf 有 12,000 字符的上下文限制，过多 `always_on` 规则会过早耗尽配额。dev-memory 的规则仅 8 行，影响可忽略。

## 安装 Skill（可选）

```bash
mkdir -p .windsurf/rules
cp SKILL.md .windsurf/rules/dev-memory-skill.md
```

## 注入触发规则（必须）

### 项目级注入

创建 `.windsurf/rules/dev-memory.md`：

```yaml
---
trigger: always_on
description: dev-memory 项目级 AI 记忆系统
---

## 开发记忆规则（强制）

1. 对话开始时：静默读取 dev-memory/DEV_MEMORY.md 的 metadata 行和"当前状态概览"，
   检查时效性和会话去重。如文件不存在，标记"本轮需初始化"。
   同步检查 dev-memory/USER_PROFILE.md，如当前设备不在列表中则追加。
2. 任务完成时（代码推送/文件创建/测试通过等）：按 dev-memory/SKILL.md 执行记忆保存流程，
   包括六维提取、对比检查、增量写入、消息级 JSONL 追加、会话摘要更新。
3. 话题切换时：同上。
4. 分诊过滤：纯概念问答、单纯文件查看、闲聊未落地 → 跳过保存。
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
