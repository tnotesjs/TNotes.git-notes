# [0033. Git 内部原理](https://github.com/tnotesjs/TNotes.git-notes/tree/main/notes/0033.%20Git%20%E5%86%85%E9%83%A8%E5%8E%9F%E7%90%86)

<!-- region:toc -->

- [1. 🎯 本节内容](#1--本节内容)
- [2. 🫧 评价](#2--评价)
- [3. 🤔 什么是 Git 引用与符号引用？](#3--什么是-git-引用与符号引用)
  - [3.1. 引用类型](#31-引用类型)
  - [3.2. 符号引用](#32-符号引用)
- [4. 🤔 什么是包文件与垃圾回收？](#4--什么是包文件与垃圾回收)
  - [4.1. 松散对象与包文件](#41-松散对象与包文件)
  - [4.2. 垃圾回收](#42-垃圾回收)
  - [4.3. 底层对象操作](#43-底层对象操作)
- [5. 🤔 Git 有哪些环境变量与底层命令？](#5--git-有哪些环境变量与底层命令)
  - [5.1. 环境变量](#51-环境变量)
  - [5.2. 底层命令（Plumbing Commands）](#52-底层命令plumbing-commands)

<!-- endregion:toc -->

## 1. 🎯 本节内容

- Git 引用（References）与符号引用
- 包文件与垃圾回收（`git gc`）
- 环境变量与底层命令（Plumbing Commands）

## 2. 🫧 评价

- todo

## 3. 🤔 什么是 Git 引用与符号引用？

Git 引用（References）是指向提交对象 SHA-1 值的文件，存储在 `.git/refs/` 目录下。引用让你可以用有意义的名称（如分支名、标签名）来代替难记的 SHA-1 哈希值。

### 3.1. 引用类型

```bash
# 分支引用：存储在 .git/refs/heads/
cat .git/refs/heads/main
# 输出: abc1234...（40 位 SHA-1）

# 标签引用：存储在 .git/refs/tags/
cat .git/refs/tags/v1.0.0

# 远程引用：存储在 .git/refs/remotes/
cat .git/refs/remotes/origin/main
```

### 3.2. 符号引用

符号引用（Symbolic Reference）是指向另一个引用的引用。最重要的符号引用是 `HEAD`：

```bash
# 查看 HEAD 指向的引用
cat .git/HEAD
# 输出: ref: refs/heads/main

# 使用底层命令操作符号引用
git symbolic-ref HEAD
git symbolic-ref HEAD refs/heads/feature
```

底层命令 `git update-ref` 可以直接操作引用：

```bash
# 创建或更新引用
git update-ref refs/heads/test abc1234

# 删除引用
git update-ref -d refs/heads/test
```

## 4. 🤔 什么是包文件与垃圾回收？

### 4.1. 松散对象与包文件

Git 最初将每个对象存储为单独的文件（松散对象），存放在 `.git/objects/` 目录下。当松散对象过多时，Git 会将它们打包成**包文件（Packfile）**以节省空间和提高性能。

包文件使用增量压缩：只存储对象之间的差异，而非完整内容。

### 4.2. 垃圾回收

`git gc` 命令触发垃圾回收，执行以下操作：

```bash
# 手动触发垃圾回收
git gc

# 激进模式（更彻底的压缩，耗时更长）
git gc --aggressive

# 查看仓库大小
git count-objects -vH
```

垃圾回收的主要工作：

- 将松散对象打包为包文件
- 删除不可达的对象（没有任何引用指向的对象）
- 压缩引用文件

Git 会在一些操作后自动触发垃圾回收（如推送时），通常不需要手动执行。

### 4.3. 底层对象操作

```bash
# 查看对象内容
git cat-file -p abc1234

# 查看对象类型
git cat-file -t abc1234

# 查看对象大小
git cat-file -s abc1234

# 手动创建 blob 对象
echo "hello" | git hash-object -w --stdin
```

## 5. 🤔 Git 有哪些环境变量与底层命令？

### 5.1. 环境变量

Git 的行为可以通过环境变量来控制：

| 变量                 | 说明                   |
| -------------------- | ---------------------- |
| `GIT_DIR`            | 指定 `.git` 目录的位置 |
| `GIT_WORK_TREE`      | 指定工作区目录         |
| `GIT_AUTHOR_NAME`    | 覆盖提交的作者名       |
| `GIT_AUTHOR_EMAIL`   | 覆盖提交的作者邮箱     |
| `GIT_COMMITTER_NAME` | 覆盖提交者名           |
| `GIT_EDITOR`         | 指定编辑器             |
| `GIT_SSH_COMMAND`    | 指定 SSH 命令          |

```bash
# 使用环境变量临时覆盖配置
GIT_AUTHOR_NAME="Bot" git commit -m "自动提交"

# 指定 SSH 密钥
GIT_SSH_COMMAND="ssh -i ~/.ssh/deploy_key" git push
```

### 5.2. 底层命令（Plumbing Commands）

Git 命令分为**瓷器命令（Porcelain）**和**底层命令（Plumbing）**。日常使用的 `git add`、`git commit` 等属于瓷器命令，而底层命令提供了更精细的控制：

```bash
# 将文件写入对象数据库
git hash-object -w file.txt

# 更新暂存区
git update-index --add --cacheinfo 100644 <sha1> file.txt

# 将暂存区写入 tree 对象
git write-tree

# 创建 commit 对象
echo "提交消息" | git commit-tree <tree-sha1> -p <parent-sha1>

# 查看 tree 对象内容
git ls-tree HEAD

# 查看暂存区内容
git ls-files --stage
```

底层命令通常用于编写 Git 工具和脚本，或者深入理解 Git 的工作原理。日常开发中使用瓷器命令即可。
