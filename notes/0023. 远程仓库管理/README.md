# [0023. 远程仓库管理](https://github.com/tnotesjs/TNotes.git-notes/tree/main/notes/0023.%20%E8%BF%9C%E7%A8%8B%E4%BB%93%E5%BA%93%E7%AE%A1%E7%90%86)

<!-- region:toc -->

- [1. 🎯 本节内容](#1--本节内容)
- [2. 🫧 评价](#2--评价)
- [3. 🤔 如何查看远程仓库信息？](#3--如何查看远程仓库信息)
- [4. 🤔 如何添加与移除远程仓库？](#4--如何添加与移除远程仓库)
- [5. 🤔 本地仓库和远程仓库的对应关系是？](#5--本地仓库和远程仓库的对应关系是)
- [6. 🤔 Git 将远程仓库的这些配置数据写入到了什么地方？](#6--git-将远程仓库的这些配置数据写入到了什么地方)
- [7. 🤔 如何获取与拉取远程数据？](#7--如何获取与拉取远程数据)
- [8. 🤔 如何推送到远程仓库？](#8--如何推送到远程仓库)
- [9. 🤔 什么是远程分支与跟踪分支？](#9--什么是远程分支与跟踪分支)

<!-- endregion:toc -->

## 1. 🎯 本节内容

- 查看远程仓库（`git remote`）
- 添加与移除远程仓库（`git remote add/remove`）
- 获取与拉取远程数据（`git fetch` / `git pull`）
- 推送到远程仓库（`git push`）
- 远程分支与跟踪分支

## 2. 🫧 评价

- todo

## 3. 🤔 如何查看远程仓库信息？

使用 `git remote` 命令管理远程仓库的引用：

```bash
# 查看已配置的远程仓库名称
git remote

# 查看远程仓库的 URL
git remote -v

# 查看某个远程仓库的详细信息
git remote show origin
```

`git remote show origin` 会显示远程仓库的 URL、跟踪分支信息、`git pull` 和 `git push` 的配置等详细内容。

克隆仓库时，Git 会自动为远程仓库添加一个名为 `origin` 的引用。

示例：

```bash
git remote
# origin
# 表示当前仓库的远程仓库名称为 origin

git remote -v
# origin  https://github.com/tnotesjs/TNotes.git-notes (fetch)
# origin  https://github.com/tnotesjs/TNotes.git-notes (push)
# 表示当前仓库的远程仓库的 URL 为 https://github.com/tnotesjs/TNotes.git-notes

git remote show origin
# * remote origin
#   Fetch URL: https://github.com/tnotesjs/TNotes.git-notes
#   Push  URL: https://github.com/tnotesjs/TNotes.git-notes
#   HEAD branch: main
#   Remote branch:
#     main tracked
#   Local branch configured for 'git pull':
#     main merges with remote main
#   Local ref configured for 'git push':
#     main pushes to main (up to date)
```

`git remote show origin` 输出内容注解：

- `Fetch/Push URL`：分别定义了拉取（下载）和推送（上传）代码的远程地址，两者可以不同。
- `HEAD branch`：指示远程仓库的默认主分支（通常是 main 或 master）。
- `Remote branch (tracked)`：列出远程存在的分支及其在本地是否有对应的跟踪分支（tracked 表示已建立跟踪关系）。
- `Local branch configured for 'git pull'`：显示执行 `git pull` 时，本地哪个分支会与远程哪个分支进行合并。
- `Local ref configured for 'git push'`：显示执行 `git push` 时，本地分支会推送到远程的哪个分支，以及当前状态（如 up to date 表示同步，ahead 表示领先，behind 表示落后）。
- `Stale tracking branches`：如果远程分支已被删除但本地仍保留引用，这里会提示需要运行 `git remote prune origin` 进行清理。

## 4. 🤔 如何添加与移除远程仓库？

```bash
# 添加远程仓库
git remote add upstream https://github.com/original/repo.git
# 常见的做法是用 origin 指向自己的 Fork，用 upstream 指向原始仓库

# 重命名远程仓库
git remote rename origin old-origin

# 移除远程仓库
git remote remove upstream
# 或
git remote rm upstream

# 修改远程仓库的 URL
git remote set-url origin https://github.com/user/new-repo.git
```

## 5. 🤔 本地仓库和远程仓库的对应关系是？

一个本地仓库可以关联多个远程仓库。

```text
       +-----------------------+
       |   本地仓库 (Local)    |
       |  (包含所有分支历史)   |
       +-----------+-----------+
                   |
      +------------+------------+------------+
      |            |            |            |
      v            v            v            v
+-----------+ +-----------+ +-----------+ +-----------+
|  Remote A | |  Remote B | |  Remote C | |  Remote D |
| (origin)  | | (upstream)| |  (gitee)  | | (backup)  |
|           | |           | |           | |           |
|  GitHub   | |  Official | |   Mirror  | |   NAS     |
|           | |   Repo    | |   Site    | |  Server   |
+-----------+ +-----------+ +-----------+ +-----------+
      ^            ^            ^            ^
      |            |            |            |
      +------------+------------+------------+
                   |
          (通过 git fetch/push 交互)
```

## 6. 🤔 Git 将远程仓库的这些配置数据写入到了什么地方？

这些配置数据存储在本地仓库根目录下的 `.git/config` 文件中。

路径为：`<你的项目根目录>/.git/config`

![img](https://cdn.jsdelivr.net/gh/tnotesjs/imgs-2026@main/2026-03-13-15-27-52.png)

如果要添加多个 Remote，也可以直接修改这个配置文件：

```text
...

[remote "origin"]
  url = https://github.com/tnotesjs/TNotes.git-notes
  fetch = +refs/heads/*:refs/remotes/origin/*

[remote "upstream"]
  url = https://github.com/original/project.git
  fetch = +refs/heads/*:refs/remotes/upstream/*

[remote "gitee"]
  url = git@gitee.com:mirror/project.git
  fetch = +refs/heads/*:refs/remotes/gitee/*

...
```

## 7. 🤔 如何获取与拉取远程数据？

```bash
# 获取远程仓库的最新数据（不合并）
git fetch origin

# 获取所有远程仓库的数据
git fetch --all

# 拉取远程数据并自动合并到当前分支
git pull

# 拉取时使用变基而非合并
git pull --rebase

# 拉取指定远程分支
git pull origin main
```

`git fetch` 和 `git pull` 的区别：

| 命令                | 行为                              |
| ------------------- | --------------------------------- |
| `git fetch`         | 下载远程数据到本地，不修改工作区  |
| `git pull`          | 相当于 `git fetch` + `git merge`  |
| `git pull --rebase` | 相当于 `git fetch` + `git rebase` |

推荐在拉取时使用 `--rebase`，这样可以保持更线性的提交历史。

## 8. 🤔 如何推送到远程仓库？

```bash
# 推送当前分支到远程
git push

# 推送到指定远程仓库和分支
git push origin main

# 推送并设置上游跟踪关系
git push -u origin feature

# 推送所有分支
git push --all

# 强制推送（慎用）
git push --force
# 更安全的强制推送
git push --force-with-lease
```

`--force-with-lease` 比 `--force` 更安全，它会在推送前检查远程分支是否被他人更新过，如果有则拒绝推送，避免覆盖他人的工作。

首次推送新分支时，使用 `-u` 参数设置上游跟踪关系，之后就可以直接使用 `git push` 而无需指定远程和分支名。

## 9. 🤔 什么是远程分支与跟踪分支？

远程跟踪分支（如 `origin/main`）是远程分支状态的本地引用，它们记录了上次与远程仓库通信时各分支的位置。你不能直接修改它们，它们会在 `git fetch` 时自动更新。

跟踪分支（Tracking Branch）是与远程跟踪分支关联的本地分支。当你在跟踪分支上运行 `git pull` 时，Git 会自动知道从哪个远程分支获取数据。

```bash
# 创建跟踪远程分支的本地分支
git switch -c feature origin/feature

# 简写形式（自动创建同名跟踪分支）
git switch feature

# 查看所有跟踪分支的关系
git branch -vv

# 设置当前分支跟踪的远程分支
git branch -u origin/main
```

`git branch -vv` 的输出会显示每个本地分支跟踪的远程分支，以及本地分支相对于远程分支是领先（ahead）还是落后（behind）了多少次提交。
