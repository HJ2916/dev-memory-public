# dev-memory — CodeBuddy 适配

## 安装

### 方式一：项目规则（推荐）

```bash
mkdir -p .codebuddy/rules
cp SKILL.md .codebuddy/rules/dev-memory.md
```

### 方式二：User Rules

在 CodeBuddy IDE 中：Settings → Rules → User Rules，粘贴 SKILL.md 内容。

## 触发机制

CodeBuddy IDE 支持多层级的规则系统：
- **Project Rules**：存储在 `.codebuddy/rules/` 目录中，受版本控制管理，可与团队成员共享
- **User Rules**：个人偏好，全局生效

建议使用 Project Rules，确保随项目走。

## 特殊说明

- CodeBuddy 是腾讯推出的 AI 编程工具，支持 IDE + CLI + 插件三种形态
- CodeBuddy Code 2.0 支持 Skills 系统，与 dev-memory 的 Skill 概念兼容
- Project Rules 受版本控制管理，适合团队协作场景
