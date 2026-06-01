# 组件与样式管理规范参考

## 组件规范

### 主组件管理

| 规范项 | 要求 | 原因 |
|--------|------|------|
| 主组件统一存放 | Components 页面或专用组件文件 | 便于查找和维护 |
| 命名分类 | 按类别前缀：Button/Primary、Input/Search | 快速定位 |
| 变体属性 | 必要属性：State、Size、Type | 灵活复用 |
| 文档描述 | 组件添加 Description 说明用法 | 团队理解 |

### 变体命名规范

```
组件类别/组件名

属性顺序：
1. Type（类型）：Primary / Secondary / Ghost / Link
2. Size（尺寸）：Large / Medium / Small
3. State（状态）：Default / Hover / Active / Disabled
4. 其他：WithIcon / IconOnly
```

### 组件使用检查清单

```
□ 相同视觉 → 使用实例（Instance）
□ 需要修改 → 修改主组件（Main Component）
□ 临时定制 → Detach 后重命名，标注"定制-XXX"
□ 新增类型 → 先确认是否可扩展 Variant
□ 找不到组件 → 检查 Team Library 是否发布
```

## 样式规范

### Color Style 体系

#### 语义化命名（推荐）

```
color/
├── brand/
│   ├── primary        # 品牌主色
│   ├── secondary      # 品牌辅色
│   └── accent         # 强调色
├── neutral/
│   ├── text-primary   # 主文字
│   ├── text-secondary # 次文字
│   ├── text-disabled  # 禁用文字
│   ├── bg-primary     # 主背景
│   ├── bg-secondary   # 次背景
│   ├── border         # 边框
│   └── divider        # 分割线
├── semantic/
│   ├── success        # 成功
│   ├── warning        # 警告
│   ├── error          # 错误
│   └── info           # 信息
└── surface/
    ├── elevated       # 提升层
    ├── overlay        # 遮罩层
    └── modal          # 弹窗层
```

#### Token化命名（备选）

```
color/blue-50 到 color/blue-900
color/gray-50 到 color/gray-900
```

> ⚠️ 一套文件只使用一种命名体系，不得混用

### Text Style 体系

```
font/
├── display/           # 大标题
│   ├── lg (32/40/bold)
│   └── md (28/36/bold)
├── heading/           # 标题
│   ├── h1 (24/32/bold)
│   ├── h2 (20/28/semibold)
│   ├── h3 (16/24/semibold)
│   └── h4 (14/20/medium)
├── body/              # 正文
│   ├── lg (16/24/regular)
│   ├── md (14/22/regular)
│   └── sm (12/18/regular)
└── caption/           # 辅助文字
    ├── md (12/16/regular)
    └── sm (10/14/regular)
```

### Effect Style 体系

```
effect/
├── shadow/
│   ├── sm    # 轻阴影
│   ├── md    # 中阴影
│   └── lg    # 重阴影
└── blur/
    ├── sm    # 轻模糊
    └── md    # 中模糊
```

## 裸值检测（零容忍）

> ⚠️ **核心原则：只允许使用文件中定义好的样式，裸色值/裸样式一律违规。**
> 无论任何场景，颜色必须关联 Color Style，文字必须关联 Text Style，效果必须关联 Effect Style。
> 如果文件中缺少所需样式，先创建样式再应用——绝不允许跳过样式直接使用裸值。

### 裸值信号

| 类型 | 信号 | 排查方式 |
|------|------|----------|
| 裸色值 | 填充面板显示 hex 值（如 #1a1a1a）而非 Style 名 | 选中元素 → 查看 Fill 是否关联 Style |
| 裸文字 | 字体面板手动设置（如 "14px/Medium"）而非 Text Style 名 | 选中文字 → 查看 Text 是否关联 Style |
| 裸效果 | 阴影手动添加参数而非 Effect Style 名 | 选中元素 → 查看 Effects 是否关联 Style |
| 间距裸值 | 奇数间距如 3/5/7px | 检查 Auto Layout spacing 值 |

### 正确使用流程

```
需要某个颜色 → 检查 Color Style 面板
  ├── 已有对应 Style → 直接应用（点击 Style 选择）
  └── 没有对应 Style → 创建 Style（按命名体系命名）→ 应用

⚠️ 禁止操作：在颜色选择器中直接拾色/输入 hex 值后不创建 Style
⚠️ 禁止操作：复制其他元素的样式后 Detach Style
⚠️ 禁止操作：临时用一下裸色值"稍后再创建 Style"
```

### 修复优先级（全部为 🔴 高优先级）

1. 🔴 裸色值 → 创建/应用 Color Style
2. 🔴 裸文字样式 → 创建/应用 Text Style
3. 🔴 裸效果样式 → 创建/应用 Effect Style
4. 🔴 样式重复（同一色值同一语义多个 Style）→ 合并为单一 Style；同色值不同语义Token允许（如 color/brand/primary 和 color/text/link 可以同色值）
5. 🟡 间距不规范 → 调整为 4px/8px，保持4的倍数
