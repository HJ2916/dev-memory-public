# dev-memory — Cline 适配

## 优先级分析

Cline 的配置系统简单：系统提示 > .clinerules（始终加载，全部生效）。

Cline 没有原生持久化记忆系统（社区有 Memory Bank 概念但非内置），.clinerules 始终加载，无冲突。

## 安装 Skill（可选）

```bash
# 方式一：单文件
cp SKILL.md .clinerules/dev-memory-skill.md

# 方式二：目录模式
mkdir -p .clinerules
cp SKILL.md .clinerules/dev-memory.md
```

## 注入触发规则（必须）

### 项目级注入

在项目根目录创建或编辑 `.clinerules`，追加以下内容：

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

### 全局注入

Cline 扩展设置 → custom instructions，粘贴同样内容，前面加一行：
```
注意：以下规则适用于所有项目。如项目内有 dev-memory/ 目录则自动生效，否则跳过。
```

## 与原生记忆系统的关系

Cline 无原生记忆系统。社区 Memory Bank 概念与 dev-memory 互补：Memory Bank 存项目结构，dev-memory 存开发过程。Cline 支持 diff 预览，与 dev-memory 的增量写入机制天然兼容。
