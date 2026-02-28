# [0017. Git 分支机制](https://github.com/tnotesjs/TNotes.git-notes/tree/main/notes/0017.%20Git%20%E5%88%86%E6%94%AF%E6%9C%BA%E5%88%B6)

<!-- region:toc -->

- [1. 🎯 本节内容](#1--本节内容)
- [2. 🫧 评价](#2--评价)
- [3. 🤔 什么是分支？](#3--什么是分支)
- [4. 🤔 如何创建分支？](#4--如何创建分支)
- [5. 🤔 如何切换分支？](#5--如何切换分支)
- [6. 🤔 HEAD 指针与当前分支是什么关系？](#6--head-指针与当前分支是什么关系)
- [7. 🤔 如何查看分支列表？](#7--如何查看分支列表)

<!-- endregion:toc -->

## 1. 🎯 本节内容

- 什么是分支（本质上是指向提交的可变指针）
- 创建分支（`git branch`）
- 切换分支（`git switch` / `git checkout`）
- HEAD 指针与当前分支
- 查看分支列表（`git branch -v`）

## 2. 🫧 评价

- todo

## 3. 🤔 什么是分支？

在 Git 中，分支本质上是一个**指向某个提交对象的可变指针**。每次提交时，当前分支的指针会自动向前移动，指向最新的提交。

Git 的默认分支名是 `main`（或 `master`）。分支在 Git 内部的实现非常轻量——它只是一个包含 40 个字符 SHA-1 校验和的文件，指向某个提交对象。因此，创建和销毁分支的成本极低。

与其他版本控制系统相比，Git 鼓励频繁使用分支，因为分支操作几乎是即时的。

## 4. 🤔 如何创建分支？

使用 `git branch` 命令创建新分支：

```bash
# 创建一个名为 feature 的新分支（不切换）
git branch feature

# 创建新分支并立即切换（推荐）
git switch -c feature
# 或
git checkout -b feature

# 从指定提交创建分支
git branch feature abc1234

# 从远程分支创建本地分支
git switch -c feature origin/feature
```

创建分支时，新分支会指向当前所在的提交。创建分支并不会自动切换到新分支，除非使用 `git switch -c` 或 `git checkout -b`。

## 5. 🤔 如何切换分支？

```bash
# 推荐方式（Git 2.23+）
git switch feature

# 传统方式
git checkout feature

# 切换到上一个分支
git switch -
```

切换分支时，Git 会将工作区的文件更新为目标分支最新提交的快照。如果工作区有未提交的修改，且与目标分支存在冲突，Git 会阻止切换并提示你先提交或贮藏修改。

`git switch` 是 Git 2.23 引入的新命令，专门用于切换分支，比 `git checkout` 更加语义清晰。

## 6. 🤔 HEAD 指针与当前分支是什么关系？

`HEAD` 是一个特殊的指针，指向当前所在的本地分支。可以把 `HEAD` 理解为"当前分支的别名"。

```
HEAD -> main -> commit C3
              feature -> commit C2
```

当你切换分支时，`HEAD` 会指向新的分支：

```bash
git switch feature
# HEAD 现在指向 feature 分支
```

当你在当前分支提交时，`HEAD` 指向的分支会自动向前移动。

可以通过以下命令查看 `HEAD` 指向：

```bash
cat .git/HEAD
# 输出: ref: refs/heads/main
```

在分离 HEAD 状态下（detached HEAD），`HEAD` 直接指向某个提交而不是分支，这种情况通常发生在检出特定提交或标签时。

## 7. 🤔 如何查看分支列表？

```bash
# 查看本地分支列表（当前分支前有 * 标记）
git branch

# 查看本地分支及最后一次提交信息
git branch -v

# 查看所有分支（包括远程分支）
git branch -a

# 只查看远程分支
git branch -r

# 查看已合并到当前分支的分支
git branch --merged

# 查看未合并到当前分支的分支
git branch --no-merged
```

`--merged` 和 `--no-merged` 在清理过时分支时特别有用。已合并的分支通常可以安全删除。
