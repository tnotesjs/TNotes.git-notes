# [0008. git stash](https://github.com/tnotesjs/TNotes.git-notes/tree/main/notes/0008.%20git%20stash)

<!-- region:toc -->

- [1. 📒 `git stash` 命令的作用](#1--git-stash-命令的作用)
- [2. 📒 `git stash` 命令列表](#2--git-stash-命令列表)
- [3. 📒 `git stash` 命令的基本使用](#3--git-stash-命令的基本使用)
  - [3.1. 暂存当前工作目录](#31-暂存当前工作目录)
  - [3.2. 查看暂存列表](#32-查看暂存列表)
  - [3.3. 恢复暂存的修改](#33-恢复暂存的修改)
    - [恢复但不删除](#恢复但不删除)
    - [恢复并删除 stash](#恢复并删除-stash)
  - [3.4. 删除 stash](#34-删除-stash)
    - [删除指定 stash](#删除指定-stash)
    - [删除所有 stash](#删除所有-stash)
  - [3.5. 仅 stash 某些文件](#35-仅-stash-某些文件)
- [4. 📒 `git stash` 的适用场景](#4--git-stash-的适用场景)

<!-- endregion:toc -->

- `git stash` 是一个非常实用的命令，适用于需要临时存储更改的场景！

## 1. 📒 `git stash` 命令的作用

- `git stash` 用于暂存（存储）当前的工作目录修改，使其回到干净状态，方便切换分支或执行其他操作。之后可以随时恢复这些修改。
- 基本功能
  - 保存未提交的修改（但不影响暂存区的内容）
  - 工作区恢复到干净状态（就像刚 `git checkout` 一样）
  - 可以多次 `stash` 并管理多个存储
  - 支持部分文件暂存
  - 可以应用或丢弃暂存的更改

## 2. 📒 `git stash` 命令列表

| 命令                       | 作用                 |
| -------------------------- | -------------------- |
| `git stash`                | 暂存所有未提交的修改 |
| `git stash list`           | 查看所有 stash       |
| `git stash apply`          | 恢复 stash，但不删除 |
| `git stash pop`            | 恢复 stash，并删除   |
| `git stash drop stash@{n}` | 删除指定 stash       |
| `git stash clear`          | 清除所有 stash       |

## 3. 📒 `git stash` 命令的基本使用

### 3.1. 暂存当前工作目录

```sh
git stash
```

- 作用：
  - 保存：未提交的修改（但不包括 `git add` 过的部分）
  - 恢复：工作区回到上次提交的状态（文件修改消失，但没有丢失）

---

### 3.2. 查看暂存列表

```sh
git stash list
```

示例输出：

```sh
stash@{0}: WIP on main: 4a5b6c7 修复 bug
stash@{1}: WIP on main: 1a2b3c4 添加新功能
```

每个 `stash@{n}` 代表一次 `stash` 记录，编号 `0` 是最近的存储。

### 3.3. 恢复暂存的修改

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

### 3.4. 删除 stash

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

### 3.5. 仅 stash 某些文件

如果你只想 stash 部分文件，可以使用：

```sh
git stash push -m "仅暂存 main.js" -- main.js
```

然后恢复时：

```sh
git stash apply stash@{0}
```

这样可以只存特定文件的修改。

## 4. 📒 `git stash` 的适用场景

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
