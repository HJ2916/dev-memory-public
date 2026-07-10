# dev-memory — AI 编程项目开发记忆持久化技能

> 让 AI 编程助手拥有"长期记忆"，换工具不丢知识，换协作者不丢上下文。

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![TRAE Skill](https://img.shields.io/badge/TRAE-Skill-blue)](https://www.trae.cn/)

## 这是什么

**dev-memory** 是一个用于 TRAE AI 编程助手的 Skill（技能），解决一个核心痛点：

> AI 编程助手（如 TRAE、Cursor、Copilot）的对话记忆是"短时记忆"——每次对话结束后，本次开发中积累的实现思路、踩坑经验、架构决策等知识就消失了。下次对话、换工具、换协作者时，又得从头探索，浪费大量算力和时间。

dev-memory 在每次对话结束时**自动提取六维开发记忆**，以纯 Markdown 文件形式保存在项目目录内，随代码版本管理。**旧记忆永不删除，新记忆以时间戳标注，对旧记忆中的错误/矛盾/过时之处进行更正和澄清。**

## 核心特性

| 特性 | 说明 |
|------|------|
| 六维记忆提取 | 任务摘要、踩坑记录、探索路径、最终结论、架构决策、环境信息 |
| 追加式演进 | 旧记忆永不删除，只追加更正说明，完整保留知识演进轨迹 |
| 零依赖 | 纯 Markdown 文件操作，不执行任何脚本，换任何工具都能读 |
| 按周归档 | 主文件保持精简（每维5条），旧条目自动归档到周文件 |
| 自我进化 | Skill 自身预留迭代记录区域，执行中发现不足时自动改进 |
| 自动迁移 | 首次使用时可从 TRAE 内置 memory 自动迁移已有项目记忆 |

## 快速开始

### 1. 安装 Skill

将 `SKILL.md` 复制到 TRAE 的 skills 目录：

```bash
# TRAE 中国版
cp SKILL.md ~/.trae-cn/skills/dev-memory/SKILL.md

# TRAE 国际版
cp SKILL.md ~/.trae/skills/dev-memory/SKILL.md
```

### 2. 自动触发

安装后，在以下场景自动触发：
- 对话结束时的自然收尾（如"好的"、"可以了"、"谢谢"）
- 一个完整任务完成后的自然收尾点
- 用户显式要求"保存记忆"或"记录开发记忆"

### 3. 查看记忆

记忆文件保存在项目的 `docs/dev-memory/` 目录下：

```
你的项目/
  docs/
    dev-memory/
      DEV_MEMORY.md           ← 主索引文件（最新状态 + 关键结论）
      archive/
        2026-W28.md           ← 按周归档
        2026-W27.md
```

## 六维记忆示例

```markdown
### 1. 任务与变更摘要
- [2026-07-10 16:30] 完成7项看板优化：标题修改、涨停分布默认显示、连板高度修复...

### 2. 踩坑记录
- [2026-07-10 16:30] ✅ 东方财富API返回rc=205 → 原因: ut和dpt参数错误 → 修正: ut=7eea...

### 3. 探索路径
- [2026-07-10 16:00] KPL API(不支持历史日期) → 东方财富(参数错误) → 修正参数后成功...

### 4. 最终结论与最佳实践
- [2026-07-10 16:30] 东方财富涨停池API正确参数: http协议 + ut=7ee... + dpt=wz.ztzt

### 5. 架构决策记录
- [2026-07-10 16:15] 连板高度放右轴(0-10)而非左轴(0-70) → 原因: 左轴数值过大导致不可见

### 6. 环境与依赖信息
- [2026-07-10 16:30] 东方财富涨停池API: http://push2ex.eastmoney.com/getTopicZTPool
```

## 为什么需要 dev-memory

### 痛点

| 场景 | 没有 dev-memory | 有 dev-memory |
|------|----------------|---------------|
| 新开对话 | AI 不知道之前做了什么，重新探索 | 读取 DEV_MEMORY.md，立即了解项目状态 |
| 换 AI 工具 | 工具级记忆丢失，从零开始 | 项目目录内的 Markdown 文件，任何工具都能读 |
| 换协作者 | 新人不知道之前的决策和踩坑 | 阅读 dev-memory，快速了解项目历史 |
| 调试问题 | 重复踩已解决过的坑 | 查阅踩坑记录，直接找到解决方案 |
| 项目交接 | 口述或写文档，容易遗漏 | 记忆随代码版本管理，完整保留 |

### 与其他方案对比

| 方案 | 优势 | 劣势 |
|------|------|------|
| dev-memory | 零依赖、随项目走、自动提取、自我进化 | 仅支持 Markdown 格式 |
| TRAE 内置 memory | 无需安装 | 工具级记忆，换工具丢失 |
| Notion/Confluence | 功能丰富 | 需手动维护、不随项目走 |
| Git commit message | 随项目走 | 仅记录代码变更，不记录思考过程 |

## 设计文档

详细的设计思路和架构决策见 [docs/design.md](docs/design.md)。

## 示例

完整的示例记忆文件见 [examples/sample-DEV_MEMORY.md](examples/sample-DEV_MEMORY.md)。

## 技术规格

- **触发方式**：对话结束时自动触发 / 用户手动触发
- **输出格式**：纯 Markdown 文件
- **归档策略**：ISO 周数归档，主文件保持精简
- **时间精度**：精确到分钟（`YYYY-MM-DD HH:MM`）
- **兼容性**：任何能读取 Markdown 的工具/平台

## 参赛信息

本项目参加 **TRAE AI 创造力大赛**（学习工作赛道）。

- 大赛官网：https://www.trae.cn/ai-creativity
- 社区专区：https://forum.trae.cn/c/38-category/38

## License

MIT License — 详见 [LICENSE](LICENSE)
