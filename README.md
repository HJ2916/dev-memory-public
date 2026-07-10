# dev-memory — 项目级 AI 记忆系统

> 把开发记忆变成随项目走的轻量资产，不绑账号、不绑工具、不绑平台。

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![TRAE Skill](https://img.shields.io/badge/TRAE-Skill-blue)](https://www.trae.cn/)

## 这是什么

**dev-memory** 是一个项目级 AI 记忆系统。在 AI 工具高速迭代的时代，工具会换、账号会变、模型会更新，唯独你的项目和设备是稳定的锚点。dev-memory 把开发记忆的归属权从账号和工具上拿下来，放到项目这一层——用纯 Markdown 文件存在项目目录里，跟着 Git 走，不绑任何平台。

AI 行业发展至今，已有针对"用户"的记忆系统（ChatGPT Memory），针对"Agent 工具"的记忆系统（Claude Code CLAUDE.md、Hermes Agent）。dev-memory 填补了缺失的一环：**针对"项目和设备"的记忆系统**。

## 核心特性

| 特性 | 说明 |
|------|------|
| 双触发机制 | 开头检查建立保存意识 + 结尾硬触发（任务完成/话题切换），不依赖用户说特定的话 |
| 六维记忆提取 | 任务摘要、踩坑记录、探索路径、最终结论、架构决策、环境信息 |
| 分诊过滤 | 不值得保存的对话直接跳过，避免无效 IO |
| 追加式演进 | 旧记忆永不删除，只追加更正说明，完整保留知识演进轨迹 |
| 轻量 Dream 整理 | 借鉴 Claude Code autoDream，内联执行记忆巩固（合并/去重/提升），不需要 daemon |
| 增量写入 | 只输出新条目和插入位置，不重写未变化部分 |
| 价值标记 | 每条记忆标注 ★★★/★★/★，低价值记忆优先归档 |
| 健康度评分 | 0-100 分，5 维度评估记忆系统状态 |
| 多工具适配 | 支持 10 个主流 AI 编程工具，一键安装 |
| 隐私感知 | 记忆文件默认 gitignore，公开仓库忽略/私有仓库建议追踪 |
| 零依赖 | 纯 Markdown 文件操作，不执行任何脚本，换任何工具都能读 |
| 按周归档 | 主文件保持精简（每维5条），旧条目自动归档到周文件 |
| 自我进化 | Skill 自身预留迭代记录区域，含触发可靠性审计 |

## 支持的 AI 工具

| 工具 | 适配方式 | 详细说明 |
|------|---------|---------|
| **TRAE** | `~/.trae-cn/skills/dev-memory/SKILL.md` | [adapters/TRAE.md](adapters/TRAE.md) |
| **Claude Code** | `~/.claude/commands/` 或 CLAUDE.md 引用 | [adapters/claude-code.md](adapters/claude-code.md) |
| **Codex CLI** | 项目 `AGENTS.md` 追加引用 | [adapters/AGENTS.md](adapters/AGENTS.md) |
| **Cursor** | `.cursor/rules/dev-memory.mdc` | [adapters/cursor.md](adapters/cursor.md) |
| **GitHub Copilot** | `.github/copilot-instructions.md` 追加引用 | [adapters/copilot.md](adapters/copilot.md) |
| **Windsurf** | `.windsurfrules` 追加引用 | [adapters/windsurf.md](adapters/windsurf.md) |
| **Cline** | `.clinerules` 追加引用 | [adapters/cline.md](adapters/cline.md) |
| **CodeBuddy** | `.codebuddy/rules/dev-memory.md` | [adapters/codebuddy.md](adapters/codebuddy.md) |
| **OpenCode / MiMoCode** | 项目 `AGENTS.md` 追加引用 | [adapters/AGENTS.md](adapters/AGENTS.md) |
| **通用** | 粘贴到任意工具的自定义指令 | [adapters/generic.md](adapters/generic.md) |

## 快速开始

### 1. 安装 Skill

根据你使用的 AI 工具，参考上方表格选择安装方式。TRAE 用户：

```bash
mkdir -p ~/.trae-cn/skills/dev-memory
cp SKILL.md ~/.trae-cn/skills/dev-memory/SKILL.md
```

### 2. 自动工作

安装后自动生效——对话开始时静默检查记忆状态，任务完成或话题切换时自动保存记忆。无需手动操作。

### 3. 查看记忆

```
你的项目/
  docs/
    dev-memory/
      CONFIG.md              ← 配置和安装引导（纳入 Git）
      DEV_MEMORY.md          ← 主记忆文件（默认 gitignore）
      archive/               ← 按周归档（默认 gitignore）
        2026-W28.md
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
- **用纯 Markdown Skill 实现**，工具兼容但不绑定

详细设计思路见 [docs/design.md](docs/design.md)。

## 示例

完整的示例记忆文件见 [examples/sample-DEV_MEMORY.md](examples/sample-DEV_MEMORY.md)。

## 技术规格

- **版本**：v1.3
- **触发方式**：双触发（开头检查 + 结尾硬触发）
- **执行流程**：8.5 步（分诊 → 多项目识别 → 读取 → 提取 → 对比 → 写入 → 归档 → 进化 → Dream → 反馈）
- **输出格式**：纯 Markdown 文件
- **归档策略**：ISO 周数归档，价值感知（★ 优先归档）
- **时间精度**：精确到分钟（`YYYY-MM-DD HH:MM`）
- **兼容性**：任何能读取 Markdown 的工具/平台

## 参赛信息

本项目参加 **TRAE AI 创造力大赛**（学习工作赛道）。

- 大赛官网：https://www.trae.cn/ai-creativity
- 社区专区：https://forum.trae.cn/c/38-category/38

## License

MIT License — 详见 [LICENSE](LICENSE)
