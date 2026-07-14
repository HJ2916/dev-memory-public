# dev-memory v2.0 实现计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 将 dev-memory 从 v1.3 扁平六维结构升级为 v2.0 三层混合分层架构，吸收 TRAE 记忆系统优点，增加寄生式注入确保跨工具触发可靠性。

**Architecture:** 三层架构（项目级 DEV_MEMORY.md + 会话级 sessions/MD + 消息级 sessions/JSONL）+ 横向 USER_PROFILE.md（用户画像+贡献者溯源）+ 寄生式注入（薄 Rule + 厚 Skill）提升触发优先级。取消周归档，简化为 7.5 步执行流程。

**Tech Stack:** 纯 Markdown + JSONL，零依赖，10 个 AI 编程工具适配。

**设计规格:** `docs/v2.0-design-spec.md`

---

## 文件结构总览

### 需要创建的文件
| 文件路径 | 职责 |
|---------|------|
| `dev-memory/USER_PROFILE.md` | 项目级用户画像 + 贡献者日志 + 设备历史 |
| `dev-memory/sessions/` | 会话级+消息级记录目录（首次创建空目录） |

### 需要重写的文件
| 文件路径 | 变更类型 | 说明 |
|---------|---------|------|
| `SKILL.md` | 全量重写 | v1.3 → v2.0，7.5 步流程，三层架构，寄生式注入 |
| `adapters/TRAE.md` | 全量重写 | 安装指引 → 优先级分析+注入方案 |
| `adapters/claude-code.md` | 全量重写 | 同上 |
| `adapters/AGENTS.md` | 全量重写 | 同上 |
| `adapters/cursor.md` | 全量重写 | 同上 |
| `adapters/copilot.md` | 全量重写 | 同上 |
| `adapters/windsurf.md` | 全量重写 | 同上 |
| `adapters/cline.md` | 全量重写 | 同上 |
| `adapters/codebuddy.md` | 全量重写 | 同上 |
| `adapters/generic.md` | 全量重写 | 同上 |

### 需要修改的文件
| 文件路径 | 变更说明 |
|---------|---------|
| `.gitignore` | 路径从 `docs/dev-memory/` 改为 `dev-memory/`，增加 USER_PROFILE.md 和 sessions/ |
| `README.md` | 更新版本号、文件结构、工具支持表、技术规格 |
| `dev-memory/CONFIG.md`（原 `docs/dev-memory/CONFIG.md`） | 更新注入指引表、路径、Git hook 模板 |

### 需要迁移的文件
| 原路径 | 新路径 | 变更 |
|-------|-------|------|
| `docs/dev-memory/DEV_MEMORY.md` | `dev-memory/DEV_MEMORY.md` | 增加引用计数，移除归档字段 |
| `docs/dev-memory/CONFIG.md` | `dev-memory/CONFIG.md` | 内容更新 |

### 需要删除的
| 路径 | 原因 |
|------|------|
| `docs/dev-memory/archive/` | v2.0 取消归档制 |

### 需要同步的
| 路径 | 说明 |
|------|------|
| `C:\Users\JacobWu\.trae-cn\skills\dev-memory\SKILL.md` | TRAE 本地 Skill 副本同步 |

---

## Task 1: 重写 SKILL.md 为 v2.0

**Files:**
- Rewrite: `SKILL.md`（全量重写，参考 `docs/v2.0-design-spec.md` 第 2-5 章）

**关键变更清单（v1.3 → v2.0）：**

1. **Description 字段**：更新为 v2.0 描述（三层混合分层、寄生式注入、7.5步）
2. **核心原则**：从 10 条调整为 12 条
   - 移除：无（按周归档相关原则隐含在"零依赖"中）
   - 修改第 3 条"分层读取"→ 保持
   - 修改第 7 条"零依赖"→ "纯 Markdown + JSONL"
   - 新增第 11 条"三层混合分层"：项目级索引 + 会话级摘要 + 消息级记录
   - 新增第 12 条"寄生式注入"：触发规则注入到各工具最高优先级层
3. **双触发机制**：保持不变，增加"开头检查"中的设备检查和 sessions/ 检查
4. **执行流程**：从 8.5 步改为 7.5 步
   - 移除原 Step 6（溢出归档）
   - 原 Step 5 拆分为 5.1（写入 DEV_MEMORY.md）+ 5.2（追加 JSONL）+ 5.3（更新会话摘要）+ 5.4（更新 USER_PROFILE.md）
   - 原 Step 7 → Step 6，原 Step 7.5 → Step 6.5，原 Step 8 → Step 7
