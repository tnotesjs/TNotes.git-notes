# [0018. 分支合并](https://github.com/tnotesjs/TNotes.git-notes/tree/main/notes/0018.%20%E5%88%86%E6%94%AF%E5%90%88%E5%B9%B6)

<!-- region:toc -->

- [1. 🎯 本节内容](#1--本节内容)
- [2. 🫧 评价](#2--评价)
- [3. 🤔 什么是快进合并？](#3--什么是快进合并)
- [4. 🤔 什么是三方合并与合并提交？](#4--什么是三方合并与合并提交)
- [5. 🤔 如何使用 git merge 命令？](#5--如何使用-git-merge-命令)

<!-- endregion:toc -->

## 1. 🎯 本节内容

- 快进合并（Fast-forward）
- 三方合并与合并提交（Merge Commit）
- 合并命令（`git merge`）

## 2. 🫧 评价

- todo

## 3. 🤔 什么是快进合并？

快进合并（Fast-forward）是最简单的合并方式。当目标分支是当前分支的直接上游（即当前分支的提交历史是目标分支的子集）时，Git 只需将当前分支指针向前移动即可，不需要创建额外的合并提交。

```bash
# 假设 feature 分支比 main 分支领先若干次提交
git switch main
git merge feature
# 输出: Fast-forward
```

合并前：

```
main -> C2
         \
          C3 -> C4 (feature)
```

快进合并后：

```
main, feature -> C4
```

如果想要在快进合并时也生成一个合并提交（保留分支历史），可以使用 `--no-ff` 参数：

```bash
git merge --no-ff feature
```

## 4. 🤔 什么是三方合并与合并提交？

当两个分支各自有了新的提交，形成了分叉，Git 就无法通过快进合并来完成。此时 Git 会执行三方合并（three-way merge），使用两个分支的最新提交以及它们的共同祖先来创建一个新的**合并提交（Merge Commit）**。

```
          C3 -> C5 (main)
         /
C1 -> C2
         \
          C4 -> C6 (feature)
```

合并后：

```
          C3 -> C5 ---\
         /             \
C1 -> C2                C7 (main, merge commit)
         \             /
          C4 -> C6 ---/
```

合并提交有两个父提交，记录了两条分支的汇合点。

## 5. 🤔 如何使用 git merge 命令？

```bash
# 将 feature 分支合并到当前分支
git switch main
git merge feature

# 合并时禁止快进（始终创建合并提交）
git merge --no-ff feature

# 合并时附带提交消息
git merge feature -m "合并 feature 分支到 main"

# 只检查能否合并，不实际执行
git merge --no-commit feature
git merge --abort  # 取消合并

# 使用 squash 合并（将分支的多次提交压缩为一次）
git merge --squash feature
git commit -m "合并 feature 功能"
```

`--squash` 合并不会创建合并提交，而是将目标分支的所有更改合并到暂存区，需要手动提交。这种方式可以保持主分支历史的整洁。

合并操作的最佳实践：

- 合并前确保工作区干净（无未提交的修改）
- 合并前先拉取远程最新代码
- 重要分支的合并建议使用 `--no-ff` 以保留分支历史
