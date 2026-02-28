# [0009. git status](https://github.com/tnotesjs/TNotes.git-notes/tree/main/notes/0009.%20git%20status)

<!-- region:toc -->

- [1. 📒 `git status` 命令的作用](#1--git-status-命令的作用)
- [2. 📒 `git status` 的基本用法](#2--git-status-的基本用法)
  - [2.1. 查看仓库状态](#21-查看仓库状态)
  - [2.2. `git status -s`（简洁模式）](#22-git-status--s简洁模式)
  - [2.3. `git status --short`](#23-git-status---short)
- [3. 📒 文件状态说明](#3--文件状态说明)
- [4. 📒 `git status` 的适用场景](#4--git-status-的适用场景)
- [5. 📒 `git status` 命令列表](#5--git-status-命令列表)

<!-- endregion:toc -->

- `git status` 用于检查当前仓库的状态，确保提交前的变更正确！

## 1. 📒 `git status` 命令的作用

`git status` 命令用于显示当前 Git 仓库的状态，包括：

- 工作目录中的文件是否有 修改
- 是否有 新文件 未被 Git 跟踪
- 是否有 文件已暂存但未提交
- 当前分支信息（如是否有未推送的提交）

## 2. 📒 `git status` 的基本用法

### 2.1. 查看仓库状态

```sh
git status
```

示例输出：

```sh
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
    modified:   file1.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)
    newfile.txt

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
    new file:   staged_file.txt
```

解释：

- `On branch main`：当前位于 `main` 分支
- `Your branch is up to date with 'origin/main'`：本地分支与远程 `origin/main` 同步
- `Changes not staged for commit`：文件已修改但未暂存
- `Untracked files`：新文件未被 Git 跟踪
- `Changes to be committed`：文件已 `git add` 暂存，但还未 `git commit`

### 2.2. `git status -s`（简洁模式）

```sh
git status -s
```

示例输出：

```sh
 M file1.txt
?? newfile.txt
A  staged_file.txt
```

- ` M`：文件被修改（但未暂存）
- `??`：新文件未被跟踪
- `A `：文件已 `git add`（暂存）

这是 `git status --porcelain` 的缩写，适合脚本解析。

### 2.3. `git status --short`

`git status --short` 和 `git status -s` 是相同的，提供简洁输出。

## 3. 📒 文件状态说明

| 状态 | 说明                 |
| ---- | -------------------- |
| ` M` | 文件修改（未暂存）   |
| `M ` | 文件修改（已暂存）   |
| ` D` | 文件删除（未暂存）   |
| `D ` | 文件删除（已暂存）   |
| `??` | 未跟踪的新文件       |
| `A ` | 新添加到暂存区的文件 |
| `R ` | 文件被重命名         |
| `UU` | 发生合并冲突         |

## 4. 📒 `git status` 的适用场景

- 确保提交前没有遗漏的更改
- 在 Git 操作前检查当前仓库状态
- 结合 `git add` 和 `git commit` 进行版本控制

## 5. 📒 `git status` 命令列表

| 命令                         | 作用                       |
| ---------------------------- | -------------------------- |
| `git status`                 | 显示详细的仓库状态         |
| `git status -s` 或 `--short` | 简洁模式，适合快速查看     |
| `git status --porcelain`     | 机器可读格式，适合脚本解析 |