5. **Step 0**：路径从 `docs/dev-memory/` 改为 `dev-memory/`
6. **Step 4**：增加引用计数更新逻辑
7. **Step 5**：增加 JSONL 追加和会话摘要更新
8. **Step 6.5 Dream**：增加引用频率提升和跨层合并
9. **文件结构**：更新为 v2.0 结构（dev-memory/ 直接在项目根目录）
10. **溢出处理**：从归档改为标记 `[~]` + Dream 合并
11. **首次初始化**：路径更新，增加 USER_PROFILE.md 创建
12. **迭代记录**：追加 v2.0 记录
13. **所有路径引用**：`docs/dev-memory/` → `dev-memory/`

**具体内容：** 按设计规格文档第 2-5 章的详细设计编写完整 SKILL.md。以下是各章节的结构要求：

### SKILL.md v2.0 结构

```
# dev-memory
Description: [v2.0 描述]
Details:

## 核心原则（12条）
1. 双触发
2. 分诊优先
3. 分层读取
4. 增量写入
5. 追加式演进
6. 价值标记
7. 零依赖（纯 Markdown + JSONL）
8. 时间精确
9. 自我进化
10. 多项目感知
11. 三层混合分层（新增）
12. 寄生式注入（新增）

## 双触发机制
### 开头检查（增加设备检查 + sessions/ 检查）
### 结尾触发（不变）

## 保存流程（不变）

## 执行流程（7.5步）
### Step 0: 多项目识别（路径更新）
### Step 1: 分诊（不变）
### Step 2: 读取现有记忆（分层读取，token 更新）
### Step 3: 提取六维记忆（不变）
### Step 4: 对比检查（增加引用计数更新）
### Step 5: 写入主文件 + 追加 JSONL + 更新会话摘要
  #### 5.1 写入 DEV_MEMORY.md（增加 [引用:0] 初始标记）
  #### 5.2 追加 sessions/YYYY-MM-DD.jsonl（新增，含完整 JSON schema）
  #### 5.3 更新 sessions/YYYY-MM-DD.md（新增，含格式模板）
  #### 5.4 更新 USER_PROFILE.md（新增）
### Step 6: 自我进化检查（增加 JSONL/MD 验证）
### Step 6.5: 轻量 Dream 整理（增加引用频率提升 + 跨层合并）
### Step 7: 可见反馈（增加 JSONL/MD 反馈）

## 文件结构（v2.0）
## 隐私感知初始化（路径更新）
## DEV_MEMORY.md 模板（更新 metadata，移除周归档）
## USER_PROFILE.md 模板（新增）
## sessions/MD 模板（新增）
## sessions/JSONL schema（新增）
## 溢出处理（替代归档）
## 首次初始化特殊规则（路径更新 + USER_PROFILE 创建）
## 迭代记录（追加 v2.0）
```

- [ ] **Step 1: 编写 SKILL.md v2.0 完整内容**

按上述结构编写完整 SKILL.md。关键内容来源：
- 设计规格文档第 2 章（架构设计）→ 文件结构和三层架构
- 设计规格文档第 3 章（各文件详细设计）→ DEV_MEMORY.md、sessions/MD、sessions/JSONL、USER_PROFILE.md 的模板和 schema
- 设计规格文档第 4 章（寄生式注入）→ 核心原则第 12 条
- 设计规格文档第 5 章（执行流程）→ 7.5 步详细说明
- v1.3 SKILL.md 中不变的部分（双触发机制、分诊、六维提取、对比检查等）

所有路径从 `docs/dev-memory/` 改为 `dev-memory/`。

- [ ] **Step 2: 验证 SKILL.md 完整性**

检查项：
- 12 条核心原则齐全
- 7.5 步流程完整（Step 0 ~ Step 7，含 Step 6.5）
- 文件结构包含 DEV_MEMORY.md、USER_PROFILE.md、sessions/、CONFIG.md、SKILL.md
- JSONL schema 含 9 个字段（message_id/timestamp/session_id/intent/actions/outcome/learned/dimensions/references）
- 溢出处理用 `[~]` 标记而非归档
- 迭代记录含 v2.0 条目
- 无残留 `docs/dev-memory/` 路径引用
- 无残留 `archive/` 引用

