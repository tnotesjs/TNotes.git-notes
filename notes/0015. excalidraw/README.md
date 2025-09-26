# [0015. excalidraw](https://github.com/tnotesjs/TNotes.git-notes/tree/main/notes/0015.%20excalidraw)

<!-- region:toc -->

- [1. 🫧 评价](#1--评价)
- [2. 📒 个人最喜欢的几个核心亮点](#2--个人最喜欢的几个核心亮点)
- [3. 💻 demos.1 - share](#3--demos1---share)
- [4. 💻 demos.2 - mermaid](#4--demos2---mermaid)
- [5. 🤔 为什么叫 excalidraw 这个名字呢？](#5--为什么叫-excalidraw-这个名字呢)
- [6. 🔗 引用](#6--引用)

<!-- endregion:toc -->

## 1. 🫧 评价

- excalidraw 是一个开源的绘图工具。
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
  - ![图 1](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-06-29-11-10-49.png)

## 3. 💻 demos.1 - share

::: swiper

![电脑和 iPad 共享画布](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-06-29-11-06-56.png)

:::

- 比如上图中的 mark 是在 iPad 上通过 Apple pencil 写的字，红色的是在 PC 端任意位置双击鼠标键入的文本内容，矩形和箭头元素，是画板自带的一些组件。

## 4. 💻 demos.2 - mermaid

- 支持解析 mermaid
- 支持修改字体
- 支持对解析后的内容进行二次编辑
  - 注意，并非所有 mermaid 图表都能导入后再二次编辑，有些图表导入后就是一张完整的图片，无法在对其中的内容进行二次编辑。

::: swiper

![输入 mermaid](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-06-29-11-19-17.png)

![导入](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-06-29-11-21-41.png)

![二次编辑](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-06-29-11-23-41.png)

:::

## 5. 🤔 为什么叫 excalidraw 这个名字呢？

- **Excalidraw** 这个名字的由来很有趣，它结合了 **"Excalibur"（亚瑟王的神剑）** 和 **"Draw"（绘画）** 两个词，寓意是希望这个工具能像传说中的神剑一样强大、优雅，成为用户绘制草图时的“利器”。
- **Excalibur（誓约胜利之剑）**：
  - 在亚瑟王传说中，Excalibur 是一把象征力量与权威的神剑，只有真正的王者才能拔出它。
  - 开发者借用这个名字，可能是想表达 Excalidraw 是一个“强大而优雅”的工具，能帮助用户轻松“斩断”复杂的设计问题。
- **Draw（绘画）**：
  - 直接表明这是一个绘图工具，功能简单明了。
- 其他可能的联想：
  - **"Excali-"** 发音类似 **"excellent"（优秀的）**，暗示这是一个出色的绘图工具。
  - 名字独特且易记，符合开源项目的命名风格（比如 **Inkscape、GIMP** 等）。
- 官方态度：
  - 虽然官方没有正式解释名字的起源，但社区普遍认可这个命名逻辑。开发者之一曾在访谈中提到，名字的灵感确实来自 **Excalibur**，希望工具能像神剑一样帮助用户“披荆斩棘”地创作。

## 6. 🔗 引用

- [excalidraw github][1]
- [excalidraw deepwiki][2]
- [mermaid 官网][3]
- [mermaid 在线网页版][4]
- [vscode 中的 mermaid 插件][5]

[1]: https://github.com/excalidraw/excalidraw
[2]: https://deepwiki.com/excalidraw/excalidraw
[3]: https://mermaid.js.org/
[4]: https://excalidraw.com/
[5]: https://marketplace.visualstudio.com/items?itemName=pomdtr.excalidraw-editor
