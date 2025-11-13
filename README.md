# BioArk Header Carousel

仿照 [BioArk Technologies](https://www.bioarktech.com/) 首页头图，实现图片渐变 + 文案滑入的轮播效果。每张图片和文案独立配置，支持自动播放与左右按钮切换。

## 技术栈

- Vue 3 + `<script setup>` 组合式 API
- Vite (开发构建)
- TypeScript
- 原生 CSS 过渡与 `<transition>` 组件

## 项目结构概览

```
src/
├─ App.vue                # 页面框架（导航 + 头图）
├─ components/
│  └─ HeroCarousel.vue    # 头图轮播组件
└─ main.ts                # 入口文件
```

## 启动方式

```bash
npm install
npm run dev
```

访问终端输出的本地地址（默认 http://localhost:5173）。

## 关键实现说明

- 轮播数据：在 `HeroCarousel.vue` 内以数组配置图片、标签、标题、描述和统计信息，可按需扩展。
- 自动播放：`setInterval` 控制间隔（默认 7 秒），鼠标移入暂停，移出继续。
- 图片切换：使用 `opacity` 渐变实现淡入淡出，没有位移滑动效果。
- 文案切换：利用 `<transition name="text-slide">` 实现上下滑入/滑出动画，与当前索引同步。
- 手动切换：左右按钮 + 底部指示器均可直接跳转指定轮播项。

