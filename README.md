# 🔍 Design File Audit — 设计文件规范检查

> AI 驱动的 Figma 设计文件规范检查工具，六维度自动审查设计文件质量，输出分级违规清单。

设计到开发流程第一步：**检查文件规范 → 评审设计质量 → 生成设计规范 → 验收 UI 实现**。

## ✨ 核心能力

| 能力 | 说明 |
|------|------|
| **六维度检查** | 图层命名、组件使用、样式管理、文件结构、Auto Layout、设计 Token 一致性 |
| **分级违规** | 🔴 高（必须修复）/ 🟡 中（建议修复）/ 🟢 低（优化建议） |
| **去重统计** | 同一组件实例的重复违规自动合并，不重复计数 |
| **多层级检查** | 项目级（全文件）/ 页面级（单页）/ 画板级（单画板） |
| **精确修复值** | 每条违规给出具体修复建议和目标值 |

## 📁 文件结构

```
design-file-audit/
├── README.md                           ← 本文件
├── SKILL.md                            ← WorkBuddy 完整版 Skill
├── portable-design-file-audit.md       ← 跨平台便携版（Cursor / Claude Code / Codex）
├── .gitignore
├── figma-design-file-audit-release.md   ← Figma Make 专用版
└── references/
    ├── naming-conventions.md            ← 图层命名规范参考
    ├── file-structure-autolayout.md     ← 文件结构与 Auto Layout 规范
    └── component-style-guidelines.md    ← 组件与样式管理规范
```

## 🚀 安装

### WorkBuddy

```bash
git clone https://github.com/YOUR_USERNAME/design-file-audit.git
cp -r design-file-audit ~/.workbuddy/skills/design-file-audit
```

### Cursor

```bash
mkdir -p .cursor/rules
cp portable-design-file-audit.md .cursor/rules/design-file-audit.mdc
```

### Claude Code

```bash
cat portable-design-file-audit.md >> CLAUDE.md
```

### Figma Make

将 `figma-design-file-audit-release.md` 文件上传到 Figma Make skill。

## 📖 使用方式

**触发词**（任一即可）：
- "帮我检查这个文件"
- "文件规范吗" / "图层命名对不对"
- "组件用得对不对" / "设计文件自查"
- "检查规范"

**输入**：Figma 文件链接 / 截图 / 文件描述

## 🔗 配套 Skill

| Skill | 说明 |
|-------|------|
| [design-review](https://github.com/YOUR_USERNAME/design-review) | 设计评审（交互/视觉质量） |
| [design-spec-generator](https://github.com/YOUR_USERNAME/design-spec-generator) | 设计规范文档生成 |
| [ui-acceptance-checker](https://github.com/YOUR_USERNAME/ui-acceptance-checker) | UI 验收（Figma vs 实现对比） |

## 📄 License

MIT
