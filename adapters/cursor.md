# dev-memory — Cursor 适配

## 优先级分析

Cursor 的规则优先级：系统提示 > Team Rules > Project Rules（.cursor/rules/*.mdc）> User Rules。

Cursor 没有原生持久化记忆系统（LLM 不跨会话记忆），不存在原生记忆覆盖自定义规则的问题。

`.cursor/rules/*.mdc` 设置 `alwaysApply: true` 后，规则内容在每次会话开始时自动注入到上下文开头。Cursor 2026 年也原生支持 AGENTS.md 作为简化替代。

## 安装 Skill（可选）

```bash
mkdir -p .cursor/rules
cp SKILL.md .cursor/rules/dev-memory-skill.mdc
```

## 注入触发规则（必须）

### 项目级注入

创建 `.cursor/rules/dev-memory.mdc`：

```yaml
---
description: dev-memory 项目级 AI 记忆系统
globs: ["**/*"]
alwaysApply: true
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

在 Cursor Settings → Rules 中添加 User Rules，内容同上，前面加一行：
```
注意：以下规则适用于所有项目。如项目内有 dev-memory/ 目录则自动生效，否则跳过。
```

## 与原生记忆系统的关系

Cursor 无原生记忆系统，无冲突。社区流传的 "Cursor Memory Bank" 等方案是第三方基于文件系统的自定义解决方案，与 dev-memory 理念一致但 dev-memory 更完整。

### Cursor Hooks（可选增强）

Cursor 2026 年的 Hooks 功能提供 `sessionStart` 和 `sessionEnd` 钩子，可确定性执行脚本：
- `sessionStart`：可注入上下文、初始化环境
- `sessionEnd`：可审计、记录

可在 `.cursor/hooks.json` 中配置 sessionEnd hook 提醒执行记忆保存。
