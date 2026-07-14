# dev-memory — TRAE 适配

## 优先级分析

TRAE 的优先级链：系统提示 > Rules（硬性准则）> Memory（柔性偏好）> Skills（按需触发）。

dev-memory 作为 Skill 时处于最低优先级，不保证每轮触发。通过将触发规则注入到项目 Rules（project_rules），提升到最高优先级（硬性准则，必须遵守）。

## 安装 Skill（可选）

```bash
# TRAE 中国版
mkdir -p ~/.trae-cn/skills/dev-memory
cp SKILL.md ~/.trae-cn/skills/dev-memory/SKILL.md

# TRAE 国际版
mkdir -p ~/.trae/skills/dev-memory
cp SKILL.md ~/.trae/skills/dev-memory/SKILL.md
```

## 注入触发规则（必须）

### 项目级注入

在 TRAE 中打开项目 → Settings → Rules → Project Rules，添加以下内容：

```markdown
## 开发记忆规则（强制）

1. 对话开始时：在生成任何回复之前，必须先静默读取 dev-memory/DEV_MEMORY.md 的 metadata 行和"当前状态概览"，
   检查时效性和会话去重。如文件不存在，标记"本轮需初始化"。
   同步检查 dev-memory/USER_PROFILE.md，如当前设备不在列表中则追加。
   扫描 [有效期:] 标记，过期条目标记 [~⏰]。
   读取最近一次 sessions/YYYY-MM-DD.dream.md 摘要（如存在）。
2. 任务完成时（代码推送/文件创建/测试通过等）：按 dev-memory/SKILL.md 执行记忆保存流程，
   包括六维提取、对比检查、增量写入、即时追加消息级 JSONL、会话摘要更新。
   JSONL 必须在每次任务完成时立即追加，不得批量补录。
3. 话题切换时：同上。
4. 分诊过滤：纯概念问答、单纯文件查看、闲聊未落地 → 跳过保存。
```

### 全局注入（可选）

在 TRAE → Settings → Rules → User Rules 中添加同样的规则，前面加一行：
```
注意：以下规则适用于所有项目。如项目内有 dev-memory/ 目录则自动生效，否则跳过。
```

## 与原生记忆系统的关系

TRAE 原生 Memory（user_profile.md + project_memory.md）与 dev-memory 互补不冲突：
- TRAE Memory 存"是什么"（项目硬约束、工程规范、经验教训）— 柔性偏好
- dev-memory 存"怎么做的"（开发过程、决策路径、消息级记录）— 强制规则
- Rules 优先级 > Memory，dev-memory 的触发规则始终生效
- TRAE Memory 的 20 条上限自动淘汰与 dev-memory 的追加式演进不冲突，两者独立运作
