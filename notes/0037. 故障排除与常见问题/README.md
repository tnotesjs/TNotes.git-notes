# [0037. 故障排除与常见问题](https://github.com/tnotesjs/TNotes.git-notes/tree/main/notes/0037.%20%E6%95%85%E9%9A%9C%E6%8E%92%E9%99%A4%E4%B8%8E%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98)

<!-- region:toc -->

- [1. 🎯 本节内容](#1--本节内容)
- [2. 🫧 评价](#2--评价)
- [3. 🤔 如何复盘合并冲突？](#3--如何复盘合并冲突)
  - [3.1. 冲突的常见原因](#31-冲突的常见原因)
  - [3.2. 预防冲突的策略](#32-预防冲突的策略)
  - [3.3. 复盘工具](#33-复盘工具)
- [4. 🤔 如何处理仓库中的敏感数据？](#4--如何处理仓库中的敏感数据)
  - [4.1. 使用 git filter-branch](#41-使用-git-filter-branch)
  - [4.2. 使用 BFG Repo-Cleaner（推荐）](#42-使用-bfg-repo-cleaner推荐)
  - [4.3. 预防措施](#43-预防措施)
- [5. 🤔 常见错误信息如何解读与解决？](#5--常见错误信息如何解读与解决)
  - [5.1. fatal: not a git repository](#51-fatal-not-a-git-repository)
  - [5.2. error: failed to push some refs](#52-error-failed-to-push-some-refs)
  - [5.3. fatal: refusing to merge unrelated histories](#53-fatal-refusing-to-merge-unrelated-histories)
  - [5.4. error: Your local changes would be overwritten](#54-error-your-local-changes-would-be-overwritten)
  - [5.5. detached HEAD state](#55-detached-head-state)
  - [5.6. fatal: bad object HEAD](#56-fatal-bad-object-head)

<!-- endregion:toc -->

## 1. 🎯 本节内容

- 合并冲突复盘
- 处理敏感数据（`git filter-branch` 与 BFG Repo-Cleaner）
- 常见错误信息解读与解决

## 2. 🫧 评价

- todo

## 3. 🤔 如何复盘合并冲突？

当合并频繁出现冲突时，需要分析原因并采取预防措施。

### 3.1. 冲突的常见原因

- 多人同时修改同一文件的同一区域
- 长期未同步的分支偏离过大
- 代码格式化工具产生的无意义变更
- 重命名或移动文件导致的路径冲突

### 3.2. 预防冲突的策略

```bash
# 频繁同步主分支的最新代码
git fetch origin
git rebase origin/main

# 拆分大的功能分支为小的增量修改
# 每个分支只做一件事，及时合并
```

### 3.3. 复盘工具

```bash
# 查看合并的详细过程
git log --merge

# 查看冲突文件的三方内容
git diff --cc file.txt

# 查看文件在两个分支上的不同版本
git diff main...feature -- file.txt

# 使用 rerere 自动记录和重用冲突解决方案
git config --global rerere.enabled true
```

开启 `rerere`（Reuse Recorded Resolution）后，Git 会记录你解决冲突的方式，下次遇到相同冲突时自动应用。

## 4. 🤔 如何处理仓库中的敏感数据？

如果不小心将密码、密钥等敏感数据提交到了仓库中，即使后续删除了文件，这些数据仍然存在于提交历史中。需要使用专门的工具彻底清除。

### 4.1. 使用 git filter-branch

```bash
# 从所有历史中删除指定文件
git filter-branch --force --index-filter \
  "git rm --cached --ignore-unmatch secrets.txt" \
  --prune-empty --tag-name-filter cat -- --all

# 清理引用和垃圾
git reflog expire --expire=now --all
git gc --prune=now --aggressive

# 强制推送到远程
git push origin --force --all
```

### 4.2. 使用 BFG Repo-Cleaner（推荐）

BFG 比 `git filter-branch` 速度快得多，使用也更简单：

```bash
# 安装 BFG（需要 Java）
brew install bfg

# 删除包含密码的文件
bfg --delete-files secrets.txt

# 替换文本中的敏感内容
bfg --replace-text passwords.txt

# 删除超大文件
bfg --strip-blobs-bigger-than 100M

# 清理并推送
git reflog expire --expire=now --all
git gc --prune=now --aggressive
git push --force
```

### 4.3. 预防措施

- 使用 `.gitignore` 忽略敏感文件
- 使用环境变量或 `.env` 文件管理敏感配置
- 在 `pre-commit` 钩子中检测敏感信息
- 在 GitHub 上开启 Secret Scanning 功能

## 5. 🤔 常见错误信息如何解读与解决？

### 5.1. fatal: not a git repository

```bash
# 原因：当前目录不是 Git 仓库
# 解决：初始化仓库或切换到正确的目录
git init
# 或
cd /path/to/your/repo
```

### 5.2. error: failed to push some refs

```bash
# 原因：远程仓库有本地没有的提交
# 解决：先拉取再推送
git pull --rebase origin main
git push origin main
```

### 5.3. fatal: refusing to merge unrelated histories

```bash
# 原因：两个没有共同祖先的仓库尝试合并
# 解决：允许合并不相关的历史
git pull origin main --allow-unrelated-histories
```

### 5.4. error: Your local changes would be overwritten

```bash
# 原因：切换分支时工作区有未提交的修改与目标分支冲突
# 解决：先提交或贮藏修改
git stash
git switch other-branch
# 完成后恢复
git switch -
git stash pop
```

### 5.5. detached HEAD state

```bash
# 原因：HEAD 直接指向某个提交而非分支
# 解决：如果需要保留修改，创建新分支
git switch -c new-branch
# 如果不需要，切换回已有分支
git switch main
```

### 5.6. fatal: bad object HEAD

```bash
# 原因：仓库数据损坏
# 解决：尝试修复
git fsck --full
# 如果无法修复，从远程重新克隆
```
