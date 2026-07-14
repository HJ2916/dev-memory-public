# dev-memory — 项目级 AI 记忆系统

> 把开发记忆变成随项目走的轻量资产，不绑账号、不绑工具、不绑平台。

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![TRAE Skill](https://img.shields.io/badge/TRAE-Skill-blue)](https://www.trae.cn/)

## 这是什么

**dev-memory** 是一个项目级 AI 记忆系统。在 AI 工具高速迭代的时代，工具会换、账号会变、模型会更新，唯独你的项目和设备是稳定的锚点。dev-memory 把开发记忆的归属权从账号和工具上拿下来，放到项目这一层——用纯 Markdown + JSONL 文件存在项目目录里，跟着 Git 走，不绑任何平台。

AI 行业发展至今，已有针对"用户"的记忆系统（ChatGPT Memory），针对"Agent 工具"的记忆系统（Claude Code CLAUDE.md、Hermes Agent）。dev-memory 填补了缺失的一环：**针对"项目和设备"的记忆系统**。

## 核心特性

| 特性 | 说明 |
|------|------|
| 三层混合分层 | 项目级索引 + 会话级摘要 + 消息级 JSONL，完整保留开发过程 |
| 寄生式注入 | 触发规则注入各工具最高优先级层，确保每轮自动触发 |
| 双触发机制 | 开头检查建立保存意识 + 结尾硬触发（任务完成/话题切换），不依赖用户说特定的话 |
| 六维记忆提取 | 任务摘要、踩坑记录、探索路径、最终结论、架构决策、环境信息 |
| 分诊过滤 | 不值得保存的对话直接跳过，避免无效 IO |
| 追加式演进 | 旧记忆永不删除，只追加更正说明，完整保留知识演进轨迹 |
| 轻量 Dream 整理 | 合并/去重/提升 + 引用频率追踪，异步执行（v2.1）+ 冷却期控制（v2.1.1） |
| 用户画像+溯源 | 贡献者日志 + 设备历史，跨设备完整继承记忆 |
| 多工具适配 | 支持 9+ 个主流 AI 编程工具，寄生式注入 |
| 隐私感知 | 记忆文件默认 gitignore，公开仓库忽略/私有仓库建议追踪 |
| 零依赖 | 纯 Markdown + JSONL，不执行任何脚本，换任何工具都能读 |
| 自我进化 | Skill 自身预留迭代记录区域，含触发可靠性审计 |
| **时序感知**（v2.1） | `[有效期:]` 标记 + `[~⏰]` 过期检测 + Dream 过期处理，纯文件实现时序推理 |
| **条件触发**（v2.2） | CONFIG.md 路径-记忆映射表（YAML），按操作路径加载匹配记忆 |
| **模块化文件**（v2.2） | `@dimensions/XX-name.md` 引用语法，单文件膨胀时自动拆分 |
| **显式遗忘**（v2.2） | "忘记 [关键词]" → `[~💀]` 标记 → Dream 归档到 `archived.jsonl` |
| **记忆健康仪表盘**（v2.1） | `health-report.md` 可视化记忆状态：总览/引用Top5/过期/Dream历史/维度健康 |
| **异步 Dream**（v2.1） | 任务完成后反馈前异步执行，结果写入 `dream.md` 供下次加载 |
| **自动关联检测**（v3.0） | Step 4 关键词匹配自动递增引用计数 + 交叉关联标记，知识网络自然形成 |

## 支持的 AI 工具

| 工具 | 注入文件 | 详细说明 |
|------|---------|---------|
| **TRAE** | 项目 Rules (project_rules) | [adapters/TRAE.md](adapters/TRAE.md) |
| **Claude Code** | `CLAUDE.md` | [adapters/claude-code.md](adapters/claude-code.md) |
| **Codex / OpenCode / MiMoCode / Qwen Code** | `AGENTS.md`（v2.2 跨工具统一入口） | [adapters/AGENTS.md](adapters/AGENTS.md) |
| **Cursor** | `.cursor/rules/dev-memory.mdc` | [adapters/cursor.md](adapters/cursor.md) |
| **GitHub Copilot** | `.github/copilot-instructions.md` | [adapters/copilot.md](adapters/copilot.md) |
| **Windsurf** | `.windsurf/rules/dev-memory.md` | [adapters/windsurf.md](adapters/windsurf.md) |
| **Cline** | `.clinerules` | [adapters/cline.md](adapters/cline.md) |
| **CodeBuddy** | `.codebuddy/rules/dev-memory.md` | [adapters/codebuddy.md](adapters/codebuddy.md) |
| **通用** | 自定义指令 | [adapters/generic.md](adapters/generic.md) |

## 快速开始

### 1. 注入触发规则

根据你使用的 AI 工具，参考上方表格选择注入方式。TRAE 用户：在项目 Settings → Rules → Project Rules 中添加触发规则（详见 [adapters/TRAE.md](adapters/TRAE.md)）。

### 2. 自动工作

注入后自动生效——对话开始时静默检查记忆状态，任务完成或话题切换时自动保存记忆。无需手动操作。

### 3. 查看记忆

```
你的项目/
  dev-memory/
    CONFIG.md              ← 配置和注入指引（纳入 Git）
    SKILL.md               ← 完整执行规范（纳入 Git，有下载地址则不需要）
    DEV_MEMORY.md          ← 主记忆文件（默认 gitignore）
    USER_PROFILE.md        ← 用户画像+贡献者日志+设备历史（默认 gitignore）
    health-report.md       ← 记忆健康仪表盘（默认 gitignore）
    sessions/              ← 会话级+消息级记录（默认 gitignore）
      2026-07-14.md        ← 当日会话摘要
      2026-07-14.jsonl     ← 当日消息级记录
      2026-07-14.dream.md  ← Dream 整理结果（v2.1）
      archived.jsonl       ← 已遗忘条目归档（v2.2）
    dimensions/            ← 模块化记忆拆分文件（v2.2，默认 gitignore）
```

## 隐私策略

| 仓库类型 | 推荐设置 | 原因 |
|---------|---------|------|
| 公开仓库 | 记忆文件 gitignore（默认） | 保护开发隐私 |
| 私有仓库 | 记忆文件纳入 Git | 便于多设备同步和团队协作 |
| 个人本地 | 任意 | 无远程仓库 |

修改方式：编辑 `.gitignore` 中 dev-memory 相关行。

## 设计理念

dev-memory 站在两个成熟方案肩上，取其精华，去其重量：

- **取 Claude Code 的**"项目目录存储 + AI 自动维护"→ 但不锁死在 Claude Code 内
- **取 Hermes Agent 的**"自我进化 + 迭代记录"→ 但不需要完整框架支撑
- **取 TRAE 的**"三层分层 + 消息级捕获"→ 但不绑定安装目录，跟随项目走
- **用纯 Markdown + JSONL 实现**，工具兼容但不绑定

详细设计思路见 [docs/design.md](docs/design.md)。

## 示例

完整的示例记忆文件见 [examples/sample-DEV_MEMORY.md](examples/sample-DEV_MEMORY.md)。

## 技术规格

- **版本**：v3.0
- **架构**：三层混合分层 + 寄生式注入
- **核心原则**：17 条（含时序感知、条件触发、模块化文件、显式遗忘、自动关联检测）
- **触发方式**：双触发（开头检查 + 结尾硬触发）+ 寄生式注入（Rule 级优先级）+ 条件触发（路径映射）
- **执行流程**：7.5 步（多项目识别 → 分诊 → 读取 → 提取 → 对比+自动关联 → 写入+JSONL+摘要 → 进化 → Dream → 反馈）
- **开头检查**：9 步（含时效扫描、Dream 摘要加载、条件触发路径匹配）
- **Dream 操作**：7 个（合并/去重/提升/引用频率/交叉引用/过期处理/遗忘归档）
- **输出格式**：Markdown（项目级/会话级）+ JSONL（消息级，10 字段，必选）
- **归档策略**：sessions/ 完整保留消息级记录；显式遗忘条目归档到 archived.jsonl
- **时间精度**：精确到分钟（`YYYY-MM-DD HH:MM`）
- **兼容性**：任何能读取 Markdown 的工具/平台

## 参赛信息

本项目参加 **TRAE AI 创造力大赛**（学习工作赛道）。

- 大赛官网：https://www.trae.cn/ai-creativity
- 社区专区：https://forum.trae.cn/c/38-category/38

## License

MIT License — 详见 [LICENSE](LICENSE)