- [ ] **Step 3: Commit**

```bash
cd D:\trae\traeworkspace\dev-memory
git add SKILL.md
git commit -m "feat: rewrite SKILL.md to v2.0 — three-layer architecture + parasitic injection"
```

---

## Task 2: 更新 .gitignore

**Files:**
- Modify: `.gitignore`

- [ ] **Step 1: 更新 .gitignore 中的 dev-memory 部分**

将现有的：
```gitignore
# === dev-memory 隐私策略 ===
# 默认不追踪记忆文件，保护开发隐私
# 如需追踪（如私有仓库便于多设备同步），注释掉以下两行
docs/dev-memory/DEV_MEMORY.md
docs/dev-memory/archive/

# 始终追踪的文件（配置和引导，不含记忆数据）
!docs/dev-memory/CONFIG.md
```

替换为：
```gitignore
# === dev-memory 隐私策略 ===
# 默认不追踪记忆文件，保护开发隐私
# 如需追踪（如私有仓库便于多设备同步），注释掉以下行
dev-memory/DEV_MEMORY.md
dev-memory/USER_PROFILE.md
dev-memory/sessions/

# 始终追踪的文件（配置和引导，不含记忆数据）
!dev-memory/CONFIG.md
!dev-memory/SKILL.md
```

- [ ] **Step 2: Commit**

```bash
git add .gitignore
git commit -m "chore: update .gitignore for v2.0 path change and new files"
```

---

## Task 3: 更新 CONFIG.md

**Files:**
- Modify: `docs/dev-memory/CONFIG.md`（迁移后为 `dev-memory/CONFIG.md`）

此任务在迁移后执行（Task 8 之后）。先用新内容写入当前位置，迁移时一起移动。

- [ ] **Step 1: 重写 CONFIG.md 内容**

用设计规格文档第 8 章的完整内容替换现有 CONFIG.md。关键变更：
- 版本号 v1.3 → v2.0
- 路径 `docs/dev-memory/` → `dev-memory/`
- 新增"多工具注入指引"表（9 个工具的注入文件和类型）
- 新增 Git Hook 模板
- 安装地址更新

- [ ] **Step 2: Commit**

```bash
git add docs/dev-memory/CONFIG.md
git commit -m "feat: update CONFIG.md with injection guide and Git hook template"
```

---

## Task 4: 更新 README.md

**Files:**
- Modify: `README.md`

- [ ] **Step 1: 更新 README.md 的以下部分**

**版本号**：v1.3 → v2.0

**核心特性表**：更新为 v2.0 特性
| 特性 | 说明 |
|------|------|
| 三层混合分层 | 项目级索引 + 会话级摘要 + 消息级 JSONL |
| 寄生式注入 | 触发规则注入各工具最高优先级层，确保每轮自动触发 |
| 双触发机制 | 开头检查 + 结尾硬触发 |
| 六维记忆提取 | 任务/踩坑/探索/结论/决策/环境 |
| 分诊过滤 | 不值得保存的对话直接跳过 |
| 追加式演进 | 旧记忆不删除，只追加更正 |
| 轻量 Dream | 合并/去重/提升 + 引用频率追踪 |
| 用户画像+溯源 | 贡献者日志 + 设备历史，跨设备完整继承 |
| 多工具适配 | 10 个工具，寄生式注入 |
| 隐私感知 | gitignore 策略 |
| 零依赖 | 纯 Markdown + JSONL |

**工具支持表**：更新"适配方式"列，从安装路径改为注入文件
| 工具 | 注入文件 | 详细说明 |
|------|---------|---------|
| TRAE | 项目 Rules (project_rules) | adapters/TRAE.md |
| Claude Code | CLAUDE.md | adapters/claude-code.md |
| Codex/OpenCode/MiMoCode | AGENTS.md | adapters/AGENTS.md |
| Cursor | .cursor/rules/dev-memory.mdc | adapters/cursor.md |
| Copilot | .github/copilot-instructions.md | adapters/copilot.md |
| Windsurf | .windsurf/rules/dev-memory.md | adapters/windsurf.md |
| Cline | .clinerules | adapters/cline.md |
| CodeBuddy | .codebuddy/rules/dev-memory.md | adapters/codebuddy.md |
| 通用 | 自定义指令 | adapters/generic.md |

