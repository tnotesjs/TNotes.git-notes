# [0015. 查看提交历史](https://github.com/tnotesjs/TNotes.git-notes/tree/main/notes/0015.%20%E6%9F%A5%E7%9C%8B%E6%8F%90%E4%BA%A4%E5%8E%86%E5%8F%B2)

<!-- region:toc -->

- [1. 🎯 本节内容](#1--本节内容)
- [2. 🫧 评价](#2--评价)
- [3. 🤔 如何查看提交日志？](#3--如何查看提交日志)
- [4. 🤔 如何格式化日志输出？](#4--如何格式化日志输出)
- [5. 🤔 如何查看特定范围的提交？](#5--如何查看特定范围的提交)

<!-- endregion:toc -->

## 1. 🎯 本节内容

- 查看日志（`git log`）
- 格式化日志输出（`git log --pretty`、`--graph`）
- 查看特定范围的提交

## 2. 🫧 评价

这篇笔记介绍了 `git log` 命令的一些基础用法。

## 3. 🤔 如何查看提交日志？

使用 `git log` 命令可以查看项目的提交历史：

```bash
# 查看完整的提交日志
git log

# 查看最近 N 次提交
git log -5

# 查看每次提交引入的差异
git log -p

# 查看每次提交的简要统计信息
git log --stat
```

默认情况下，`git log` 会按时间倒序列出所有提交，每条记录包含：提交的 SHA-1 校验和、作者、日期和提交消息。

常用的日志过滤选项：

```bash
# 按作者过滤
git log --author="John"

# 按日期范围过滤
git log --since="2024-01-01" --until="2024-12-31"

# 按提交消息关键字过滤
git log --grep="fix"

# 查看涉及某个文件的提交
git log -- path/to/file.txt
```

## 4. 🤔 如何格式化日志输出？

Git 提供了多种方式来自定义日志的输出格式：

```bash
# 单行显示
git log --oneline

# 显示分支合并图
git log --graph

# 组合使用：单行 + 分支图 + 所有分支
git log --oneline --graph --all

# 使用内置格式
git log --pretty=short
git log --pretty=full
git log --pretty=fuller
```

使用 `--pretty=format` 可以完全自定义输出格式：

```bash
git log --pretty=format:"%h - %an, %ar : %s"
# 1df198f - Tdahuyou, 15 minutes ago : 📝 Update notes - 2026-03-12 10:13:21
# 3d4da12 - Tdahuyou, 23 minutes ago : update
# 7da7418 - Tdahuyou, 36 minutes ago : 📝 Update notes - 2026-03-12 09:52:21
```

常用的格式占位符：

| 占位符 | 说明                        |
| ------ | --------------------------- |
| `%H`   | 完整的提交哈希              |
| `%h`   | 简短的提交哈希              |
| `%an`  | 作者名字                    |
| `%ae`  | 作者邮箱                    |
| `%ar`  | 相对日期（如 "2 days ago"） |
| `%s`   | 提交消息                    |
| `%d`   | 引用名称（分支、标签等）    |

## 5. 🤔 如何查看特定范围的提交？

Git 支持通过多种方式限定要查看的提交范围：

```bash
# 查看两个提交之间的历史
git log commit1..commit2

# 查看分支 B 中有但分支 A 中没有的提交
git log main..feature

# 查看涉及某个函数的修改历史
git log -S "functionName"

# 查看某个文件的完整修改历史（包括重命名）
git log --follow -- file.txt

# 合并提交不展开
git log --no-merges

# 只显示合并提交
git log --merges
```

这些过滤选项可以组合使用。例如，查看作者 `Tdahuyou` 在特定时间段内对当前这篇笔记的修改历史：

```bash
git log --author="Tdahuyou" --since="2026-01-01" -- "notes\0015. 查看提交历史\README.md"
# commit 561ce221e6c0c11158e561526699b90bd59399fd
# Author: Tdahuyou <dahuyoutop@gmail.com>
# Date:   Sat Feb 28 21:48:47 2026 +0800

#     📝 Update notes - 2026-02-28 21:48:46

# commit 4d69a3bf3aa1fa40c303c658591c2867ce771694
# Author: Tdahuyou <dahuyoutop@gmail.com>
# Date:   Sat Feb 28 21:38:00 2026 +0800

#     📝 Update notes - 2026-02-28 21:37:59
```
