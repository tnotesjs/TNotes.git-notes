# [0016. 撤消操作](https://github.com/tnotesjs/TNotes.git-notes/tree/main/notes/0016.%20%E6%92%A4%E6%B6%88%E6%93%8D%E4%BD%9C)

<!-- region:toc -->

- [1. 🎯 本节内容](#1--本节内容)
- [2. 🫧 评价](#2--评价)
- [3. 🤔 如何补充提交？](#3--如何补充提交)
- [4. 🤔 如何取消暂存的文件？](#4--如何取消暂存的文件)
- [5. 🤔 如何撤消对文件的修改？](#5--如何撤消对文件的修改)
- [6. 🤔 如何使用 git revert 进行安全撤销？](#6--如何使用-git-revert-进行安全撤销)

<!-- endregion:toc -->

## 1. 🎯 本节内容

- 补充提交（`git commit --amend`）
- 取消暂存的文件（`git restore --staged` / `git reset HEAD`）
- 撤消对文件的修改（`git restore` / `git checkout --`）
- 使用 `git revert` 进行安全撤销

## 2. 🫧 评价

- todo

## 3. 🤔 如何补充提交？

如果提交后发现漏掉了文件或提交消息写错了，可以使用 `--amend` 参数来修补上一次提交：

```bash
# 修改提交消息
git commit --amend -m "修正后的提交消息"

# 补充遗漏的文件到上一次提交
git add forgotten_file.txt
git commit --amend --no-edit
```

`--amend` 实际上会用一个新的提交替换掉上一个提交。如果暂存区有内容，这些内容会被包含在新提交中。`--no-edit` 参数表示不修改提交消息。

注意：不要对已经推送到远程仓库的提交使用 `--amend`，因为这会改变提交的 SHA-1 值，导致历史不一致。

## 4. 🤔 如何取消暂存的文件？

如果不小心将不需要的文件添加到了暂存区，可以取消暂存：

```bash
# 推荐方式（Git 2.23+）
git restore --staged file.txt

# 传统方式
git reset HEAD file.txt

# 取消所有暂存
git restore --staged .
```

这个操作只是将文件从暂存区移除，不会影响工作区中的文件内容。

## 5. 🤔 如何撤消对文件的修改？

如果想要丢弃工作区中对某个文件的修改，恢复到上次提交的状态：

```bash
# 推荐方式（Git 2.23+）
git restore file.txt

# 传统方式
git checkout -- file.txt

# 恢复所有文件
git restore .
```

**注意**：这个操作是不可逆的。被丢弃的修改无法找回，因为这些修改从未被提交过。在执行之前，请确保你真的不需要这些修改。

如果只想恢复到某个特定提交的状态：

```bash
git restore --source=HEAD~2 file.txt
```

## 6. 🤔 如何使用 git revert 进行安全撤销？

`git revert` 通过创建一个新的提交来撤销指定提交的更改，不会修改已有的提交历史，是一种安全的撤销方式：

```bash
# 撤销最近的一次提交
git revert HEAD

# 撤销指定的提交
git revert abc1234

# 撤销多个连续的提交
git revert HEAD~3..HEAD

# 只修改工作区和暂存区，不自动创建提交
git revert --no-commit abc1234
```

`git revert` 与 `git reset` 的区别：

| 特性   | `git revert`     | `git reset`      |
| ------ | ---------------- | ---------------- |
| 原理   | 创建新提交来撤销 | 移动 HEAD 指针   |
| 历史   | 保留完整历史     | 可能丢弃历史     |
| 安全性 | 适合已推送的提交 | 适合未推送的提交 |
| 协作   | 不影响其他人     | 可能影响其他人   |

对于已推送到远程的提交，应该使用 `git revert` 来撤销，避免对团队协作造成影响。
