# dev-memory — Claude Code 适配

## 优先级分析

Claude Code 的系统提示组装管线分三层：静态层（硬编码）> 动态层（Auto Memory + Skills）> 用户指令层（CLAUDE.md）。

Claude Code 的 Skills 采用"渐进式披露"：启动时只加载 Skill 描述，内容按需读取。dev-memory 作为 Skill 不保证每轮触发。通过将触发规则注入到项目级 CLAUDE.md（始终加载，用户指令层），确保每轮对话自动生效。

CLAUDE.md 与 Auto Memory 并列注入系统提示，由模型仲裁。CLAUDE.md 是"用户指令"权重更高，且 Auto Memory 明确不保存 CLAUDE.md 已记录的内容，无冲突。

## 安装 Skill（可选）

> **重要**：SKILL.md 文件顶部必须包含 YAML front matter 元数据块（`---name/description---`），否则 Claude Code Skills 系统无法识别。

```bash
# 方式一：Slash Command
mkdir -p ~/.claude/commands
cp SKILL.md ~/.claude/commands/dev-memory.md

# 方式二：Skills 系统（Claude Code 2.1+）
mkdir -p ~/.claude/skills/dev-memory
cp SKILL.md ~/.claude/skills/dev-memory/SKILL.md
```

### 安装验证

1. 重启 Claude Code
2. 输入 `/dev-memory` 检查是否可用（Slash Command 模式）
3. 或在对话中输入 "使用 dev-memory skill"，检查 AI 是否能加载 Skill 内容

## 注入触发规则（必须）

### 项目级注入

在项目根目录创建或编辑 `CLAUDE.md`，追加以下内容：

```markdown
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

在 `~/.claude/CLAUDE.md` 中添加同样的规则，前面加一行：
```
注意：以下规则适用于所有项目。如项目内有 dev-memory/ 目录则自动生效，否则跳过。
```

## 与原生记忆系统的关系

Claude Code 的 Auto Memory 与 dev-memory 互补不冲突：
- Auto Memory 是 AI 自己写给自己的笔记（user/feedback/project/reference 四类），后台自动生成
- dev-memory 是结构化的项目开发记忆（六维 + 消息级 JSONL + 会话摘要）
- Auto Memory 明确不保存 CLAUDE.md 已记录的内容，无重复冲突
- Auto Memory 有 Dream 整理（合并/去重/裁剪），dev-memory 有轻量 Dream（合并/去重/提升+引用频率），两者独立运作
- CLAUDE.md 不限制字符数（假定深思熟虑），Auto Memory 有 40000 字符截断
