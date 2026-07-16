# Changelog

所有重要变更记录在此文件中。格式基于 [Keep a Changelog](https://keepachangelog.com/)。

## [3.1.1] - 2026-07-16

### Fixed
- **关键修复**：SKILL.md 顶部添加 YAML front matter（`---name/description---`），修复 TRAE 等工具无法识别和注册 Skill 的问题
- 所有 9 个适配器（adapters/）统一更新："安装 Skill（可选）"改为"安装 Skill（推荐）"，添加 YAML front matter 警告和安装验证步骤
- README.md "快速开始"新增 Step 1 "安装 Skill"步骤，明确两步安装流程（Skill 安装 + 触发规则注入）

### Added
- 每个适配器新增"安装验证"章节，列出 3 步验证方法
- README.md 新增"为什么需要两步？"说明（Skill 提供执行规范，触发规则确保自动触发）

## [3.1.0] - 2026-07-15

### Added
- 记忆缺口检测（开头检查 Step 10）：JSONL 时间戳 vs 代码文件 mtime，自动发现跨会话遗漏并补录
- Step 5.5 合规自检：6 项文件逐项检查（DEV_MEMORY/JSONL/摘要/PROFILE/metadata/索引），杜绝"部分执行"
- 跨会话隔离限制说明
- 迁移后新对话建议

### Changed
- 核心原则 17→19 条
- 开头检查 8→10 步
- 执行流程 7→7.5 步

## [3.0.0] - 2026-07-14

### Added
- 自动关联检测（原则#17）：Step 4 对比检查时自动检测新条目与已有条目的关键词重合度（≥2 个关键名词匹配），自动递增引用计数并追加交叉关联标记
- health-report 评估指标增强：引用率、覆盖率、时效健康度量化指标
- 解决引用频率偏低问题（v2.2 时 44/48 条为零引用）

### Changed
- 核心原则 16→17 条
- SKILL.md Description 行补充 v2.2/v3.0 功能描述
- CHANGELOG.md 补全 v1.1 到 v2.2 全部版本记录

### Fixed
- docs/design.md 标注为历史文档（v1.0 设计），指向当前 SKILL.md 和 README.md

## [2.2.0] - 2026-07-14

### Added
- 条件触发（原则#14）：CONFIG.md 路径-记忆映射表（YAML），开头检查步骤 9 按操作路径加载匹配记忆
- 模块化文件（原则#15）：`@dimensions/XX-name.md` 引用语法，单文件膨胀时自动拆分
- AGENTS.md 跨工具统一适配：Cursor/Claude Code/Codex/Qwen Code 统一入口
- 显式遗忘（原则#16）："忘记 [关键词]" → `[~💀]` 标记 → Dream 操作⑦ → `sessions/archived.jsonl`
- CONFIG.md 升级：路径映射表 + 遗忘指令 + 新文件说明表
- .gitignore 更新：+health-report.md +dimensions/

### Fixed
- 核心原则标题计数 12→16
- 9 个适配器触发规则统一升级到 v2.2 标准（5 条规则）
- README.md 更新到 v2.2（特性表 +6 项、文件结构 +4 文件、技术规格全面更新）
- DEV_MEMORY.md 当前状态概览从 v2.0 更新到 v2.2
- health-report.md 从 Git 追踪中移除（已在 .gitignore 中）

## [2.1.1] - 2026-07-14

### Fixed
- Dream 冷却期：距上次 <3 天跳过（仅标记"待整理"），过期 >5 条除外
- JSONL 即时追加强制化：Step 5.2 新增即时追加原则，不得批量补录
- 开头检查措辞强制化：从"静默执行"改为"必须生成回复前执行，不得跳过"
- ⑥ 维度有效期补全：v1.2/v1.3/gh CLI/Git 代理 4 条标记 [有效期:2026-10-10]
- TRAE 适配器触发规则同步更新

## [2.1.0] - 2026-07-14

### Added
- 时序感知（原则#13）：`[有效期: YYYY-MM-DD]` 标记 + `[~⏰]` 过期检测 + Dream 操作⑥（过期处理）
- 异步 Dream 整理：Step 6.5 改为任务后反馈前执行，结果写入 dream.md
- 记忆摘要页：health-report.md 健康度仪表盘（总览/引用Top5/过期/Dream历史/维度健康）
- JSONL valid_until 字段
- 开头检查步骤 7-8（过期扫描 + Dream 摘要加载）

### Fixed
- 引用计数修复：⑤ 寄生式注入条目 [引用:0]→[引用:1]
- 首次 Dream 执行：42 条 >24 阈值，3 条交叉引用 + 5 条有效期标记
- metadata 条目计数修正：31→42

## [2.0.0] - 2026-07-14

### Added
- 三层混合分层架构：项目级（DEV_MEMORY.md）+ 会话级（sessions/MD）+ 消息级（sessions/JSONL）
- 寄生式注入（薄 Rule + 厚 Skill）：触发规则注入 9 个工具最高优先级层
- USER_PROFILE.md：贡献者日志 + 设备溯源
- 取消周归档：sessions/ 完整保留所有消息级记录
- 7.5 步执行流程（从 8.5 步简化）
- 12 条核心原则、9 个适配器、JSONL 10 字段

### Changed
- 所有路径从 docs/dev-memory/ 改为 dev-memory/
- 溢出处理：[~] 标记 + Dream 跨层合并代替归档

## [1.3.0] - 2026-07-10

### Added
- 轻量 Dream 整理（Step 7.5）：合并/去重/提升 + 引用频率追踪
- 多工具适配：adapters/ 目录含 9 个文件覆盖 10 个工具
- CONFIG.md 配置文件
- 隐私感知 Git 策略
- 健康度评分（5 维度/0-100 分）
- 记忆指纹（metadata 摘要）

## [1.2.0] - 2026-07-10

### Added
- 双触发机制（开头检查 + 结尾硬触发）
- 分诊过滤（跳过低价值对话）
- 增量写入（降低 40% 写入成本）
- 分层读取（降低 87% 检查成本）
- 价值标记（★/★★/★★★）
- 时效性检测（>14 天标记过时）
- 会话去重
- 自我审计
- 主动初始化

### Changed
- 执行流程从 7 步变为 8 步

## [1.1.0] - 2026-07-10

### Added
- Step 0：多项目识别与记忆分发
- 支持一个 workspace 内多项目、一次会话跨多项目

### Changed
- 执行流程从 6 步变为 7 步

## [1.0.0] - 2026-07-10

### Added
- 六维记忆提取：任务摘要、踩坑记录、探索路径、最终结论、架构决策、环境信息
- 主文件 + 按周归档机制（ISO 周数）
- 追加式更正机制，旧记忆不删除
- 对话结束时自动触发
- 首次初始化支持从 TRAE 内置 memory 迁移
- 溢出阈值：摘要/踩坑/结论/环境 5 条，探索/决策 3 条
- 自我进化检查机制
