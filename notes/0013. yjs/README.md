# [0013. yjs](https://github.com/Tdahuyou/TNotes.git-notes/tree/main/notes/0013.%20yjs)

<!-- region:toc -->

- [1. 📝 概述](#1--概述)
- [2. 🤔 为什么叫 YJS，Y 是什么意思？](#2--为什么叫-yjsy-是什么意思)
- [3. 🔗 References](#3--references)

<!-- endregion:toc -->

## 1. 📝 概述

- yjs
  - yjs 是一个用于构建协同编辑应用的 JavaScript 库，支持 **实时多人协作功能**。
  - yjs 基于 CRDT（Conflict-Free Replicated Data Type）技术实现，确保在分布式环境中数据的一致性和同步性。
- 核心特性：
  - **CRDT 支持**：通过内置的 CRDT 实现，确保多用户并发操作时的数据一致性。
  - **实时协作**：适用于需要多人同时编辑的场景，例如在线文档、协作文档编辑器等。
  - **模块化设计**：提供灵活的 API 和插件系统，方便集成到各种前端框架中。
  - **跨平台**：支持浏览器和 Node.js 环境。
- 使用场景：
  - 在线文档协作工具（如 Google Docs 类似功能）
  - 实时聊天与协同白板
  - 多人游戏或协同开发工具

## 2. 🤔 为什么叫 YJS，Y 是什么意思？

- 👇 from Google search ai summary

> - The "Y" in Yjs could be interpreted in several ways, potentially relating to:
> - Yjs 中的 "Y "可以有多种解释，可能与以下方面有关：
>   - Yielding: The idea of yielding changes from different users to be merged, possibly with a focus on "yes" to the merging operation.
>   - 让渡：让渡来自不同用户的变更以进行合并的想法，可能侧重于合并操作的 "是"。
>   - Y-combinator: While not directly a Y-combinator, the concept of recursion and self-reference could be relevant to the way Yjs handles data structures.
>   - Y-combinator：虽然不是直接的 Y-combinator，但递归和自我引用的概念可能与 Yjs 处理数据结构的方式有关。
>   - Merging: The core function of Yjs is to merge changes from different sources, so the "Y" could represent the merging operation.
>   - 合并：Yjs 的核心功能是合并来自不同来源的更改，因此 "Y "可以代表合并操作。

## 3. 🔗 References

- github
  - https://github.com/yjs/yjs
  - **Who is using YJS**
    - 这一部分提到了很多开源的笔记软件。
  - **Yjs CRDT Algorithm**
    - 这一部分对 yjs 的核心算法做了一个简单的介绍说明。
- 官方文档
  - https://docs.yjs.dev/
  - 查阅官方文档，了解如何使用 yjs。