**文件结构**：更新为 v2.0 结构
```
你的项目/
  dev-memory/
    CONFIG.md              ← 配置和注入指引（纳入 Git）
    DEV_MEMORY.md          ← 主记忆文件（默认 gitignore）
    USER_PROFILE.md        ← 用户画像+贡献者日志+设备历史（默认 gitignore）
    SKILL.md               ← 完整执行规范（可选，有下载地址则不需要）
    sessions/              ← 会话级+消息级记录（默认 gitignore）
      2026-07-14.md        ← 当日会话摘要
      2026-07-14.jsonl     ← 当日消息级记录
```

**技术规格**：
- 版本：v2.0
- 执行流程：7.5 步
- 架构：三层混合分层 + 寄生式注入
- 消息级：JSONL（必选）
- 归档策略：已取消，sessions/ 完整保留

- [ ] **Step 2: Commit**

```bash
git add README.md
git commit -m "docs: update README.md for v2.0 — three-layer architecture + injection"
```

---

## Task 5: 重写 TRAE.md 和 claude-code.md 适配器

**Files:**
- Rewrite: `adapters/TRAE.md`
- Rewrite: `adapters/claude-code.md`

每个适配器统一为 4 章节结构：优先级分析 → 安装 Skill（可选）→ 注入触发规则（必须）→ 与原生记忆系统的关系。

- [ ] **Step 1: 重写 adapters/TRAE.md**

```markdown
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

1. 对话开始时：静默读取 dev-memory/DEV_MEMORY.md 的 metadata 行和"当前状态概览"，
   检查时效性和会话去重。如文件不存在，标记"本轮需初始化"。
   同步检查 dev-memory/USER_PROFILE.md，如当前设备不在列表中则追加。
2. 任务完成时（代码推送/文件创建/测试通过等）：按 dev-memory/SKILL.md 执行记忆保存流程，
   包括六维提取、对比检查、增量写入、消息级 JSONL 追加、会话摘要更新。
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
```

- [ ] **Step 2: 重写 adapters/claude-code.md**

```markdown
# dev-memory — Claude Code 适配

## 优先级分析

Claude Code 的系统提示组装管线分三层：静态层（硬编码）> 动态层（Auto Memory + Skills）> 用户指令层（CLAUDE.md）。

Claude Code 的 Skills 采用"渐进式披露"：启动时只加载 Skill 描述，内容按需读取。dev-memory 作为 Skill 不保证每轮触发。通过将触发规则注入到项目级 CLAUDE.md（始终加载，用户指令层），确保每轮对话自动生效。

CLAUDE.md 与 Auto Memory 并列注入系统提示，由模型仲裁。CLAUDE.md 是"用户指令"权重更高，且 Auto Memory 明确不保存 CLAUDE.md 已记录的内容，无冲突。

## 安装 Skill（可选）

```bash
# 方式一：Slash Command
mkdir -p ~/.claude/commands
cp SKILL.md ~/.claude/commands/dev-memory.md

# 方式二：Skills 系统（Claude Code 2.1+）
mkdir -p ~/.claude/skills/dev-memory
cp SKILL.md ~/.claude/skills/dev-memory/SKILL.md
```

## 注入触发规则（必须）

### 项目级注入

在项目根目录创建或编辑 `CLAUDE.md`，追加以下内容：

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
```

- [ ] **Step 3: Commit**

```bash
git add adapters/TRAE.md adapters/claude-code.md
git commit -m "feat: rewrite TRAE and Claude Code adapters with injection scheme"
```

---

## Task 6: 重写 AGENTS.md、cursor.md、copilot.md 适配器

**Files:**
- Rewrite: `adapters/AGENTS.md`
- Rewrite: `adapters/cursor.md`
- Rewrite: `adapters/copilot.md`

- [ ] **Step 1: 重写 adapters/AGENTS.md**

