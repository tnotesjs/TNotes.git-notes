# [0014. 记录变更到仓库](https://github.com/tnotesjs/TNotes.git-notes/tree/main/notes/0014.%20%E8%AE%B0%E5%BD%95%E5%8F%98%E6%9B%B4%E5%88%B0%E4%BB%93%E5%BA%93)

<!-- region:toc -->

- [1. 🎯 本节内容](#1--本节内容)
- [2. 🫧 评价](#2--评价)
- [3. 🤔 如何检查文件状态？](#3--如何检查文件状态)
- [4. 🤔 如何跟踪新文件与暂存修改？](#4--如何跟踪新文件与暂存修改)
- [5. 🤔 如何配置忽略文件？](#5--如何配置忽略文件)
- [6. 🤔 如何查看文件差异？](#6--如何查看文件差异)
- [7. 🤔 如何提交更新？](#7--如何提交更新)
- [8. 🤔 如何跳过暂存区直接提交？](#8--如何跳过暂存区直接提交)
- [9. 🤔 如何移除文件？](#9--如何移除文件)
- [10. 🤔 如何移动或重命名文件？](#10--如何移动或重命名文件)

<!-- endregion:toc -->

## 1. 🎯 本节内容

- 检查文件状态（`git status`）
- 跟踪新文件与暂存修改（`git add`）
- 忽略文件（`.gitignore` 配置）
- 查看差异（`git diff` 与 `git diff --staged`）
- 提交更新（`git commit`）
- 跳过暂存区直接提交（`git commit -a`）
- 移除文件（`git rm`）
- 移动或重命名文件（`git mv`）

## 2. 🫧 评价

有了 Git 仓库之后，下一步就是学习 Git 仓库中的文件状态管理了，包括：

- 学会看 Git 仓库中的文件状态
- 知道如何在工作区（Working Directory）、暂存区（Staging Area / Index）、版本库（Repository / .git）之间流转文件

## 3. 🤔 如何检查文件状态？

使用 `git status` 命令可以查看当前工作区和暂存区的状态：

```bash
git status
```

输出会告诉你哪些文件被修改了、哪些文件已暂存、哪些文件未被跟踪。常见的状态信息包括：

- Untracked files：新创建但还未被 Git 跟踪的文件
- Changes not staged for commit：已修改但未暂存的文件
- Changes to be committed：已暂存等待提交的文件

如果想要更简洁的输出，可以使用：

```bash
git status -s
# 或
git status --short
```

简洁模式下，文件状态用两列字符表示：左列是暂存区状态，右列是工作区状态。例如 `M` 表示已修改，`A` 表示新添加，`??` 表示未跟踪。

## 4. 🤔 如何跟踪新文件与暂存修改？

使用 `git add` 命令来跟踪新文件或暂存已修改的文件：

```bash
# 添加单个文件
git add README.md

# 添加整个目录
git add src/

# 添加所有变更（包括新文件、修改和删除）
git add .

# 添加所有已跟踪文件的修改（不包括新文件）
git add -u

# 交互式添加（可以选择部分内容暂存）
git add -p
```

`git add` 是一个多功能命令：对未跟踪的文件，它开始跟踪；对已跟踪的文件，它将修改暂存。如果暂存后又修改了文件，需要再次运行 `git add` 来暂存最新的修改。

## 5. 🤔 如何配置忽略文件？

通过 `.gitignore` 文件可以告诉 Git 哪些文件不需要纳入版本控制。在项目根目录创建 `.gitignore` 文件：

```gitignore
# 忽略所有 .log 文件
*.log

# 忽略 node_modules 目录
node_modules/

# 忽略构建输出
dist/
build/

# 忽略环境配置文件
.env
.env.local

# 忽略操作系统生成的文件
.DS_Store
Thumbs.db
```

`.gitignore` 的匹配规则：

- 以 `#` 开头的行是注释
- 以 `/` 结尾表示目录
- 以 `!` 开头表示取反（不忽略）
- 支持 glob 模式匹配（`*`、`?`、`[abc]`、``）

注意：`.gitignore` 只能忽略未被跟踪的文件。如果文件已经被跟踪，需要先用 `git rm --cached <file>` 取消跟踪。

## 6. 🤔 如何查看文件差异？

使用 `git diff` 查看具体的修改内容：

```bash
# 查看工作区与暂存区的差异（未暂存的修改）
git diff

# 查看暂存区与上次提交的差异（已暂存的修改）
git diff --staged
# 或
git diff --cached

# 查看工作区与上次提交的差异
git diff HEAD

# 查看指定文件的差异
git diff README.md
```

`git diff` 默认使用统一格式（unified diff），用 `+` 表示新增行，`-` 表示删除行。

## 7. 🤔 如何提交更新？

使用 `git commit` 将暂存区的内容提交到仓库：

```bash
# 打开编辑器输入提交消息
git commit

# 使用 -m 参数直接指定提交消息
git commit -m "添加用户登录功能"

# 使用 -v 参数在编辑器中显示差异
git commit -v
```

每次提交都会生成一个独一无二的 SHA-1 校验和，记录提交者信息、时间戳和提交消息。

编写好的提交消息很重要，建议遵循以下规范：

- 第一行简短概括（不超过 50 个字符）
- 空一行后写详细说明（如有必要）
- 使用祈使语气（如"添加"而非"添加了"）

## 8. 🤔 如何跳过暂存区直接提交？

如果你想跳过 `git add` 步骤，直接提交所有已跟踪文件的修改，可以使用 `-a` 参数：

```bash
git commit -a -m "修复导航栏样式问题"
```

这等价于先执行 `git add -u` 然后 `git commit`。注意，`-a` 参数只会暂存已跟踪文件的修改，不会添加新的未跟踪文件。

## 9. 🤔 如何移除文件？

从 Git 中移除文件需要使用 `git rm`：

```bash
# 从工作区和暂存区同时移除文件
git rm file.txt

# 仅从 Git 中移除（保留工作区的文件）
git rm --cached file.txt

# 移除整个目录
git rm -r directory/

# 使用通配符
git rm log/\*.log
```

如果只是简单地用 `rm` 命令删除文件，Git 会将其标记为"已删除但未暂存"，你仍需要执行 `git rm` 来暂存这个删除操作。

## 10. 🤔 如何移动或重命名文件？

使用 `git mv` 命令来移动或重命名文件：

```bash
git mv old_name.txt new_name.txt
git mv file.txt src/file.txt
```

这个命令实际上等价于以下三条命令的组合：

```bash
mv old_name.txt new_name.txt
git rm old_name.txt
git add new_name.txt
```

Git 会自动检测到这是一个重命名操作，并在提交历史中正确记录。
