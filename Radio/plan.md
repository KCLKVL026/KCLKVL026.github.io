# 音频播放器网页计划

## 目标
在 `Radio/` 目录下创建一个自包含的音频播放器 HTML 页面，用于播放 `Circuit_Analysis/` 中的音频文件。

## 技术约束
- 纯静态站点，托管于 GitHub Pages，无服务端逻辑
- 目前 `Circuit_Analysis/` 下有 1 个文件：`01_DC_Circuit_Analysis-Circuit_Laws_Discussion.m4a`
- 未来会持续添加更多音频文件

## 方案：使用 GitHub API 动态获取文件列表

通过 GitHub Contents API 获取 `Circuit_Analysis/` 目录下所有文件，自动构建播放列表。这样添加新音频后无需修改 HTML，只需 push 即可。

API 端点：
```
https://api.github.com/repos/KCLKVL026/KCLKVL026.github.io/contents/Radio/Circuit_Analysis
```

## 文件结构
```
Radio/
  index.html          ← 新建：音频播放器页面
  plan.md             ← 本计划文件
```

## 页面功能

### 1. 播放列表区域
- 页面加载时调用 GitHub API 获取 `Radio/Circuit_Analysis/` 目录下所有音频文件
- 过滤出音频格式：`.m4a` `.mp3` `.wav` `.ogg` `.flac` `.aac`
- 以列表形式展示文件名（去除扩展名），点击可播放
- 当前播放项高亮显示
- 显示文件总数

### 2. 播放器控制区
- 播放/暂停按钮
- 上一曲/下一曲按钮
- 进度条（可拖拽 seek）
- 当前时间 / 总时长显示
- 音量控制滑块
- 播放模式切换：顺序播放 / 单曲循环 / 列表循环

### 3. 样式设计
- 深色主题，现代简洁风格
- 使用 Tailwind CSS（CDN）实现响应式布局
- 左侧或上方为播放列表，右侧或下方为播放器控制
- 移动端适配

### 4. 技术细节
- 单文件 HTML，内联 `<style>` + `<script>`
- Tailwind CSS v3 via CDN
- 纯原生 HTML5 Audio API，无第三方播放器库
- 音频 URL 通过 GitHub API 返回的 `download_url` 拼接（`raw.githubusercontent.com` 链接）
- 错误处理：API 请求失败时显示友好提示，支持离线时手动刷新

## 实施步骤

1. 创建 `Radio/index.html`
2. 实现 GitHub API 文件列表获取逻辑
3. 实现播放列表 UI 和交互
4. 实现音频播放器控制逻辑（播放、暂停、进度、音量、模式切换）
5. 响应式样式适配
6. 测试验证
