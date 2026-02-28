# [0013. 获取 Git 仓库](https://github.com/tnotesjs/TNotes.git-notes/tree/main/notes/0013.%20%E8%8E%B7%E5%8F%96%20Git%20%E4%BB%93%E5%BA%93)

<!-- region:toc -->

- [1. 🎯 本节内容](#1--本节内容)
- [2. 🫧 评价](#2--评价)
- [3. 🤔 如何初始化一个新的 Git 仓库？](#3--如何初始化一个新的-git-仓库)
- [4. 🤔 如何克隆远程仓库？](#4--如何克隆远程仓库)
- [5. 🤔 克隆时有哪些常用选项？](#5--克隆时有哪些常用选项)

<!-- endregion:toc -->

## 1. 🎯 本节内容

- 初始化新仓库（`git init`）
- 克隆远程仓库（`git clone`）
- 克隆时的常用选项（浅克隆、指定分支等）

## 2. 🫧 评价

- todo

## 3. 🤔 如何初始化一个新的 Git 仓库？

如果你有一个尚未进行版本控制的项目目录，可以使用 `git init` 来初始化一个新的 Git 仓库：

```bash
cd your-project
git init
```

该命令会在当前目录下创建一个 `.git` 子目录，这个目录包含了 Git 仓库所需的所有元数据和对象数据库。此时项目中的文件还没有被跟踪。

如果要开始跟踪现有文件，需要执行：

```bash
git add .
git commit -m "初始提交"
```

也可以直接在指定路径创建一个新的空仓库：

```bash
git init my-new-project
```

## 4. 🤔 如何克隆远程仓库？

如果你想获取一个已有的 Git 仓库的副本，使用 `git clone` 命令：

```bash
git clone https://github.com/user/repo.git
```

Git 会创建一个与仓库同名的目录，在其中初始化 `.git` 目录，拉取所有数据，并检出最新版本的工作副本。

可以指定自定义目录名：

```bash
git clone https://github.com/user/repo.git my-project
```

Git 支持多种传输协议：

- HTTPS：`https://github.com/user/repo.git`
- SSH：`git@github.com:user/repo.git`
- Git 协议：`git://github.com/user/repo.git`

SSH 协议需要配置 SSH 密钥，但不需要每次输入密码，适合日常开发使用。

## 5. 🤔 克隆时有哪些常用选项？

`git clone` 提供了多种实用选项来控制克隆行为：

```bash
# 浅克隆：只获取最近的 N 次提交，适合大型仓库
git clone --depth 1 https://github.com/user/repo.git

# 克隆指定分支
git clone -b develop https://github.com/user/repo.git

# 递归克隆（包含子模块）
git clone --recurse-submodules https://github.com/user/repo.git

# 只克隆单个分支（减少数据量）
git clone --single-branch https://github.com/user/repo.git
```

常用选项速查：

| 选项                   | 说明                            |
| ---------------------- | ------------------------------- |
| `--depth <n>`          | 创建浅克隆，只包含最近 n 次提交 |
| `-b <branch>`          | 克隆后检出指定分支              |
| `--recurse-submodules` | 自动初始化并更新子模块          |
| `--single-branch`      | 只克隆指定分支的历史            |
| `--bare`               | 创建裸仓库（无工作区）          |
