# [0045. git clean](https://github.com/tnotesjs/TNotes.git-notes/tree/main/notes/0045.%20git%20clean)

<!-- region:toc -->

- [1. 🎯 本节内容](#1--本节内容)
- [2. 🫧 评价](#2--评价)
- [3. 🤔 `git clean` 的作用是什么？](#3--git-clean-的作用是什么)
- [4. 🤔 `git clean` 如何使用？](#4--git-clean-如何使用)
  - [4.1. 基本用法](#41-基本用法)
  - [4.2. 常用选项](#42-常用选项)
  - [4.3. 注意事项](#43-注意事项)
  - [4.4. `git clean` 的典型使用流程](#44-git-clean-的典型使用流程)

<!-- endregion:toc -->

## 1. 🎯 本节内容

- `git clean` 使用说明

## 2. 🫧 评价

`git clean` 主要用于删除工作目录中未被跟踪的文件。

`git clean` 是一个危险操作，因为被删除的文件无法恢复。建议先使用 `-n`（干跑模式）预览将要删除的文件，确认无误后再执行。

## 3. 🤔 `git clean` 的作用是什么？

`git clean` 用于删除工作目录中未被 Git 追踪的文件和目录（untracked files）。

简单说：`git stash` 是“藏起来”，`git clean` 是“扔掉”。

## 4. 🤔 `git clean` 如何使用？

### 4.1. 基本用法

```bash
git clean [选项]
```

### 4.2. 常用选项

| 选项                  | 作用                                 |
| --------------------- | ------------------------------------ |
| `-n` / `--dry-run`    | 预览会删除哪些文件，但不实际删除     |
| `-f` / `--force`      | 强制执行删除（必须加，否则默认拒绝） |
| `-d`                  | 同时删除未追踪的目录                 |
| `-x`                  | 也删除被 `.gitignore` 忽略的文件     |
| `-X`                  | 只删除被 `.gitignore` 忽略的文件     |
| `-i`                  | 交互式删除，逐个确认                 |
| `--exclude <pattern>` | 排除匹配该模式的文件                 |

### 4.3. 注意事项

- `git clean` 不可撤销
  - 删除的文件不进回收站，不在 Git 历史中，无法恢复
  - 用之前务必 `git clean -n` 预览
- 如果要利用 `git clean` 删除文件，必须加 `-f`
  - Git 默认拒绝执行 `git clean`，因为操作不可逆
  - 这是一种安全保护
- 不会删除已追踪的文件
  - 已 commit 的文件，无法使用 `git clean` 删除
  - 已 staged 的文件，可以利用 `git restore --staged <file>...` 将其从暂存区中移除，再利用 git clean 删除
- 被 `.gitignore` 忽略的文件
  - 默认情况下 `git clean` 不会删除被 `.gitignore` 忽略的文件，除非显式加 `-x`

### 4.4. `git clean` 的典型使用流程

```bash
# 1. 先预览（永远先跑这个）
git clean -n

# 输出示例：
# git clean -n
# Would remove debug.log
# Would remove config.local.json

# 2. 删除未追踪的文件（如果只需要删除未被跟踪的文件，跑这个）
git clean -f

# 3. 删除未追踪的文件和目录（如果需要同时删除未被跟踪的文件和目录，跑这个）
git clean -fd

# 4. 支持交互式操作（如果需要逐个确认是否需要删除，跑这个）
git clean -i

# 输出示例：
# git clean -i
# Would remove the following items:
#   1.js  2.js
# *** Commands ***
#     1: clean
#     2: filter by pattern
#     3: select by numbers
#     4: ask each
#     5: quit
#     6: help
# What now> 6
# clean               - start cleaning
# filter by pattern   - exclude items from deletion
# select by numbers   - select items to be deleted by numbers
# ask each            - confirm each deletion (like "rm -i")
# quit                - stop cleaning
# help                - this screen
# ?                   - help for prompt selection
```

`git clean -i` 交互菜单说明：

| 选项 | 命令 | 作用 |
| --- | --- | --- |
| `1` / `c` | `clean` | 直接删除所有列出的文件（全删） |
| `2` / `f` | `filter by pattern` | 排除匹配某个模式的文件（反选，回车表示确认退出） |
| `3` / `s` | `select by numbers` | 按编号选择要删除的文件（正选，回车表示确认退出） |
| `4` / `a` | `ask each` | 逐个确认，每个文件都问你删不删（y 表示删、N 或直接回车表示不删） |
| `5` / `q` | `quit` | 退出，什么都不删 |
| `6` / `h` | `help` | 显示帮助信息 |

```bash
# 1. clean - 全部删除
# What now> 1
# 直接删除所有列出的未追踪文件，不问第二次。

# 2. filter by pattern — 排除不想删的
# What now> 2
# Input ignore patterns>> *.js    # 排除所有 .js 文件
# 输入模式后，匹配的文件会从待删除列表中移除，剩下的才被删除。

# 3. select by numbers — 勾选要删的
# What now> 3
#     1: 1.js    2: 2.js
# Select items to delete>> 1       # 输入编号选中
#   * 1: 1.js    2: 2.js
# Select items to delete>>         # 直接回车确认
# 只有带 * 标记的会被删除。

# 4. ask each — 逐个确认
# What now> 4
# Remove 1.js [y/N]? y
# Remove 2.js [y/N]? n

# 5. quit — 放弃
# What now> 5
# 什么也不做，直接退出。
```

::: tip 命令交互的补充说明：

- 选择跑哪个命令的时候，可以输入编号（比如 `6`）确认，也可以输入命令的首字母（比如 `h`）来确认。

如何决策：

- 全删 -> 输入 `1`
- 只删某几个 -> 输入 `3`，选编号，回车确认
- 除了某几个都删 -> 输入 `2`，输入排除模式
- 一个个确认 -> 输入 `4`
- 算了不删了 -> 输入 `5`

:::

使用 `git clean` 清理所有未追踪文件和目录：

```bash
# 清理所有文件（包括被 .gitignore 忽略的）
git clean -fdx
# 这是“最彻底的清理”，会删掉所有未追踪文件和目录

# 清理所有文件（只包括被 .gitignore 忽略的）
git clean -fdX

# 注意事项：
# 如果文件是被 .gitignore 忽略的
# 那么在执行 git clean -n 时并不会输出它们
# 因此不要误以为 git clean -n 没有打印的文件就不会被删除
```

在执行删除操作时，可通过 `--exclude` 支持排除特定文件：

```bash
#
git clean -fd --exclude="*.env"
# 删除所有未追踪文件和目录，但保留 .env 文件。
```
