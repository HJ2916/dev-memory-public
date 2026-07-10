# dev-memory — TRAE 适配

## 安装

```bash
# TRAE 中国版
mkdir -p ~/.trae-cn/skills/dev-memory
cp SKILL.md ~/.trae-cn/skills/dev-memory/SKILL.md

# TRAE 国际版
mkdir -p ~/.trae/skills/dev-memory
cp SKILL.md ~/.trae/skills/dev-memory/SKILL.md
```

## 触发机制

TRAE 的 Skill 系统自动加载 `~/.trae-cn/skills/` 下的 SKILL.md 文件。安装后无需额外配置，skill 会在对话开始和结束时自动触发。

## 特殊说明

- TRAE 是 dev-memory 的原生平台，支持最完整的双触发机制
- TRAE 的 Skill 系统会自动将 SKILL.md 内容注入到 AI 上下文中
- 无需格式转换，直接使用原始 SKILL.md
