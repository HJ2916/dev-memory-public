# dev-memory — CodeBuddy 适配

## 优先级分析

CodeBuddy（腾讯）支持多层级的规则系统：Project Rules（.codebuddy/rules/）> User Rules。

Project Rules 受版本控制管理，可与团队成员共享。CodeBuddy Code 2.0 支持 Skills 系统。

## 安装 Skill（可选）

```bash
mkdir -p .codebuddy/rules
cp SKILL.md .codebuddy/rules/dev-memory-skill.md
```

## 注入触发规则（必须）

### 项目级注入

创建 `.codebuddy/rules/dev-memory.md`：

```markdown
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

在 CodeBuddy IDE 中：Settings → Rules → User Rules，粘贴同样内容，前面加一行：
```
注意：以下规则适用于所有项目。如项目内有 dev-memory/ 目录则自动生效，否则跳过。
```

## 与原生记忆系统的关系

CodeBuddy 的 Skills 系统与 dev-memory 的 Skill 概念兼容。Project Rules 受版本控制管理，适合团队协作场景。dev-memory 的触发规则作为 Project Rules 始终生效。
