# dev-memory — GitHub Copilot 适配

## 优先级分析

Copilot 的配置优先级：组织级 > 仓库级（.github/copilot-instructions.md）> 个人级。

Copilot 没有原生持久化记忆系统，配置文件始终加载，无冲突。

## 安装 Skill（推荐）

将 SKILL.md 复制到项目内：
```bash
cp SKILL.md dev-memory/SKILL.md
```

### 安装验证

1. 重启 GitHub Copilot 扩展
2. 确认 `.github/copilot-instructions.md` 中包含 dev-memory 触发规则
3. 在对话中输入 "保存记忆"，检查 AI 是否能读取 SKILL.md 执行保存流程

## 注入触发规则（必须）

### 项目级注入

在项目根目录创建或编辑 `.github/copilot-instructions.md`，追加以下内容：

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

也支持 `.github/instructions/*.instructions.md` 格式的分文件指令。

### 全局注入（可选）

在 Copilot Personal-level instructions 中添加同样的规则，前面加一行：
```
注意：以下规则适用于所有项目。如项目内有 dev-memory/ 目录则自动生效，否则跳过。
```

## 与原生记忆系统的关系

Copilot 无原生记忆系统，无冲突。copilot-instructions.md 同时影响 Copilot Chat 和 Copilot 代码评审。
