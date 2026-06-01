---
name: design-file-audit
description: >
  Audit Figma design files against six dimensions of engineering standards:
  layer naming, component usage, style management, file structure, Auto Layout,
  and design token consistency. Use this skill when the user asks to check a
  Figma file for规范 compliance, naming issues, style violations, missing Auto
  Layout, or design debt. Reports violations with severity levels (high/medium/low),
  deduplicates by component instance, and provides exact修复 values. 
  For design quality review (not规范检查), use design-review instead.
---

# 设计文件规范检查 — Figma Make 专用版

> **两种使用方式**：
> - **上传**：将 `figma-community-skill` 文件夹上传到 Figma Community Skills 平台
> - **粘贴**：打开本文件 → 全选复制 → 粘贴到 Figma Make 对话框即可
>
> Figma Make 拥有当前 Figma 文件的直接访问能力，无需任何 API 配置。

严谨的 Figma 文件工程规范检查，覆盖六大维度。报告按问题模式分组、附带精确节点名定位、给出具体修复值。

---

## Figma 文件结构导航规则

检查时按 Figma 原生层级遍历：

```
Document
 └─ Page[] 页面列表
     └─ Frame[] / Component[] / Group[] 顶层节点
         └─ 子节点嵌套树 Frame / Text / Rectangle / Instance / Group
```

**关键节点属性速查**：

| 属性 | 用途 | 取值示例 |
|------|------|----------|
| `name` | 图层名称 | "Frame 1"、"主按钮" |
| `type` | 节点类型 | FRAME, TEXT, RECTANGLE, ELLIPSE, COMPONENT, INSTANCE, GROUP |
| `visible` | 可见性 | true / false |
| `fills` | 填充色数组 | `[{type:"SOLID", color:{r,g,b}},...]` |
| `fillStyleId` | 关联的 Color Style ID | `"S:xxx"` 或空字符串 |
| `strokes` | 描边数组 | `[{type:"SOLID", color:{r,g,b}},...]` |
| `strokeStyleId` | 关联的描边 Style ID | `"S:xxx"` 或空 |
| `effects` | 效果数组 | `[{type:"DROP_SHADOW",...}]` |
| `effectStyleId` | 关联的 Effect Style ID | `"S:xxx"` 或空 |
| `fontName` | 字体 | `{family:"PingFang SC", style:"Medium"}` |
| `fontSize` | 字号 px | 12, 14, 24 |
| `textStyleId` | 关联的 Text Style ID | `"S:xxx"` 或空 |
| `cornerRadius` | 圆角 px | 8, 12 |
| `layoutMode` | Auto Layout 模式 | "NONE" / "HORIZONTAL" / "VERTICAL" |
| `paddingLeft/Right/Top/Bottom` | AL 内边距 | 16, 24 |
| `itemSpacing` | AL 间距 | 8, 12 |
| `layoutSizingHorizontal` | 水平尺寸 | "FIXED" / "HUG" / "FILL" |
| `layoutSizingVertical` | 垂直尺寸 | "FIXED" / "HUG" / "FILL" |
| `componentId` | 实例指向的组件 ID | 非空表示 Instance |
| `children` | 子节点数组 | — |

---

## 六大检查维度

对每个可视节点逐项检查。**`visible: false` 的节点及子节点全部跳过。**

---

### 1. 图层命名规范

| # | 检查规则 | 判断逻辑 | 严重度 |
|---|----------|----------|--------|
| 1.1 | 无默认命名 | `name` 匹配 `Frame \d+` / `Group \d+` / `Rectangle \d+` / `Ellipse \d+` / `Vector \d+` / `Component \d+` / `Line \d+` | 🔴 高 |
| 1.2 | 无拷贝后缀 | `name` 匹配 `.+ \d+$`（末尾空格+数字，如"按钮 1"） | 🟡 中 |
| 1.3 | 命名语义化 | `name` 为无意义词如 "Image"、"Rectangle"（不含数字后缀） | 🟡 中 |
| 1.4 | 同级风格统一 | 同一 Frame 下同级节点命名混用中英文格式 | 🟢 低 |
| 1.5 | 冗余前缀 | `name` 包含与父级重复前缀（父 Frame "卡片"，子节点 "卡片标题"） | 🟢 低 |

