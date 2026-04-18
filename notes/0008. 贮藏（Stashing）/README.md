# [0008. 贮藏（Stashing）](https://github.com/tnotesjs/TNotes.git-notes/tree/main/notes/0008.%20%E8%B4%AE%E8%97%8F%EF%BC%88Stashing%EF%BC%89)

<!-- region:toc -->

- [1. 🎯 本节内容](#1--本节内容)
- [2. 🫧 评价](#2--评价)
- [3. 🤔 Git 中的 Stash 是什么？](#3--git-中的-stash-是什么)
- [4. 🤔 `git stash` 命令的作用是什么？](#4--git-stash-命令的作用是什么)
- [5. 🤔 和 Stash 相关的 Git 常用命令都有哪些？](#5--和-stash-相关的-git-常用命令都有哪些)
- [6. 🤔 Stash 只存在于本地仓库中吗？](#6--stash-只存在于本地仓库中吗)
- [7. 🤔 Stash 适合长期保存吗？](#7--stash-适合长期保存吗)
- [8. 🤔 Stash 通常会在什么场景下使用？](#8--stash-通常会在什么场景下使用)
- [9. 🤔 每次 `git stash` 都会创建一个新的 `stash@{0}` 吗？](#9--每次-git-stash-都会创建一个新的-stash0-吗)
- [10. 🆚 `git stash push -m` vs `git stash save`](#10--git-stash-push--m-vs-git-stash-save)
  - [10.1. `push` 支持更多选项，`save` 不支持](#101-push-支持更多选项save-不支持)
  - [10.2. `save` 在新版 Git 中会提示弃用](#102-save-在新版-git-中会提示弃用)
  - [10.3. `push` 还支持 `--staged`（Git 2.35+）](#103-push-还支持---stagedgit-235)
- [11. 🤔 Stash 对象和普通 Commit 对象有什么区别？](#11--stash-对象和普通-commit-对象有什么区别)
- [12. 🤔 为什么 Stash 需要记录多个 parent？](#12--为什么-stash-需要记录多个-parent)
  - [12.1. 先理解普通 commit 为什么只有一个 parent](#121-先理解普通-commit-为什么只有一个-parent)
  - [12.2. Stash 为什么需要多个 parent](#122-stash-为什么需要多个-parent)
  - [12.3. pop 的时候怎么用](#123-pop-的时候怎么用)
  - [12.4. 如果只有一个 parent 会怎样？](#124-如果只有一个-parent-会怎样)
  - [12.5. 一句话总结](#125-一句话总结)

<!-- endregion:toc -->

## 1. 🎯 本节内容

- Stash 的本质
- Stash 在 Git 中的表现形式
- Stash 和 Commit 之间的区别
- `git stash` 相关命令的基本用法

## 2. 🫧 评价

`git stash` 是一个非常实用的命令，适用于需要临时存储更改的场景！

需要注意它主要是用来解决临时存储的需求的，如果是需要长期存储，或者需要同步到仓库历史提交中，走 Commit 比走 Stash 更为合适。

## 3. 🤔 Git 中的 Stash 是什么？

stash 本质上就是一个特殊的 commit。

存放位置：

```
.git/
├── refs/stash    <-  stash 的引用（类似分支指针）
└── objects/      <-  stash commit 对象存在这里，和普通 commit 在一起
```

可以创建一个 stash 查看 `refs/stash` 来验证一下：

```bash
# 创建一个 stash
git stash

# 查看 stash 引用指向的 commit
git cat-file -p refs/stash

# 会输出类似以下内容：
# tree 0873c2e174835dff1189b68f3ff8ce63abae0043                 <- 工作目录的快照
# parent e06371639e72eee621a533259617ffcc35b17ef6               <- parent 1: 创建 stash 时的 HEAD
# parent b5e5987b16d67111cce31dd391d5dbba3fdb047a               <- parent 2: 暂存区的状态
# author Tdahuyou <dahuyoutop@gmail.com> 1776405728 +0800       <- author: 作者信息
# committer Tdahuyou <dahuyoutop@gmail.com> 1776405728 +0800    <- committer: 提交者信息

# WIP on main: e063716 fix: github workflows release.yml        <- 提交信息（commit message）

# 注解：
# author Tdahuyou <dahuyoutop@gmail.com> 1776405728 +0800
# committer Tdahuyou <dahuyoutop@gmail.com> 1776405728 +0800
# author：最初编写这些代码的人
# committer：最后提交这些代码的人
# stash 是本地操作，所以两者通常是同一个人、同一时间。

# WIP on main: e063716 fix: github workflows release.yml
# WIP = Work In Progress（工作中）
# 说明"在哪个分支上、基于哪个 commit 创建的 stash"
```

这就是一个标准的 commit 结构，这就意味着我们可以对 stash 做任何对 commit 能做的事：

```bash
# 像查看 commit 一样查看 stash
git show stash@{0}
git log stash@{0}

# 把 stash 当作分支来用
git stash branch new-branch

# cherry-pick 一个 stash
git cherry-pick stash@{1}
```

::: tip 提示：PowerShell 中花括号被转义的经典问题

这是 PowerShell 的经典坑 => `{` 和 `}` 是 PowerShell 的特殊字符，Git 根本收不到完整的参数。

```powershell
git show stash@{0}
# fatal: ambiguous argument 'stash@': unknown revision or path not in the working tree.
# Use '--' to separate paths from revisions, like this:
# 'git <command> [<revision>...] -- [<file>...]'

# 原因分析：
# PowerShell 把 stash@{0} 当成了自己的语法结构，传给 Git 的参数只剩 stash@，所以报错：
# PowerShell 实际传给 Git 的：
# git show stash@          <- 花括号被 PowerShell 吃掉了
# 我们想要的：
# git show stash@{0}       <- 可以通过加引号来完整传递
```

解决方案：加引号

```powershell
# 用双引号包裹
git show "stash@{0}"
git show "stash@{1}"

# 或者用单引号
git show 'stash@{0}'
```

或者直接用简写，省略 `stash@{0}`

```bash
# stash@{0} 可以省略为 stash（默认就是最新的）
git show stash
git stash pop
git stash drop
```

:::

## 4. 🤔 `git stash` 命令的作用是什么？

`git stash` 的作用是临时保存你的工作区修改，让你的工作目录回到干净的状态（即最近一次 commit 的样子）。

`git stash` 的基本功能：

- 保存未提交的修改（但不影响暂存区的内容）
- 工作区恢复到干净状态（就像刚 `git checkout` 一样）
- 可以多次 `stash` 并管理多个存储
- 支持部分文件暂存
- 可以应用或丢弃暂存的更改

## 5. 🤔 和 Stash 相关的 Git 常用命令都有哪些？

```bash
git stash                     # 暂存当前修改（已跟踪文件的修改和暂存的内容）
git stash -u                  # 贮藏时包含未跟踪的文件
git stash -a                  # 贮藏时包含所有文件（包括被忽略的）
git stash push -m "xxx"       # 带备注的暂存（推荐）
# git stash save "描述信息"    # 带备注的暂存（早期版本写法）
git stash list                # 查看所有 stash 记录
git stash pop                 # 恢复最近一次 stash 并删除这条贮藏记录
git stash apply               # 恢复最近一次 stash 但保留这条贮藏记录
git stash apply stash@{2}     # 恢复指定的贮藏
git stash drop                # 删除某条 stash 记录
git stash drop stash@{1}      # 删除指定的贮藏
git stash clear               # 清空所有 stash
git stash branch new-branch   # 从贮藏创建新分支
```

## 6. 🤔 Stash 只存在于本地仓库中吗？

是的，stash 只存在于本地仓库中。

它本质上是存储在 `.git/objects` 目录里的一个特殊对象，不会随任何常规 Git 操作推送到远程。

`git stash` 本身没有 `push` 到远程的机制，但我们可以用以下方式间接实现：

```bash
# 把 stash 转成一个普通分支或 tag，然后推送。

# 示例：
git stash branch temp-branch   # 从 stash 创建一个分支
git push origin temp-branch    # 推送到远程

# 这样别人就能拉到你的临时改动了。
```

## 7. 🤔 Stash 适合长期保存吗？

不适合，适合短期暂存（几小时~几天）。

如果需要长期保存或跨设备同步，建议直接建一个临时分支 commit 进去，比 stash 更可靠。

## 8. 🤔 Stash 通常会在什么场景下使用？

典型使用场景：你正在一个分支上开发，突然需要切到另一个分支修 bug，但当前改动还没完成、不想提交一个半成品 commit。这时就可以用 stash 暂存起来。

```sh
# 切换分支时不想提交但又不想丢失修改：
git stash # 先把这个半成品临时存到 Stash 中
git checkout bug-fix-branch # 切换到其他分支修复 BUG

# 修复完后：
git checkout <之前的工作分支> # 回到之前的工作分支
git stash pop  # 恢复原来的代码
```

什么时候使用 Stash？

总结一句话：想要避免临时提交，又不想丢失当前的工作进度，stash 是最好的选择。

## 9. 🤔 每次 `git stash` 都会创建一个新的 `stash@{0}` 吗？

是的，每次 `git stash` 都会创建一个新的 `stash@{0}`，之前的所有 stash 依次往后挪一位。

示例：

```
初始状态：
  stash@{0}: stash-A
  stash@{1}: stash-B

执行 git stash（创建了 stash-C）：
  stash@{0}: stash-C  ← 新的
  stash@{1}: stash-A  ← 原来的 0 变成 1
  stash@{2}: stash-B  ← 原来的 1 变成 2
```

会影响 `stash@{N}` 编号的相关操作：

| 操作                         | 效果                                |
| ---------------------------- | ----------------------------------- |
| `git stash`                  | 新 stash 压入栈顶，编号全部 `+1`    |
| `git stash pop`              | 弹出栈顶 `stash@{0}`，编号全部 `-1` |
| `git stash drop "stash@{1}"` | 删除指定位置，后面编号依次前移      |

## 10. 🆚 `git stash push -m` vs `git stash save`

功能上基本相同，都是带描述信息地暂存当前修改。但它们是不同时代的产物：

| 对比项   | `git stash save "xxx"` | `git stash push -m "xxx"` |
| -------- | ---------------------- | ------------------------- |
| 引入版本 | Git 1.6.x（早期）      | Git 2.13+（较新）         |
| 推荐程度 | 已标记为 `deprecated`  | ✅ 官方推荐               |
| 语法风格 | 旧式子命令             | 新式子命令风格            |

建议：用 `git stash push -m "xxx"` 替代 `git stash save "xxx"`。

虽然它们功能基本相同，但 `push` 更灵活（支持部分文件、staged 区域等选项），且是官方推荐的新写法。`save` 本质上已被废弃。

### 10.1. `push` 支持更多选项，`save` 不支持

```bash
# push 可以指定暂存部分文件
git stash push -m "只暂存 src 目录" -- src/

# save 做不到这个
git stash save "xxx" -- src/   # ❌ 这里的 -- 会被忽略，仍然暂存所有文件
```

这是 `push` 最大的优势 => 可以选择性地 stash 部分文件。

### 10.2. `save` 在新版 Git 中会提示弃用

```bash
$ git stash save "test"
# warning: push.default is unset; ...
```

不会报错，但会有 deprecation 提示。

### 10.3. `push` 还支持 `--staged`（Git 2.35+）

```bash
# 只暂存已 add 到暂存区的内容
git stash push --staged -m "只暂存 staged 的部分"
```

`save` 完全没有这个能力。

## 11. 🤔 Stash 对象和普通 Commit 对象有什么区别？

普通 commit 通常只有一个 parent，而 stash commit 有多个 parent：

```
stash commit (W)
  ├── parent 1 -> 创建 stash 时的 HEAD 提交
  ├── parent 2 -> 暂存区（index）的状态
  └── parent 3 -> 未跟踪文件（仅 git stash -u/-a 时存在）
```

可以这样理解：

```
普通 commit:  工作目录 -> 暂存区 -> 提交（一份快照）

stash commit: 同时保存了两份（或三份）快照
              ├── 工作目录的状态
              ├── 暂存区的状态
              └── 未跟踪文件的状态（可选）
```

验证：

```bash
# 查看 Stash 对象
git cat-file -p refs/stash
# tree 630cfd0a337c0c4496889adebe1ad040d79310cc
# parent e06371639e72eee621a533259617ffcc35b17ef6
# parent fb055f7310421395c9711366197312ac33c2af94
# author Tdahuyou <dahuyoutop@gmail.com> 1776407219 +0800
# committer Tdahuyou <dahuyoutop@gmail.com> 1776407219 +0800

# WIP on main: e063716 fix: github workflows release.yml



# 查看普通的 Commit 对象
git cat-file -p e063716 # 传入 CommitID 查看特定 Commit 对象文件记录的信息
# tree f9cbf66604ad44048d235dfaee219c652dc2e59c
# parent eeb3527fa34008cba3b5b2bdab793937c599bf78
# author Tdahuyou <dahuyoutop@gmail.com> 1775205716 +0800
# committer Tdahuyou <dahuyoutop@gmail.com> 1775205716 +0800

# fix: github workflows release.yml
```

|                | 普通 commit  | stash commit        |
| -------------- | ------------ | ------------------- |
| tree           | 1 个         | 1 个                |
| parent         | 1 个         | 2~3 个              |
| author         | 1 个         | 1 个                |
| committer      | 1 个         | 1 个                |
| commit message | 有意义的描述 | 自动生成的 WIP 信息 |

结构完全一样，唯一的区别就是 parent 数量不同。

## 12. 🤔 为什么 Stash 需要记录多个 parent？

因为 Stash 需要同时保存多个不同的状态 => 工作目录和暂存区的状态可能不一样。

### 12.1. 先理解普通 commit 为什么只有一个 parent

普通 commit 做的事：工作目录的修改 -> git add -> 暂存区 -> git commit -> 一份快照

它只需要记录一个 tree（暂存区的内容），不需要区分哪些是工作目录的、哪些是暂存区的，因为 commit 之后这两者就统一了。

### 12.2. Stash 为什么需要多个 parent

Stash 的场景不同 => 它要在不提交的情况下保存进度，而工作目录和暂存区的状态可能不一样：

```
假设你有这些修改：

文件 A：已 add 到暂存区（staged）
文件 B：只在工作目录修改了（unstaged）

git stash 时需要同时记住：
  ├── 文件 A 在暂存区里的状态 → parent 2
  └── 文件 B 在工作目录里的状态 → tree（主快照）
```

### 12.3. pop 的时候怎么用

```
git stash pop

Git 读取 stash commit：
  ├── 从 parent 1 恢复 HEAD 状态
  ├── 从 parent 2 恢复暂存区状态
  └── 从 tree 恢复工作目录状态
```

这样 pop 之后，原来 staged 的还是 staged，原来 unstaged 的还是 unstaged，和 stash 之前一模一样。

### 12.4. 如果只有一个 parent 会怎样？

```
假设 stash 只保存一份快照：

  工作目录 + 暂存区 → 合并成一个快照

  pop 的时候：
  ├── 所有修改都恢复到工作目录
  └── 暂存区里的状态丢了

  你之前 git add 的东西，pop 完全没了
```

### 12.5. 一句话总结

Stash 有多个 parent 是因为暂存区和工作目录的状态可能不同，需要分别记录，pop 时才能精确还原两者的区别。
