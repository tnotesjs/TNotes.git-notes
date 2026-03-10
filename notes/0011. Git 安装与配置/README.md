# [0011. Git 安装与配置](https://github.com/tnotesjs/TNotes.git-notes/tree/main/notes/0011.%20Git%20%E5%AE%89%E8%A3%85%E4%B8%8E%E9%85%8D%E7%BD%AE)

<!-- region:toc -->

- [1. 🎯 本节内容](#1--本节内容)
- [2. 🫧 评价](#2--评价)
- [3. 🤔 如何在不同操作系统上安装 Git？](#3--如何在不同操作系统上安装-git)
  - [3.1. Windows](#31-windows)
  - [3.2. macOS](#32-macos)
  - [3.3. Linux](#33-linux)
  - [3.4. 验证安装是否成功](#34-验证安装是否成功)
- [4. 🤔 Git 命令行的基础用法有哪些？](#4--git-命令行的基础用法有哪些)
- [5. 🤔 如何配置用户信息？](#5--如何配置用户信息)
- [6. 🤔 如何配置文本编辑器与命令别名？](#6--如何配置文本编辑器与命令别名)
  - [6.1. 配置默认编辑器](#61-配置默认编辑器)
  - [6.2. 配置命令别名](#62-配置命令别名)
- [7. 🤔 如何查看和管理 Git 配置信息？](#7--如何查看和管理-git-配置信息)
- [8. 🔗 引用](#8--引用)

<!-- endregion:toc -->

## 1. 🎯 本节内容

- 在 Windows、macOS、Linux 上安装 Git
- Git 命令行基础
- 配置用户信息（user.name 和 user.email）
- 配置文本编辑器与别名（alias）
- 查看配置信息（`git config`）

## 2. 🫧 评价

## 3. 🤔 如何在不同操作系统上安装 Git？

### 3.1. Windows

访问 Git 官网下载安装包，运行后按照向导完成安装。安装完成后可以使用 Git Bash 或在 CMD/PowerShell 中使用 Git。

### 3.2. macOS

macOS 通常自带 Git，但版本可能较旧。推荐通过 Homebrew 安装最新版本：

```bash
brew install git
```

### 3.3. Linux

不同发行版使用对应的包管理器：

```bash
# Debian/Ubuntu
sudo apt install git

# Fedora
sudo dnf install git

# Arch Linux
sudo pacman -S git
```

### 3.4. 验证安装是否成功

安装完成后，使用以下命令验证安装是否成功：

```bash
git --version
```

输出示例：

![img](https://cdn.jsdelivr.net/gh/tnotesjs/imgs-2026@main/2026-03-10-16-30-29.png)

提示：关于 Git 的安装教程，网上有一堆，如果安装过程中遇到啥问题，把错误的截图直接发给 AI 基本都能解决。

## 4. 🤔 Git 命令行的基础用法有哪些？

Git 的所有操作都可以通过命令行完成。常用的帮助命令：

```bash
# 查看 Git 帮助信息
git help

# 查看某个具体命令的帮助
git help <command>
git <command> --help

# 查看简洁版帮助
git <command> -h
```

Git 命令的基本格式为 `git <command> [options] [arguments]`。大多数命令都支持 `--help` 参数来查看详细用法说明。

示例：

```bash
git status --help
# 该命令会利用你的默认浏览器打开本地的 git-doc
# 比如：file:///C:/Program%20Files/Git/mingw64/share/doc/git-doc/git-status.html
```

提示：git-doc 中的帮助文档默认是英文的，若英文文档阅读比较吃力，你可以利用浏览器默认的翻译功能将内容转为中文。

::: swiper

![1](https://cdn.jsdelivr.net/gh/tnotesjs/imgs-2026@main/2026-03-10-16-36-01.png)

![2](https://cdn.jsdelivr.net/gh/tnotesjs/imgs-2026@main/2026-03-10-16-36-09.png)

:::

## 5. 🤔 如何配置用户信息？

安装 Git 后的第一件事就是设置用户名和邮箱地址，这些信息会被写入每一次提交中：

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

`--global` 表示这是全局配置，对当前用户的所有仓库生效。如果想为某个项目单独设置，去掉 `--global` 即可：

```bash
cd your-project
git config user.name "Project Specific Name"
git config user.email "project@example.com"
```

这个用户信息配置的操作往往只需要做一次即可，目的是让 Git 知道你是谁，除非你需要在不同项目中使用不同的身份信息。

## 6. 🤔 如何配置文本编辑器与命令别名？

### 6.1. 配置默认编辑器

Git 在需要输入信息时（如编写提交消息）会调用默认编辑器。可以设置为你常用的编辑器：

```bash
# 设置为 VS Code
git config --global core.editor "code --wait"

# 设置为 Vim
git config --global core.editor vim

# 设置为 Nano
git config --global core.editor nano
```

### 6.2. 配置命令别名

通过别名（alias）可以为常用命令创建简短的替代：

```bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.lg "log --oneline --graph --all"
```

配置后就可以使用 `git st` 代替 `git status`，`git lg` 代替冗长的日志命令。

你可以通过 `git config --global --get-regexp ^alias\.` 来查看所有配置的别名。

## 7. 🤔 如何查看和管理 Git 配置信息？

Git 的配置分为三个级别，优先级从低到高：

| 级别   | 文件位置         | 命令参数   |
| ------ | ---------------- | ---------- |
| 系统级 | `/etc/gitconfig` | `--system` |
| 用户级 | `~/.gitconfig`   | `--global` |
| 仓库级 | `.git/config`    | `--local`  |

常用的配置查看命令：

```bash
# 查看所有配置及其来源
git config --list --show-origin

# 查看某个具体配置项
git config user.name

# 查看全局配置
git config --global --list
```

如果需要删除某个配置项：

```bash
git config --global --unset alias.st
```

## 8. 🔗 引用

- [Git - 官方网站][1]

[1]: https://git-scm.com/download/win
