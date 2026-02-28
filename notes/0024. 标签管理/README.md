# [0024. 标签管理](https://github.com/tnotesjs/TNotes.git-notes/tree/main/notes/0024.%20%E6%A0%87%E7%AD%BE%E7%AE%A1%E7%90%86)

<!-- region:toc -->

- [1. 🎯 本节内容](#1--本节内容)
- [2. 🫧 评价](#2--评价)
- [3. 🤔 如何列出标签？](#3--如何列出标签)
- [4. 🤔 如何创建标签？](#4--如何创建标签)
  - [4.1. 轻量标签（Lightweight Tag）](#41-轻量标签lightweight-tag)
  - [4.2. 附注标签（Annotated Tag）](#42-附注标签annotated-tag)
- [5. 🤔 如何共享标签到远程仓库？](#5--如何共享标签到远程仓库)
- [6. 🤔 如何删除标签？](#6--如何删除标签)

<!-- endregion:toc -->

## 1. 🎯 本节内容

- 列出标签（`git tag`）
- 创建标签（轻量标签与附注标签）
- 共享标签（`git push --tags`）
- 删除标签（本地与远程）

## 2. 🫧 评价

- todo

## 3. 🤔 如何列出标签？

```bash
# 列出所有标签
git tag

# 按通配符模式列出标签
git tag -l "v1.8.*"

# 按版本号排序
git tag --sort=version:refname

# 查看标签详细信息
git show v1.0.0
```

`git tag` 会按字母顺序列出标签。如果标签数量很多，可以使用 `-l` 参数配合通配符来过滤。

## 4. 🤔 如何创建标签？

Git 支持两种类型的标签：

### 4.1. 轻量标签（Lightweight Tag）

轻量标签只是一个指向特定提交的引用，不包含额外信息：

```bash
git tag v1.0.0
```

### 4.2. 附注标签（Annotated Tag）

附注标签是存储在 Git 数据库中的完整对象，包含标签名、邮箱、日期和标签消息，推荐使用：

```bash
# 创建附注标签
git tag -a v1.0.0 -m "发布 1.0.0 版本"

# 为历史某个提交打标签
git tag -a v0.9.0 abc1234 -m "补打 0.9.0 版本标签"
```

通常建议使用附注标签，因为它包含了更多的元数据信息，方便日后追溯。

## 5. 🤔 如何共享标签到远程仓库？

默认情况下，`git push` 不会将标签推送到远程仓库，需要显式推送：

```bash
# 推送单个标签
git push origin v1.0.0

# 推送所有标签
git push origin --tags

# 只推送附注标签（不推送轻量标签）
git push origin --follow-tags
```

可以设置 `git push` 默认推送附注标签：

```bash
git config --global push.followTags true
```

## 6. 🤔 如何删除标签？

```bash
# 删除本地标签
git tag -d v1.0.0

# 删除远程标签
git push origin --delete v1.0.0
# 或
git push origin :refs/tags/v1.0.0
```

删除远程标签需要两步：先删除本地标签，再推送删除操作到远程仓库。

如果想要检出某个标签对应的代码：

```bash
# 这会使 HEAD 进入分离状态
git checkout v1.0.0

# 如果需要在标签基础上开发，创建一个新分支
git switch -c fix-v1.0.0 v1.0.0
```
