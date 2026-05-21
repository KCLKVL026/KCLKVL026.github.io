# 本地 CSS 替代 Tailwind 方案

## 现状分析

四个 HTML 文件（`01_DC_Resistive_Circuits.html` ~ `04_Two_Port_Networks.html`）均通过 CDN 引入 Tailwind CSS（`<script src="https://cdn.tailwindcss.com"></script>`），但 **Tailwind 仅用于"返回主页"按钮组件**，其余样式均为 `<style>` 标签内的自定义 CSS。

### 返回主页按钮用到的 Tailwind 类

| 元素 | Tailwind 类 |
|------|------------|
| 外层容器 `<div>` | `flex justify-start mb-4` |
| 按钮链接 `<a>` | `inline-flex items-center gap-1 text-sm font-medium text-blue-600 bg-blue-50 hover:bg-blue-100 hover:text-blue-800 transition-colors duration-200 rounded-full px-4 py-2 shadow-sm` |
| SVG 图标 `<svg>` | `w-4 h-4` |

共计 **15 个 Tailwind 工具类**，全部服务于这一个按钮组件。

---

## 修改计划

### 第一步：创建 `css/back-button.css`

新建文件 `./css/back-button.css`，将上述 Tailwind 类等效替换为手写 CSS：

| Tailwind 类 | 等效 CSS | 说明 |
|---|---|---|
| `flex` | `display: flex` | 弹性布局 |
| `justify-start` | `justify-content: flex-start` | 主轴起始对齐 |
| `mb-4` | `margin-bottom: 1rem` | 下方间距 16px |
| `inline-flex` | `display: inline-flex` | 行内弹性布局 |
| `items-center` | `align-items: center` | 交叉轴居中 |
| `gap-1` | `gap: 0.25rem` | 子元素间距 4px |
| `text-sm` | `font-size: 0.875rem; line-height: 1.25rem` | 14px 字号 |
| `font-medium` | `font-weight: 500` | 中等字重 |
| `text-blue-600` | `color: #2563eb` | 文字颜色 |
| `bg-blue-50` | `background-color: #eff6ff` | 浅蓝背景 |
| `hover:bg-blue-100` | `&:hover { background-color: #dbeafe }` | 悬停背景 |
| `hover:text-blue-800` | `&:hover { color: #1e40af }` | 悬停文字色 |
| `transition-colors duration-200` | `transition: color 0.2s, background-color 0.2s, border-color 0.2s` | 颜色过渡 200ms |
| `rounded-full` | `border-radius: 9999px` | 全圆角胶囊形 |
| `px-4 py-2` | `padding: 0.5rem 1rem` | 内边距 16px/8px |
| `shadow-sm` | `box-shadow: 0 1px 2px rgba(0,0,0,0.05)` | 微投影 |
| `w-4 h-4` | `width: 1rem; height: 1rem` | 16px 图标尺寸 |

CSS 文件中将使用语义化类名：

```css
/* 容器 */
.back-btn-wrap { ... }

/* 按钮链接 */
.back-btn { ... }
.back-btn:hover { ... }

/* SVG 图标 */
.back-btn svg { ... }
```

### 第二步：修改四个 HTML 文件

每个文件需做 **两处改动**：

**改动 1 — 头部引用**

```html
<!-- 删除 -->
<script src="https://cdn.tailwindcss.com"></script>

<!-- 替换为 -->
<link rel="stylesheet" href="./css/back-button.css">
```

**改动 2 — 按钮 HTML**

```html
<!-- 修改前 -->
<div class="flex justify-start mb-4">
  <a href="../index.html"
    class="inline-flex items-center gap-1 text-sm font-medium text-blue-600 bg-blue-50 hover:bg-blue-100 hover:text-blue-800 transition-colors duration-200 rounded-full px-4 py-2 shadow-sm">
    <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
      ...
    </svg>
    返回主页
  </a>
</div>

<!-- 修改后 -->
<div class="back-btn-wrap">
  <a href="../index.html" class="back-btn">
    <svg ...>
      ...
    </svg>
    返回主页
  </a>
</div>
```

### 第三步：验证

修改后在浏览器中打开任意一个 HTML 文件，确认返回按钮样式与修改前一致（蓝色胶囊形按钮、悬停变色、阴影、图标对齐）。

---

## 文件清单

| 文件 | 操作 |
|------|------|
| `./css/back-button.css` | 新建 |
| `./01_DC_Resistive_Circuits.html` | 修改头部引用 + 按钮 class |
| `./02_Sinusoidal_Steady_State.html` | 同上 |
| `./03_Transient_Analysis.html` | 同上 |
| `./04_Two_Port_Networks.html` | 同上 |
| `./css/PLAN.md` | 本文件（修改完成后可删除） |

---

## 风险与注意事项

- 本方案仅替换 Tailwind 用于按钮的 15 个工具类，不引入 Tailwind 的完整 CSS 重置/预设。
- 按钮样式已使用语义化类名（`back-btn`），后续如需微调按钮外观只需修改 `back-button.css` 一个文件。
- 如未来其他组件也需要类似按钮样式，可复用 `back-button.css` 中的类。
