# 图层命名规范参考

## 通用命名规则

### 基本原则

1. **语义优先**：名称应描述"这是什么"而非"它长什么样"
2. **简洁明确**：2-5个字/词为宜，避免过长命名
3. **层级清晰**：通过命名体现层级关系
4. **团队统一**：文件内命名风格保持一致

### 命名格式选择（团队选定一种）

| 格式 | 示例 | 适用场景 |
|------|------|----------|
| 中文 | 首页容器、用户头像、主按钮 | 国内团队、非技术背景成员 |
| 英文驼峰 | homeContainer, userAvatar, primaryBtn | 技术导向团队、需对接开发 |
| 英文短横线 | home-container, user-avatar, primary-btn | CSS 对齐、前端友好 |
| 中英混合 | 容器-Home, 头像-Avatar, 按钮-Primary | 过渡期团队 |

### 常见元素命名参考

#### 容器类

| 默认名 | 规范命名 |
|--------|----------|
| Frame 1 | 首页容器 / homeContainer |
| Frame 2 | 列表容器 / listContainer |
| Group 1 | 卡片组 / cardGroup |
| Frame 3 | 头部导航 / headerNav |
| Frame 4 | 底部栏 / footerBar |

#### 基础元素类

| 默认名 | 规范命名 |
|--------|----------|
| Rectangle 1 | 背景 / background |
| Rectangle 2 | 分割线 / divider |
| Rectangle 3 | 卡片 / card |
| Ellipse 1 | 头像 / avatar |
| Line 1 | 分隔线 / separator |
| Vector 1 | 图标-搜索 / icon-search |

#### 交互元素类

| 默认名 | 规范命名 |
|--------|----------|
| Rectangle + Text | 主按钮 / btnPrimary |
| Rectangle + Text | 次按钮 / btnSecondary |
| Input Field | 搜索框 / searchInput |
| Checkbox | 勾选项 / checkbox |

#### 图片类

| 默认名 | 规范命名 |
|--------|----------|
| Image 1 | 封面图 / coverImage |
| Image 2 | 产品图 / productImage |
| Image 3 | 占位图 / placeholder |

### 命名层级规范

```
页面层：🏠 首页
  └── 区块层：轮播区
       └── 组件层：轮播卡片
            ├── 内容层：标题
            ├── 内容层：描述
            └── 内容层：指示器
```

### 特殊标记

| 前缀 | 含义 | 示例 |
|------|------|------|
| 🗑 | 待删除 | 🗑 旧版头部 |
| 📌 | 锁定不移动 | 📌 全局导航 |
| ⚡ | 交互状态 | ⚡ Hover态 |
| 🔧 | 开发标注 | 🔧 安全区域 |

## 与 design-review 的分工

- **本参考**关注：图层叫什么名、是否语义化、格式是否统一
- **design-review**关注：界面好不好用、交互是否合理、视觉层级是否清晰
