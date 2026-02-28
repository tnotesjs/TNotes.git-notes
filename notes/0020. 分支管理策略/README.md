# [0020. 分支管理策略](https://github.com/tnotesjs/TNotes.git-notes/tree/main/notes/0020.%20%E5%88%86%E6%94%AF%E7%AE%A1%E7%90%86%E7%AD%96%E7%95%A5)

<!-- region:toc -->

- [1. 🎯 本节内容](#1--本节内容)
- [2. 🫧 评价](#2--评价)
- [3. 🤔 如何查看所有分支？](#3--如何查看所有分支)
- [4. 🤔 如何重命名与删除分支？](#4--如何重命名与删除分支)
  - [4.1. 重命名分支](#41-重命名分支)
  - [4.2. 删除分支](#42-删除分支)
- [5. 🤔 常见的分支工作流有哪些？](#5--常见的分支工作流有哪些)
  - [5.1. Git Flow](#51-git-flow)
  - [5.2. GitHub Flow](#52-github-flow)
  - [5.3. Trunk Based Development](#53-trunk-based-development)

<!-- endregion:toc -->

## 1. 🎯 本节内容

- 查看所有分支（本地与远程）
- 重命名与删除分支（`git branch -d/-D`）
- 常见的分支工作流（Git Flow、GitHub Flow、Trunk Based）

## 2. 🫧 评价

- todo

## 3. 🤔 如何查看所有分支？

```bash
# 查看本地分支（* 标记当前分支）
git branch

# 查看本地分支及最后一次提交
git branch -v

# 查看远程分支
git branch -r

# 查看所有分支（本地 + 远程）
git branch -a

# 查看已合并到当前分支的分支
git branch --merged

# 查看未合并到当前分支的分支
git branch --no-merged

# 查看已合并到指定分支的分支
git branch --merged main
```

远程分支的显示格式为 `origin/branch-name`，表示远程仓库 `origin` 上的分支。

## 4. 🤔 如何重命名与删除分支？

### 4.1. 重命名分支

```bash
# 重命名当前分支
git branch -m new-name

# 重命名指定分支
git branch -m old-name new-name

# 重命名后更新远程分支
git push origin -d old-name
git push origin new-name
git push origin -u new-name
```

### 4.2. 删除分支

```bash
# 删除已合并的本地分支
git branch -d feature

# 强制删除分支（即使未合并）
git branch -D feature

# 删除远程分支
git push origin --delete feature
# 或
git push origin :feature

# 清理本地已失效的远程跟踪分支
git fetch --prune
```

`-d` 参数会在分支未合并时拒绝删除，起到安全保护作用。如果确定要删除未合并的分支，使用 `-D` 强制删除。

## 5. 🤔 常见的分支工作流有哪些？

### 5.1. Git Flow

Git Flow 是一种经典的分支模型，适合有计划发布周期的项目：

- `main`：生产环境代码，始终保持可发布状态
- `develop`：开发主线，集成最新的开发成果
- `feature/*`：功能分支，从 `develop` 创建，完成后合并回 `develop`
- `release/*`：预发布分支，用于发布前的测试和修复
- `hotfix/*`：热修复分支，从 `main` 创建，修复后合并到 `main` 和 `develop`

### 5.2. GitHub Flow

GitHub Flow 更加简洁，适合持续部署的项目：

1. 从 `main` 创建功能分支
2. 在功能分支上开发和提交
3. 发起 Pull Request
4. 代码审查通过后合并到 `main`
5. 部署 `main` 分支

### 5.3. Trunk Based Development

基于主干的开发模式，所有开发者在同一主干（`main`）上协作：

- 开发者频繁地向 `main` 提交小的改动
- 使用特性开关（Feature Flags）控制功能的启用
- 适合拥有完善 CI/CD 流水线的团队
- 减少了长期分支带来的合并复杂度

每种工作流都有其适用场景，团队应根据项目规模、发布节奏和团队规模来选择。