---

### 2. 组件使用规范

| # | 检查规则 | 判断逻辑 | 严重度 |
|---|----------|----------|--------|
| 2.1 | 组件实例覆盖 | `type`=INSTANCE 且与相邻同类视觉元素一致但非 INSTANCE | 🔴 高 |
| 2.2 | 主组件位置 | `type`=COMPONENT 但所在 Page 名不含"组件"/"Component"/"📦" | 🔴 高 |
| 2.3 | 组件嵌套深度 | COMPONENT/INSTANCE 嵌套层级大于 4 | 🟡 中 |
| 2.4 | 重复组件 | 多个 COMPONENT 视觉相同但各自独立 | 🟡 中 |
| 2.5 | 属性缺失 | COMPONENT 含 Variant 但缺 State/Size/Type 属性 | 🟡 中 |

---

### 3. 样式管理规范

核心原则：所有颜色/文字/效果必须关联 Style，裸值一律违规。

| # | 检查规则 | 判断逻辑 | 严重度 |
|---|----------|----------|--------|
| 3.1 | 裸填充色 | `fills` 含 SOLID 但 `fillStyleId` 为空 | 🔴 高 |
| 3.2 | 裸文字样式 | `type`=TEXT 且 `textStyleId` 为空 | 🔴 高 |
| 3.3 | 裸描边 | `strokes` 含 SOLID 但 `strokeStyleId` 为空 | 🔴 高 |
| 3.4 | 裸效果 | `effects` 非空但 `effectStyleId` 为空 | 🟡 中 |
| 3.5 | 样式命名混乱 | 文件内 Color Style 同时存在 semantic 和 token 两套体系 | 🟡 中 |
| 3.6 | 废弃样式 | Color/Text Style 未被任何节点引用 | 🟢 低 |
| 3.7 | 样式重复 | 同一色值同一语义 2 个以上 Color Style（不同语义允许） | 🔴 高 |

---

### 4. 文件结构规范

仅项目级/页面级执行，画板级跳过。

| # | 检查规则 | 判断逻辑 | 严重度 |
|---|----------|----------|--------|
| 4.1 | 页面命名 | Page `name` 为 "Page 1" 等默认名 | 🔴 高 |
| 4.2 | 排序规范 | Page 未按「组件库→规范→迭代→废弃」排序，中间无分割线页 | 🔴 高 |
| 4.3 | 版本页命名 | 迭代页 `name` 不含「日期+版本号+迭代名」三项（`YYYY.MM.DD Vx.x 名称`） | 🔴 高 |
| 4.4 | 废弃画板 | Page 中存在标记为废弃的 Frame 未移至「废弃」页 | 🔴 高 |
| 4.5 | Cover 缺失 | 未设置封面 Frame | 🟢 低 |

---

### 5. Auto Layout 规范

| # | 检查规则 | 判断逻辑 | 严重度 |
|---|----------|----------|--------|
| 5.1 | 容器缺 AL | `type`=FRAME，含 2 个以上子节点且构成列表/卡片，`layoutMode`="NONE" | 🔴 高 |
| 5.2 | 固定尺寸滥用 | `layoutSizingHorizontal`="FIXED" 或 `layoutSizingVertical`="FIXED"，但内容可变 | 🟡 中 |
| 5.3 | 间距不一致 | 同一容器内 `itemSpacing` 与同级容器不统一 | 🟡 中 |
| 5.4 | Resizing 不当 | 子节点应 FILL 却设 FIXED，或反之 | 🟡 中 |
| 5.5 | 嵌套过深 | AL Frame 嵌套大于 5 层 | 🟢 低 |

---

### 6. Token 一致性

