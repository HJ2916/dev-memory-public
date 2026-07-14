# dev-memory 配置

## 版本
- dev-memory 版本: v2.2
- 执行规范: dev-memory/SKILL.md
- 安装地址: https://github.com/HJ2916/dev-memory-public

## 多工具注入指引

dev-memory v2.0 采用"寄生式注入"策略，将触发规则注入到各工具的最高优先级配置文件中，确保每轮对话自动触发。

| 工具 | 注入文件 | 注入类型 | 适配说明 |
|------|---------|---------|---------|
| **TRAE** | 项目 Rules (project_rules) | 硬性准则 | adapters/TRAE.md |
| **Claude Code** | `CLAUDE.md` | 用户指令 | adapters/claude-code.md |
| **Codex / OpenCode / MiMoCode** | `AGENTS.md` | 确定性加载 | adapters/AGENTS.md |
| **Cursor** | `.cursor/rules/dev-memory.mdc` | alwaysApply | adapters/cursor.md |
| **GitHub Copilot** | `.github/copilot-instructions.md` | 仓库级 | adapters/copilot.md |
| **Windsurf** | `.windsurf/rules/dev-memory.md` | always_on | adapters/windsurf.md |
| **Cline** | `.clinerules` | 始终加载 | adapters/cline.md |
| **CodeBuddy** | `.codebuddy/rules/dev-memory.md` | 始终加载 | adapters/codebuddy.md |
| **通用** | 自定义指令 | 始终加载 | adapters/generic.md |

详细注入步骤和规则内容见各适配器文件。

## 隐私设置

| 仓库类型 | 推荐设置 | 修改方式 |
|---------|---------|---------|
| 公开仓库 | 记忆文件 gitignore（默认） | 保持 .gitignore 不变 |
| 私有仓库 | 记忆文件纳入 Git | 注释掉 .gitignore 中 dev-memory 相关行 |
| 个人本地 | 任意 | 无远程仓库 |

## 记忆文件说明

| 文件 | 说明 | 是否纳入 Git |
|------|------|-------------|
| `CONFIG.md` | 本文件，配置和注入指引 | 始终纳入 |
| `SKILL.md` | 完整执行规范 | 始终纳入 |
| `DEV_MEMORY.md` | 项目级六维记忆索引 | 默认不纳入 |
| `USER_PROFILE.md` | 用户画像+贡献者日志+设备历史 | 默认不纳入 |
| `health-report.md` | 记忆健康仪表盘（v2.1） | 默认不纳入 |
| `dimensions/*.md` | 模块化记忆拆分文件（v2.2） | 默认不纳入 |
| `sessions/*.md` | 会话级摘要 | 默认不纳入 |
| `sessions/*.jsonl` | 消息级记录 | 默认不纳入 |
| `sessions/*.dream.md` | Dream 整理结果（v2.1） | 默认不纳入 |
| `sessions/archived.jsonl` | 已遗忘条目归档（v2.2） | 默认不纳入 |

## 路径-记忆映射表（v2.2 新增，可选）

在此配置文件路径模式与记忆维度的映射，开头检查时按当前操作路径额外加载匹配记忆。
留空则不启用条件触发，仅使用基础开头检查。

```yaml
# 示例配置（取消注释并按需修改）
# path_mappings:
#   - path: "src/api/**"
#     dimensions: ["⑤架构决策", "②踩坑记录"]
#     keywords: ["API", "接口", "端点"]
#   - path: "*.test.*"
#     dimensions: ["②踩坑记录"]
#     keywords: ["测试", "test", "mock"]
#   - path: "config/**"
#     dimensions: ["⑥环境信息", "⑤架构决策"]
#     keywords: ["配置", "config", "环境变量"]
```

## 显式遗忘指令（v2.2 新增）

用户可通过以下指令标记记忆为遗忘：
- "忘记 [关键词]" — 搜索匹配条目，标记 `[~💀]`
- "忘掉 [关键词]" — 同上
- 标记后的条目在下次 Dream 整理时移入 `sessions/archived.jsonl`（不删除，但不再加载）

## Git Hook（可选，提高触发可靠性）

将以下内容保存为 `.git/hooks/post-commit`：

```bash
#!/bin/bash
echo "⚠️ dev-memory: 本次 commit 的变更已记录到 Git。"
echo "请确认 AI 已执行记忆保存流程（dev-memory/DEV_MEMORY.md + sessions/ JSONL）。"
echo "如未执行，请提醒 AI: '请按 dev-memory/SKILL.md 保存记忆'"
```

## 快速开始

1. 选择你的 AI 工具，按上方注入指引配置触发规则
2. 安装 Skill（可选，有下载地址则不需要）：`cp SKILL.md dev-memory/SKILL.md`
3. 开始正常开发，AI 会在对话开始时静默检查、任务完成时自动保存记忆
4. 查看记忆：打开 `dev-memory/DEV_MEMORY.md`
5. 如需手动触发：告诉 AI "保存记忆"或"更新项目记忆"
