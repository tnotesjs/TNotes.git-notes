# [0015. excalidraw](https://github.com/Tdahuyou/TNotes.git-notes/tree/main/notes/0015.%20excalidraw)

<!-- region:toc -->

- [1. 📝 概述](#1--概述)
- [2. 📒 个人最喜欢的几个核心亮点](#2--个人最喜欢的几个核心亮点)
- [3. 💻 demos.1 - share](#3--demos1---share)
- [4. 💻 demos.2 - mermaid](#4--demos2---mermaid)
- [5. 🔗 References](#5--references)

<!-- endregion:toc -->

## 1. 📝 概述

- 记录一款开源的绘图工具 excalidraw。
- 评价：
  - 🤩 🤩 🤩 这个项目太棒啦～～～
  - 在接触到这个项目之前，一直在找笔记分享中，关于绘图需求的解决方案。在此之前用的是 yuque 画板来实现绘图的需求，现在（25.06）开始全盘转向 excalidraw 来实现。

## 2. 📒 个人最喜欢的几个核心亮点

- 免费
- 页面和交互体验都极其优雅。
- 支持 mermaid 解析，并且解析后的内容还支持可视化的二次编辑。
- 有网页版和 vsocde 插件版
  - 网页版可以直接在平板上实现绘图
  - vscode 插件版，可以将一个个作品以文件的形式存储下来，接下来在做一个笔记分享的时候，可以直接利用它来完成大部分的绘图工作。比如 leetcode 题解中如果需要作图说明，就可以用这个工具来实现。
  - 你可以直接复制 A（浏览器版、vscode 插件版） 画板内容粘贴到 B（vscode 插件版、浏览器版） 上，格式是完全兼容的。
- Share 功能
  - 你可以通过链接共享画板中的内容，这意味着你可以在电脑上打开网页，生成一个共享链接，然后在你的 iPad 上打开，对画板进行编辑。也可以将链接分享给其他朋友，它们可以通过链接实时看到你的画板内容，并且也可以协同编辑。
- 核心原理是基于前端技术栈实现的
  - ![图 1](https://cdn.jsdelivr.net/gh/Tdahuyou/imgs@main/2025-06-29-11-10-49.png)

## 3. 💻 demos.1 - share

- ![图 0](https://cdn.jsdelivr.net/gh/Tdahuyou/imgs@main/2025-06-29-11-06-56.png)
- 比如上图中的 mark 是在 iPad 上通过 Apple pencil 写的字，红色的是在 PC 端任意位置双击鼠标键入的文本内容，矩形和箭头元素，是画板自带的一些组件。

## 4. 💻 demos.2 - mermaid

- 支持解析 mermaid
- 支持修改字体
- 支持对解析后的内容进行二次编辑

::: swiper

![图 2](https://cdn.jsdelivr.net/gh/Tdahuyou/imgs@main/2025-06-29-11-19-17.png)

![图 3](https://cdn.jsdelivr.net/gh/Tdahuyou/imgs@main/2025-06-29-11-21-41.png)

![图 4](https://cdn.jsdelivr.net/gh/Tdahuyou/imgs@main/2025-06-29-11-23-41.png)

:::

## 5. 🔗 References

- github
  - https://github.com/excalidraw/excalidraw
- deepwiki
  - https://deepwiki.com/excalidraw/excalidraw
- mermaid 官网
  - https://mermaid.js.org/
- 在线网页版
  - https://excalidraw.com/
- vscode 插件
  - https://marketplace.visualstudio.com/items?itemName=pomdtr.excalidraw-editor
