# dev-memory — AGENTS.md 跨工具统一适配（v3.0 兼容）

## 跨工具标准地位（v2.2 新增，v3.0 兼容）

AGENTS.md 已成为跨工具通用标准，被以下工具原生支持：
- **Codex CLI** / **OpenCode** / **MiMoCode**：主要指令文件
- **Cursor**：v0.45+ 自动读取 AGENTS.md
- **Claude Code**：通过 `@AGENTS.md` 导入语法加载
- **Qwen Code**：原生支持

**建议**：对于同时使用多个工具的项目，优先注入 AGENTS.md 作为统一入口，再按需在工具特定文件（CLAUDE.md/.cursor/rules 等）中补充工具特定配置。

## 优先级分析

AGENTS.md 是确定性加载（每次运行重建，无缓存），优先级高于 Memories（默认关闭，概率性）。

完整优先级链：系统提示 > AGENTS.md（确定性）> Memories（概率性，默认关闭）。

dev-memory 的触发规则注入到 AGENTS.md 后，确保每次运行都生效。

### 各工具的全局配置位置

| 工具 | 项目级 | 全局级 |
|------|--------|--------|
| Codex CLI | `./AGENTS.md` | `~/.codex/AGENTS.md` |
| OpenCode | `./AGENTS.md` | `~/.opencode/AGENTS.md` |
| MiMoCode | `./AGENTS.md` | `~/.mimo/AGENTS.md` |

## 安装 Skill（推荐）

将 SKILL.md 复制到项目内：
```bash
cp SKILL.md dev-memory/SKILL.md
```

### 安装验证

1. 重启工具（Codex/OpenCode/MiMoCode 等）
2. 确认 `AGENTS.md` 中包含 dev-memory 触发规则
3. 在对话中输入 "保存记忆"，检查 AI 是否执行保存流程

## 注入触发规则（必须）

### 项目级注入

在项目根目录创建或编辑 `AGENTS.md`，追加以下内容：

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

注意：AGENTS.md 建议控制在 200 行以内（"地图而非手册"原则），dev-memory 的规则只需 8 行。

### 全局注入（可选）

将同样的规则追加到全局 AGENTS.md（`~/.codex/AGENTS.md` 等），前面加一行：
```
注意：以下规则适用于所有项目。如项目内有 dev-memory/ 目录则自动生效，否则跳过。
```

## 与原生记忆系统的关系

Codex 的 Memories（默认关闭）与 dev-memory 互补：
- Memories 是概率性回忆，后台生成，存储在 `~/.codex/memories/`
- dev-memory 是确定性结构化记录，存储在项目目录内
- AGENTS.md 优先级高于 Memories，dev-memory 触发规则始终生效
- 如开启 Memories，两者互补：Memories 存偏好上下文，dev-memory 存开发过程

### AGENTS.md 加载细节

- 每次运行重建指令链，无缓存
- 从 Git 根到当前目录逐层检查，离工作目录越近的文件越靠后（覆盖前面的）
- 组合总大小上限 32 KiB（`project_doc_max_bytes`）
- `AGENTS.override.md` 优先于同目录的 `AGENTS.md`
