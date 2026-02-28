# [0025. 多人协作流程](https://github.com/tnotesjs/TNotes.git-notes/tree/main/notes/0025.%20%E5%A4%9A%E4%BA%BA%E5%8D%8F%E4%BD%9C%E6%B5%81%E7%A8%8B)

<!-- region:toc -->

- [1. 🎯 本节内容](#1--本节内容)
- [2. 🫧 评价](#2--评价)
- [3. 🤔 什么是派生（Fork）与克隆？](#3--什么是派生fork与克隆)
- [4. 🤔 如何保持本地仓库与上游同步？](#4--如何保持本地仓库与上游同步)
- [5. 🤔 如何推送代码与发起 Pull Request？](#5--如何推送代码与发起-pull-request)
- [6. 🤔 代码审查与合并流程是怎样的？](#6--代码审查与合并流程是怎样的)

<!-- endregion:toc -->

## 1. 🎯 本节内容

- 派生（Fork）与克隆
- 保持本地仓库最新（`git pull --rebase`）
- 推送代码与发起 Pull Request / Merge Request
- 代码审查与合并

## 2. 🫧 评价

- todo

## 3. 🤔 什么是派生（Fork）与克隆？

**克隆（Clone）** 是将远程仓库完整复制到本地的操作。克隆后的本地仓库与远程仓库保持关联。

**派生（Fork）** 是在 Git 托管平台（如 GitHub）上将他人的仓库复制到自己的账户下。Fork 后你拥有一个独立的远程仓库副本，可以自由修改而不影响原始仓库。

典型的开源贡献流程：

```bash
# 1. 在 GitHub 上 Fork 原始仓库

# 2. 克隆自己的 Fork 到本地
git clone https://github.com/your-name/repo.git

# 3. 添加原始仓库作为上游
git remote add upstream https://github.com/original/repo.git
```

这样你就有了两个远程仓库：`origin` 指向自己的 Fork，`upstream` 指向原始仓库。

## 4. 🤔 如何保持本地仓库与上游同步？

在开发过程中，原始仓库可能会有新的提交，需要定期同步：

```bash
# 获取上游仓库的最新数据
git fetch upstream

# 切换到主分支
git switch main

# 将上游的更新合并到本地主分支（推荐用变基）
git rebase upstream/main

# 将更新推送到自己的 Fork
git push origin main
```

使用 `git pull --rebase` 可以简化这个流程：

```bash
git switch main
git pull --rebase upstream main
git push origin main
```

保持与上游同步可以减少后续提交 Pull Request 时的冲突。

## 5. 🤔 如何推送代码与发起 Pull Request？

完成功能开发后，将代码推送到自己的 Fork 并发起 Pull Request：

```bash
# 1. 确保功能分支基于最新的上游代码
git switch feature
git rebase main

# 2. 推送功能分支到自己的 Fork
git push origin feature
```

然后在 GitHub/GitLab 等平台上：

1. 访问原始仓库页面
2. 点击 "New Pull Request" 或 "Create Merge Request"
3. 选择要合并的分支（从你的 Fork 的 feature 分支合并到原始仓库的 main 分支）
4. 填写 PR 标题和描述，说明修改内容
5. 提交 PR

编写良好的 PR 描述应包含：修改的目的、实现方式、测试情况以及相关的 Issue 编号。

## 6. 🤔 代码审查与合并流程是怎样的？

Pull Request 提交后，通常需要经过代码审查（Code Review）才能合并：

**审查流程**：

1. 维护者或团队成员审查代码变更
2. 审查者可以添加评论、提出修改建议
3. 开发者根据反馈修改代码并推送更新
4. 审查通过后，维护者批准并合并 PR

**常见的合并方式**：

| 方式             | 说明                           | 适用场景           |
| ---------------- | ------------------------------ | ------------------ |
| Merge Commit     | 创建合并提交，保留完整分支历史 | 功能分支的常规合并 |
| Squash and Merge | 将所有提交压缩为一个提交       | 提交历史较碎的分支 |
| Rebase and Merge | 将提交变基到目标分支上         | 追求线性历史       |

合并后记得清理已合并的分支：

```bash
# 删除本地功能分支
git branch -d feature

# 删除远程功能分支
git push origin --delete feature
```