| # | 检查规则 | 判断逻辑 | 严重度 |
|---|----------|----------|--------|
| 6.1 | 圆角非 4 倍数 | `cornerRadius` 存在且 `cornerRadius % 4 != 0` | 🔴 高 |
| 6.2 | 同类型圆角不统一 | 同类型 Frame（如所有卡片）`cornerRadius` 不一致 | 🔴 高 |
| 6.3 | 字号为奇数 | `type`=TEXT，`fontSize` 为奇数（13/15/17 等） | 🔴 高 |
| 6.4 | 间距非 4 倍数 | `paddingLeft`/`itemSpacing` 等值 `% 4 != 0` | 🟡 中 |
| 6.5 | 图标尺寸非标准 | 图标 Frame 尺寸不在 16/20/24/32 标准值 | 🔴 高 |
| 6.6 | 颜色值重复 Style | 同一色值同一语义存在 2 个以上 Color Style | 🔴 高 |

---

## 同元素去重规则

1. **同一组件实例**：同一 Component 的 N 个 Instance 同类违规 → 计 1 条，标注实例数
2. **同结构子元素**：同一父容器内相同 `type` + 相同结构的子节点 → 计 1 条
3. **同属性值**：分散各处的 `cornerRadius:6` → 计 1 条「6px→8px」

---

## 输出模板

> **重要**：Figma Make 左侧面板较窄，禁止使用 Markdown 表格。每条违规信息换行展示，每行带字段标签，层次分明。

按以下结构输出：

```
# 设计文件规范检查报告

文件：[文件名] · 检查范围：[项目级/页面级/画板级] · 检查时间：[日期]
总节点：N · 违规模式：N 处 · 跳过隐藏节点：N 个

---

## 数据统计

（数据统计使用表格，仅此一处）

| 严重度 | 模式数 | 涉及节点数 |
|--------|--------|-----------|
| 高危 | N | N |
| 中危 | N | N |
| 合计 | N | 去重前原始违规约 N 处 |

---

## 问题清单

> 以下全部使用换行列表，禁止表格。每条信息独立一行，带标签前缀。

### 1. 图层命名 — 28 条

🔴 默认命名（12 条）

  · Rectangle 1
    类型：RECTANGLE
    修复：重命名为 搜索框

  · Frame 3
    类型：FRAME
    修复：重命名为 卡片容器

🟡 拷贝后缀残留（6 条）

  · 按钮 2
    类型：INSTANCE
    修复：重命名为 主操作按钮

### 2. 组件使用 — N 条

🔴 主组件错放画板（3 条）

  · btn/primary
    位置：Page:迭代
    修复：移至 Page:📦组件库

🟡 组件嵌套过深（5 条）

  · 数据卡片
    层级：6
    修复：精简至 ≤4 层

### 3. 样式管理 — 90 条

🔴 裸填充色（fillStyleId 为空）（45 条）

  · 主按钮背景
    类型：RECTANGLE
    修复：创建 Style Primary/Blue

  · 卡片底色
    类型：FRAME
    修复：创建 Style Bg/Card

🔴 裸文字（textStyleId 为空）（30 条）

  · 标题文字
    类型：TEXT
    修复：创建 Style Heading/20

🟡 裸效果（effectStyleId 为空）（15 条）

  · 导航栏
    类型：FRAME
    修复：创建 Effect Style Shadow/Nav

### 4. 文件结构 — N 条
（画板级标注"跳过"）

### 5. Auto Layout — 98 条

🔴 容器缺 Auto Layout（layoutMode:NONE）（60 条）

  · 操作按钮组
    类型：FRAME
    修复：启用 Auto Layout HORIZONTAL

  · 卡片内容区
    类型：FRAME
    修复：启用 Auto Layout VERTICAL

🟡 固定尺寸滥用（layoutSizing:FIXED）（38 条）

  · 标签容器
    类型：FRAME
    修复：HUG 改为 FILL

### 6. Token 一致性 — 4 条

🔴 圆角非 4 倍数（3 条）

  · 6px → 8px
    涉及：头像、标签

  · 14px → 16px
    涉及：卡片容器

🔴 字号为奇数（1 条）

  · 13px → 14px
    涉及：辅助文案（/文字/辅助）

---

## 批量修复路线

1. 组件源文件修复（修 1 处消 N 条）
2. 样式创建（建 Style 消裸值）
3. 属性批量修改（圆角、字号统一）
4. 命名手动修正

---

## 修复勾选清单
```
