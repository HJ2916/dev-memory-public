# 项目开发记忆

> 最后更新: 2026-07-10 18:10 | 会话: 6a50bc3f | 周归档: 2026-W28

## 当前状态概览

dev-memory 项目完成 v1.0 发布和 v1.1 升级。v1.0 实现六维记忆提取、按周归档、追加式更正、自我进化；v1.1 新增多项目识别与记忆分发功能。项目已推送至 GitHub 私密仓库，参赛文件（创意提案 HTML、交互式 Demo HTML、报名帖正文）已就绪，准备提交 TRAE AI 创造力大赛（学习工作赛道）。

## 六维记忆索引

### 1. 任务与变更摘要

- [2026-07-10 18:05] 完成 v1.1 升级：新增 Step 0（多项目识别与记忆分发），支持一个 workspace 内多项目、一次会话跨多项目的场景，执行流程从 6 步变为 7 步
- [2026-07-10 17:34] 重写创意提案 HTML，拔高定位为"项目级 AI 记忆系统"，加入 AI 记忆历史脉络（用户级→Agent级→项目级），对比 Claude Code 和 Hermes，强调数据主权和中国用户友好性
- [2026-07-10 17:20] 创建参赛文件：创意提案 HTML、交互式 Demo HTML、参赛指南文档、报名帖正文
- [2026-07-10 17:00] 初始化项目结构（SKILL.md、README.md、LICENSE、.gitignore、CHANGELOG.md、设计文档、示例文件），创建 Git 仓库并推送至 GitHub 私密仓库
- [2026-07-10 16:50] 安装 GitHub CLI (winget install GitHub.cli)，完成认证（账号 HJ2916）

### 2. 踩坑记录

- [2026-07-10 17:05] ✅ GitHub push 失败 "Failed to connect to github.com port 443" → 原因: git 全局 https.proxy 未配置，只有 http.proxy → 修正: `git config --global https.proxy http://127.0.0.1:9674`，并设置环境变量 HTTP_PROXY/HTTPS_PROXY/ALL_PROXY
- [2026-07-10 17:00] ✅ gh CLI 未安装 → 原因: 新环境未预装 → 修正: `winget install --id GitHub.cli`
- [2026-07-10 17:10] ✅ Demo HTML 子代理保存到错误路径(workspace根目录而非 dev-memory/competition) → 原因: 子代理对工作目录理解偏差 → 修正: 手动 Move-Item 移到正确位置

### 3. 探索路径

- [2026-07-10 17:30] 创意提案定位: 初版只写"AI记忆持久化"(太平) → 加入AI记忆历史脉络(用户级/Agent级/项目级) → 对比Claude Code(CLAUDE.md)和Hermes(四层记忆) → 强调Claude Code对中国用户区别对待的现实背景 → 最终定位为"项目级AI记忆系统，融合Claude Code和Hermes优点的轻量替代品"
- [2026-07-10 16:55] GitHub仓库创建: gh repo create --private --source . --push(一步完成创建+关联+推送) → push失败(代理问题) → 修复代理后重新push成功

### 4. 最终结论与最佳实践

- [2026-07-10 18:05] 多项目识别功能是 dev-memory 的关键差异化：实际开发中一个 workspace 经常有多个项目，一次会话可能跨项目，机械写入当前目录会导致记忆遗漏
- [2026-07-10 17:30] 参赛定位核心叙事线：AI记忆已有用户级(ChatGPT)和Agent级(Claude Code/Hermes)，缺项目级；只有项目和设备才是开发者真正拥有的稳定资产
- [2026-07-10 17:05] Windows 环境下 git push GitHub 需同时配置 http.proxy 和 https.proxy，且需设置环境变量 ALL_PROXY
- [2026-07-10 17:00] gh CLI 可通过 winget 一键安装：`winget install --id GitHub.cli`
- [2026-07-10 16:50] `gh repo create <name> --private --source . --push` 可一步完成仓库创建、远程关联和代码推送

### 5. 架构决策记录

- [2026-07-10 18:05] 多项目识别用"线索优先级"而非自动推断：文件路径>项目名称>代码特征>已有记忆>当前目录 → 原因: 精确度递减，高优先级线索可靠，低优先级兜底
- [2026-07-10 17:30] 参赛文件与项目代码分离存放：competition/ 目录独立于核心代码 → 原因: 参赛材料是临时性的，项目代码是长期资产，分离便于后续管理
- [2026-07-10 17:00] GitHub 仓库设为私密 → 原因: 参赛要求作品不得在公开平台展示参赛成品形态，初赛时可附链接供评审查看

### 6. 环境与依赖信息

- [2026-07-10 18:05] dev-memory v1.1: 新增 Step 0 多项目识别，7步执行流程
- [2026-07-10 17:00] GitHub 仓库: https://github.com/HJ2916/dev-memory (PRIVATE)
- [2026-07-10 17:00] GitHub CLI: gh 2.96.0 (winget 安装)
- [2026-07-10 17:00] Git 代理: http://127.0.0.1:9674 (http.proxy + https.proxy)
- [2026-07-10 16:50] 项目目录: D:\trae\traeworkspace\dev-memory
- [2026-07-10 16:50] TRAE skill 副本: C:\Users\JacobWu\.trae-cn\skills\dev-memory\SKILL.md

## 历史更正记录

（首次初始化，尚无更正记录。后续执行中发现旧记忆有误时在此追加。）

## 归档索引

（首次初始化，尚无归档文件。当主文件条目溢出时将自动创建归档。）
- 2026-W28 (2026-07-07 ~ 2026-07-13) → archive/2026-W28.md (待创建)