```markdown
# dev-memory — Codex CLI / OpenCode / MiMoCode 适配

## 优先级分析

这三个工具都使用 AGENTS.md 标准。AGENTS.md 是确定性加载（每次运行重建，无缓存），优先级高于 Memories（默认关闭，概率性）。

完整优先级链：系统提示 > AGENTS.md（确定性）> Memories（概率性，默认关闭）。

dev-memory 的触发规则注入到 AGENTS.md 后，确保每次运行都生效。

### 各工具的全局配置位置

| 工具 | 项目级 | 全局级 |
|------|--------|--------|
| Codex CLI | `./AGENTS.md` | `~/.codex/AGENTS.md` |
| OpenCode | `./AGENTS.md` | `~/.opencode/AGENTS.md` |
| MiMoCode | `./AGENTS.md` | `~/.mimo/AGENTS.md` |

## 安装 Skill（可选）

将 SKILL.md 复制到项目内：
```bash
cp SKILL.md dev-memory/SKILL.md
```

## 注入触发规则（必须）

### 项目级注入

在项目根目录创建或编辑 `AGENTS.md`，追加以下内容：

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
```

- [ ] **Step 2: 重写 adapters/cursor.md**

```markdown
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
```

- [ ] **Step 3: 重写 adapters/copilot.md**

