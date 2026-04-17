# [0008. 贮藏（Stashing）](https://github.com/tnotesjs/TNotes.git-notes/tree/main/notes/0008.%20%E8%B4%AE%E8%97%8F%EF%BC%88Stashing%EF%BC%89)

<!-- region:toc -->

- [1. 🎯 本节内容](#1--本节内容)
- [2. 🫧 评价](#2--评价)
- [3. 🤔 如何使用 git stash 贮藏工作？](#3--如何使用-git-stash-贮藏工作)
- [4. 📒 `git stash` 命令的作用](#4--git-stash-命令的作用)
- [5. 📒 `git stash` 命令列表](#5--git-stash-命令列表)
- [6. 📒 `git stash` 命令的基本使用](#6--git-stash-命令的基本使用)
  - [6.1. 暂存当前工作目录](#61-暂存当前工作目录)
  - [6.2. 查看暂存列表](#62-查看暂存列表)
  - [6.3. 恢复暂存的修改](#63-恢复暂存的修改)
    - [恢复但不删除](#恢复但不删除)
    - [恢复并删除 stash](#恢复并删除-stash)
  - [6.4. 删除 stash](#64-删除-stash)
    - [删除指定 stash](#删除指定-stash)
    - [删除所有 stash](#删除所有-stash)
  - [6.5. 仅 stash 某些文件](#65-仅-stash-某些文件)
- [7. 📒 `git stash` 的适用场景](#7--git-stash-的适用场景)

<!-- endregion:toc -->

## 1. 🎯 本节内容

- 贮藏（Stashing）：`git stash`

## 2. 🫧 评价

`git stash` 是一个非常实用的命令，适用于需要临时存储更改的场景！

<!--
git stash 是什么？它是将内容存储到本地吗？是否可以同步到远程？

## Git Stash 简介

### 它是什么？

`git stash` 的作用是**临时保存你的工作区修改**，让你的工作目录回到干净的状态（即最近一次 commit 的样子）。

典型使用场景：你正在一个分支上开发，突然需要切到另一个分支修 bug，但当前改动还没完成、不想提交一个半成品 commit。这时就可以用 stash 暂存起来。

### 常用命令

```bash
git stash                  # 暂存当前修改
git stash save "描述信息"  # 带备注的暂存（推荐）
git stash list             # 查看所有 stash 记录
git stash pop              # 恢复最近一次 stash 并删除它
git stash apply            # 恢复最近一次 stash 但保留记录
git stash drop             # 删除某条 stash 记录
git stash clear            # 清空所有 stash
```

---

### 是存储在本地吗？

**是的，stash 只存在于本地仓库中。**

它本质上是存储在 `.git/objects` 目录里的一个特殊对象，不会随任何常规 Git 操作推送到远程。

---

### 能同步到远程吗？

**默认不能，但有变通方法：**

`git stash` 本身没有 `push` 到远程的机制，但你可以用以下方式间接实现：

```bash
# 方法：把 stash 转成一个普通分支或 tag，然后推送
git stash branch temp-branch   # 从 stash 创建一个分支
git push origin temp-branch    # 推送到远程
```

这样别人就能拉到你的临时改动了。

---

### 总结

| 问题 | 答案 |
|---|---|
| stash 是什么 | 临时保存未提交的修改 |
| 存在哪里 | **本地仓库**（`.git` 目录内） |
| 能否推送到远程 | 不能直接推送，需先转成分支再 push |
| 适合长期保存吗 | 不适合，适合短期暂存（几小时~几天） |

如果需要长期保存或跨设备同步，建议直接建一个临时分支 commit 进去，比 stash 更可靠。

-------------

git stash push -m "xxx"
git stash save "xxx"
这两者的区别是？

-----------

## `git stash push -m` vs `git stash save`

### 简短回答

**功能上基本相同**，都是带描述信息地暂存当前修改。但它们是不同时代的产物：

| | `git stash save "xxx"` | `git stash push -m "xxx"` |
|---|---|---|
| 引入版本 | Git 1.6.x（早期） | Git 2.13+（较新） |
| 推荐程度 | **已标记为 deprecated** | ✅ 官方推荐 |
| 语法风格 | 旧式子命令 | 新式子命令风格 |

---

### 关键区别

#### 1. `push` 支持更多选项，`save` 不支持

```bash
# push 可以指定暂存部分文件
git stash push -m "只暂存 src 目录" -- src/

# save 做不到这个
git stash save "xxx" -- src/   # ❌ 这里的 -- 会被忽略，仍然暂存所有文件
```

这是 `push` 最大的优势——**可以选择性地 stash 部分文件**。

#### 2. `save` 在新版 Git 中会提示弃用

```bash
$ git stash save "test"
warning: push.default is unset; ...
```

不会报错，但会有 deprecation 提示。

#### 3. `push` 还支持 `--staged`（Git 2.35+）

```bash
# 只暂存已 add 到暂存区的内容
git stash push --staged -m "只暂存 staged 的部分"
```

`save` 完全没有这个能力。

---

### 总结

> **用 `git stash push -m "xxx"` 替代 `git stash save "xxx"`**。
>
> 功能相同，但 `push` 更灵活（支持部分文件、staged 区域等选项），且是官方推荐的新写法。`save` 本质上已被废弃。
 -->

## 3. 🤔 如何使用 git stash 贮藏工作？

当你正在开发某个功能，但需要临时切换到其他分支处理紧急事务时，可以使用 `git stash` 将当前的工作进度贮藏起来：

```bash
# 贮藏当前工作（已跟踪文件的修改和暂存的内容）
git stash

# 贮藏时附带消息
git stash push -m "正在开发用户模块"

# 贮藏时包含未跟踪的文件
git stash -u

# 贮藏时包含所有文件（包括被忽略的）
git stash -a
```

管理贮藏的常用命令：

```bash
# 查看贮藏列表
git stash list

# 恢复最近的贮藏（保留贮藏记录）
git stash apply

# 恢复最近的贮藏并删除记录
git stash pop

# 恢复指定的贮藏
git stash apply stash@{2}

# 删除指定的贮藏
git stash drop stash@{0}

# 清空所有贮藏
git stash clear

# 从贮藏创建新分支
git stash branch new-branch
```

## 4. 📒 `git stash` 命令的作用

- `git stash` 用于暂存（存储）当前的工作目录修改，使其回到干净状态，方便切换分支或执行其他操作。之后可以随时恢复这些修改。
- 基本功能
  - 保存未提交的修改（但不影响暂存区的内容）
  - 工作区恢复到干净状态（就像刚 `git checkout` 一样）
  - 可以多次 `stash` 并管理多个存储
  - 支持部分文件暂存
  - 可以应用或丢弃暂存的更改

## 5. 📒 `git stash` 命令列表

| 命令                       | 作用                 |
| -------------------------- | -------------------- |
| `git stash`                | 暂存所有未提交的修改 |
| `git stash list`           | 查看所有 stash       |
| `git stash apply`          | 恢复 stash，但不删除 |
| `git stash pop`            | 恢复 stash，并删除   |
| `git stash drop stash@{n}` | 删除指定 stash       |
| `git stash clear`          | 清除所有 stash       |

## 6. 📒 `git stash` 命令的基本使用

### 6.1. 暂存当前工作目录

```sh
git stash
```

- 作用：
  - 保存：未提交的修改（但不包括 `git add` 过的部分）
  - 恢复：工作区回到上次提交的状态（文件修改消失，但没有丢失）

---

### 6.2. 查看暂存列表

```sh
git stash list
```

示例输出：

```sh
stash@{0}: WIP on main: 4a5b6c7 修复 bug
stash@{1}: WIP on main: 1a2b3c4 添加新功能
```

每个 `stash@{n}` 代表一次 `stash` 记录，编号 `0` 是最近的存储。

### 6.3. 恢复暂存的修改

#### 恢复但不删除

```sh
git stash apply stash@{0}
```

- 只恢复 `stash@{0}` 里的内容，但不会删除 stash 记录。

#### 恢复并删除 stash

```sh
git stash pop
```

- 恢复最新的 `stash`，并删除它。

### 6.4. 删除 stash

#### 删除指定 stash

```sh
git stash drop stash@{0}
```

- 仅删除 `stash@{0}` 这一项。

#### 删除所有 stash

```sh
git stash clear
```

- 清空所有 stash 记录，无法恢复！

### 6.5. 仅 stash 某些文件

如果你只想 stash 部分文件，可以使用：

```sh
git stash push -m "仅暂存 main.js" -- main.js
```

然后恢复时：

```sh
git stash apply stash@{0}
```

这样可以只存特定文件的修改。

## 7. 📒 `git stash` 的适用场景

1. 切换分支时不想提交但又不想丢失修改
   ```sh
   git stash
   git checkout 其他分支
   ```
2. 修复紧急 bug
   ```sh
   git stash
   git checkout bug-fix-branch
   ```
   修复完后：
   ```sh
   git checkout main
   git stash pop  # 恢复原来的代码
   ```
3. 避免临时提交
   - 只想保存工作，不想提交的情况下，stash 是最好的选择。
