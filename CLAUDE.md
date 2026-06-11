# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概述

管家玉个人简历/作品集网站。单页面静态站点，HTML/CSS/JS 全部内联在 `index.html` 中，无框架、无构建工具、无 npm 依赖。

## 开发和部署

- 直接打开 `index.html` 即可在浏览器预览，无需任何构建或启动命令
- 部署在 Vercel，直接推送静态文件即可触发部署

## 架构

`index.html` 是一个完全自包含的文件，结构如下：

1. **`<style>`（行 8-1126）**：CSS 自定义属性系统（`--bg`, `--surface`, `--text`, `--accent` 等），`body.dark` 覆盖暗色值。移动优先的响应式设计，断点在 480px / 768px / 1024px。包含打印样式（`@media print`）。
2. **HTML（行 1128-1308）**：开场动画层（`#intro-overlay`）、固定导航栏、全局浮窗（`#global-tooltip`）、主内容区（Hero、亮点、图片展示、互动卡片、经历/项目 Tabs、技能矩阵、证书、教育）、返回顶部按钮、Toast、图片灯箱。
3. **`<script>`（行 1310-1984）**：IIFE，包含所有交互逻辑。核心数据来自 `resume` 对象（行 1313-1427），所有渲染从该对象驱动。

### 关键设计模式

- **数据驱动渲染**：`resume` 对象是唯一的数据源，经历、项目、技能均通过 JS 动态插入 DOM。修改内容只需改 `resume` 对象。
- **浮窗系统**：使用 `data-has-tooltip="true"` + `data-tooltip-img` / `data-tooltip-label` 属性标记元素，脚本统一监听 hover/touch 事件显示浮窗。
- **导航 section 切换**：`showAllSections()` 显示全部，`showSectionOnly(name)` 只显示指定 `data-section` 的区块。
- **深浅色模式**：`localStorage.getItem('theme')` 持久化用户偏好，默认跟随系统 `prefers-color-scheme`。
- **开场动画**：Canvas 粒子 + 打字机效果，空格键/点击跳过。通过 `animSkipped` 标志防止重复触发。

### 文件说明

| 文件 | 用途 |
|------|------|
| `index.html` | 完整网站 |
| `tx.png` | 头像 |
| `1.png`-`6.png` | 各区块展示图片 |
| `11.png`-`44.png` | 证书图片 |
| `rst.png` | 机器人大赛证书 |
| `wx.png` | 微信二维码（浮窗展示） |
| `内蒙古工业大学-管家玉.pdf` | 可下载的 PDF 简历 |