```markdown
# dev-memory — GitHub Copilot 适配

## 优先级分析

Copilot 的配置优先级：组织级 > 仓库级（.github/copilot-instructions.md）> 个人级。

Copilot 没有原生持久化记忆系统，配置文件始终加载，无冲突。

## 安装 Skill（可选）

将 SKILL.md 复制到项目内：
```bash
cp SKILL.md dev-memory/SKILL.md
```

## 注入触发规则（必须）

### 项目级注入

在项目根目录创建或编辑 `.github/copilot-instructions.md`，追加以下内容：

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

也支持 `.github/instructions/*.instructions.md` 格式的分文件指令。

### 全局注入（可选）

在 Copilot Personal-level instructions 中添加同样的规则，前面加一行：
```
注意：以下规则适用于所有项目。如项目内有 dev-memory/ 目录则自动生效，否则跳过。
```

## 与原生记忆系统的关系

Copilot 无原生记忆系统，无冲突。copilot-instructions.md 同时影响 Copilot Chat 和 Copilot 代码评审。
```

- [ ] **Step 4: Commit**

```bash
git add adapters/AGENTS.md adapters/cursor.md adapters/copilot.md
git commit -m "feat: rewrite AGENTS, Cursor, Copilot adapters with injection scheme"
```

---

## Task 7: 重写 windsurf.md、cline.md、codebuddy.md、generic.md 适配器

**Files:**
- Rewrite: `adapters/windsurf.md`
- Rewrite: `adapters/cline.md`
- Rewrite: `adapters/codebuddy.md`
- Rewrite: `adapters/generic.md`

- [ ] **Step 1: 重写 adapters/windsurf.md**

```markdown
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
```

- [ ] **Step 2: 重写 adapters/cline.md**

```markdown
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
```

- [ ] **Step 3: 重写 adapters/codebuddy.md**

```markdown
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
```

- [ ] **Step 4: 重写 adapters/generic.md**

```markdown
# dev-memory — 通用适配

## 适用场景

任何支持自定义指令/系统提示词的 AI 工具，不限于上述已适配的工具。

## 优先级分析

大多数 AI 工具的自定义指令都是始终加载的，优先级仅次于系统提示。dev-memory 的触发规则注入后，确保每轮对话自动生效。

如果工具支持条件触发（如按文件类型），建议设置为"始终触发"或"匹配所有文件"。

## 安装 Skill（可选）

将 SKILL.md 复制到项目内：
```bash
cp SKILL.md dev-memory/SKILL.md
```

## 注入触发规则（必须）

### 项目级注入

在 AI 工具支持的自定义指令文件中（如 `ai-instructions.md`、`prompt.md`、`system-prompt.md` 等），追加以下内容：

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

### 对话中手动触发

如果工具不支持自动加载自定义指令，在对话开始时告诉 AI：
```
请读取 dev-memory/SKILL.md 并按照其中的流程维护项目记忆。
```

## 与原生记忆系统的关系

dev-memory 的核心是纯 Markdown + JSONL 指令，任何能读取文件的 AI 都能执行。如果工具有原生记忆系统，dev-memory 作为项目级补充层，不替代原生记忆，而是提供更结构化的开发过程记录。
```

- [ ] **Step 5: Commit**

```bash
git add adapters/windsurf.md adapters/cline.md adapters/codebuddy.md adapters/generic.md
git commit -m "feat: rewrite Windsurf, Cline, CodeBuddy, generic adapters with injection scheme"
```

---

## Task 8: 项目迁移 — 移动目录 + 创建新文件

**Files:**
- Move: `docs/dev-memory/` → `dev-memory/`
- Create: `dev-memory/sessions/`（空目录，放 .gitkeep）
- Create: `dev-memory/USER_PROFILE.md`

- [ ] **Step 1: 移动目录**

```powershell
cd D:\trae\traeworkspace\dev-memory
Move-Item docs/dev-memory/ dev-memory/
```

- [ ] **Step 2: 创建 sessions/ 目录**

```powershell
mkdir dev-memory/sessions
# 创建 .gitkeep 使空目录可被 Git 追踪（如 sessions/ 纳入 Git 的话）
# 但 sessions/ 默认 gitignore，所以不需要 .gitkeep
```

- [ ] **Step 3: 创建 USER_PROFILE.md**

按设计规格第 3.4 章的模板创建。从 TRAE 的 `user_profile.md` 迁移偏好，创建贡献者日志和设备历史。

获取设备标识：
```powershell
$env:COMPUTERNAME
$env:USERNAME
```

写入 `dev-memory/USER_PROFILE.md`，包含三个区块：当前开发者偏好、贡献者日志、设备历史。

- [ ] **Step 4: 删除旧 archive/ 目录（如有）**

```powershell
Remove-Item -Recurse -Force dev-memory/archive/ -ErrorAction SilentlyContinue
```

- [ ] **Step 5: 验证文件结构**

```powershell
# 确认目录结构
LS dev-memory/
# 应看到: CONFIG.md, DEV_MEMORY.md, USER_PROFILE.md, sessions/
# 不应看到: archive/

# 确认旧目录已移走
Test-Path docs/dev-memory/
# 应返回 False
```

- [ ] **Step 6: Commit**

```bash
git add -A
git commit -m "refactor: migrate to v2.0 directory structure — dev-memory/ at project root"
```

---

## Task 9: 迁移 DEV_MEMORY.md

**Files:**
- Modify: `dev-memory/DEV_MEMORY.md`（已迁移到新位置）

- [ ] **Step 1: 更新 metadata 行**

将：
```markdown
> 最后更新: 2026-07-10 19:15 | 会话: 6a50bc3f | 周归档: 2026-W28 | 指纹: 6维/29条/0更正/90分
```

替换为：
```markdown
> 最后更新: 2026-07-14 12:19 | 会话: 6a55a2ee | 指纹: 6维/29条/0更正/90分 | 最后整理: 2026-07-10
```

- [ ] **Step 2: 为所有条目添加引用计数**

在每个六维条目末尾添加 ` [引用:0]`。例如：
```markdown
- [2026-07-10 19:15] [★★★] 完成 v1.3 升级：... [引用:0]
```

- [ ] **Step 3: 更新溢出上限说明**

在各维度标题下更新上限说明：
- 任务摘要 / 踩坑记录：上限 8 条（原 5 条）
- 探索路径 / 架构决策：上限 5 条（原 3 条）
- 最终结论 / 环境信息：上限 8 条（原 5 条）

- [ ] **Step 4: 更新归档索引部分**

将"归档索引"部分替换为：
```markdown
## 会话索引

（v2.0 取消归档制，所有消息级记录保存在 sessions/ 目录。以下为近期会话摘要索引。）
- sessions/2026-07-14.md — v2.0 架构设计
```

- [ ] **Step 5: 追加 v2.0 变更记录**

在"任务与变更摘要"维度顶部追加：
```markdown
- [2026-07-14 12:19] [★★★] 完成 v2.0 架构升级：三层混合分层（项目级+会话级+消息级JSONL）、寄生式注入（薄Rule+厚Skill）、USER_PROFILE.md（贡献者日志+设备溯源）、取消周归档、7.5步流程 [引用:0]
```

- [ ] **Step 6: Commit**

```bash
git add dev-memory/DEV_MEMORY.md
git commit -m "refactor: migrate DEV_MEMORY.md to v2.0 format — reference counts, no archive"
```

---

## Task 10: 更新 TRAE 本地 Skill 副本

**Files:**
- Copy: `SKILL.md` → `C:\Users\JacobWu\.trae-cn\skills\dev-memory\SKILL.md`

- [ ] **Step 1: 复制更新后的 SKILL.md 到 TRAE skills 目录**

```powershell
Copy-Item SKILL.md C:\Users\JacobWu\.trae-cn\skills\dev-memory\SKILL.md -Force
```

- [ ] **Step 2: 验证**

```powershell
# 确认文件已更新
Get-Item C:\Users\JacobWu\.trae-cn\skills\dev-memory\SKILL.md | Select-Object LastWriteTime
```

---

## Task 11: 最终验证 + Git 推送

- [ ] **Step 1: 验证文件结构完整性**

```powershell
cd D:\trae\traeworkspace\dev-memory

# 检查核心文件
Test-Path SKILL.md
Test-Path README.md
Test-Path .gitignore
Test-Path dev-memory/CONFIG.md
Test-Path dev-memory/DEV_MEMORY.md
Test-Path dev-memory/USER_PROFILE.md
Test-Path dev-memory/sessions/
Test-Path adapters/TRAE.md
Test-Path adapters/claude-code.md
Test-Path adapters/AGENTS.md
Test-Path adapters/cursor.md
Test-Path adapters/copilot.md
Test-Path adapters/windsurf.md
Test-Path adapters/cline.md
Test-Path adapters/codebuddy.md
Test-Path adapters/generic.md
# 全部应返回 True
```

- [ ] **Step 2: 验证无残留旧路径**

```powershell
# 搜索 SKILL.md 中是否还有 docs/dev-memory/ 引用
Select-String -Path SKILL.md -Pattern "docs/dev-memory/" -SimpleMatch
# 应无输出

# 搜索所有适配器中的旧路径
Select-String -Path adapters/*.md -Pattern "docs/dev-memory/" -SimpleMatch
# 应无输出
```

- [ ] **Step 3: 验证 .gitignore**

```powershell
# 确认 gitignore 包含新路径
Select-String -Path .gitignore -Pattern "dev-memory/"
# 应看到 DEV_MEMORY.md, USER_PROFILE.md, sessions/, !CONFIG.md, !SKILL.md
```

- [ ] **Step 4: Git 推送**

```bash
git push origin main
```

- [ ] **Step 5: 更新公开仓库（可选）**

```bash
# 公开仓库同步（不含记忆文件）
git push public main
```

---

## 自审检查

### Spec 覆盖检查

| 设计规格章节 | 对应 Task | 覆盖状态 |
|-------------|----------|---------|
| 第 2 章 架构设计 | Task 1 (SKILL.md) | ✓ |
| 第 3.1 章 DEV_MEMORY.md | Task 1 (模板) + Task 9 (迁移) | ✓ |
| 第 3.2 章 sessions/MD | Task 1 (模板) | ✓ |
| 第 3.3 章 sessions/JSONL | Task 1 (schema) | ✓ |
| 第 3.4 章 USER_PROFILE.md | Task 1 (模板) + Task 8 (创建) | ✓ |
| 第 4 章 寄生式注入 | Task 1 (原则) + Task 5-7 (适配器) | ✓ |
| 第 5 章 执行流程 | Task 1 (SKILL.md) | ✓ |
| 第 6 章 .gitignore | Task 2 | ✓ |
| 第 7 章 适配器升级 | Task 5, 6, 7 | ✓ |
| 第 8 章 CONFIG.md | Task 3 | ✓ |
| 第 9 章 迁移方案 | Task 8, 9, 10 | ✓ |
| 第 10 章 成本分析 | 无需实现（分析文档） | N/A |
| 第 11 章 竞品对比 | 无需实现（分析文档） | N/A |

### 类型一致性检查

- JSONL schema 字段名在 SKILL.md 和适配器中一致：message_id, timestamp, session_id, intent, actions, outcome, learned, dimensions, references ✓
- 文件路径在所有文件中一致：`dev-memory/`（非 `docs/dev-memory/`） ✓
- 适配器中的注入规则文本在所有 9 个文件中一致（8 行规则） ✓
- metadata 格式一致：`> 最后更新: ... | 会话: ... | 指纹: ... | 最后整理: ...` ✓

### 占位符扫描

无 TBD/TODO/未完成段落。所有 Task 都有具体的文件内容和命令。 ✓
