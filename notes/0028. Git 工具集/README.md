# [0028. Git 工具集](https://github.com/tnotesjs/TNotes.git-notes/tree/main/notes/0028.%20Git%20%E5%B7%A5%E5%85%B7%E9%9B%86)

<!-- region:toc -->

- [1. 🎯 本节内容](#1--本节内容)
- [2. 🫧 评价](#2--评价)
- [3. 🤔 如何清理工作目录？](#3--如何清理工作目录)
- [4. 🤔 如何使用 GPG 签名签署工作？](#4--如何使用-gpg-签名签署工作)
- [5. 🤔 如何使用 git grep 搜索代码？](#5--如何使用-git-grep-搜索代码)
- [6. 🤔 如何使用 git bisect 定位错误提交？](#6--如何使用-git-bisect-定位错误提交)

<!-- endregion:toc -->

## 1. 🎯 本节内容

- 贮藏（Stashing）：`git stash`
- 清理工作目录（`git clean`）
- 签署工作（GPG 签名）
- 搜索（`git grep`）
- 二分查找（`git bisect`）定位错误提交

## 2. 🫧 评价

- todo

## 3. 🤔 如何清理工作目录？

`git clean` 用于删除工作目录中未被跟踪的文件：

```bash
# 预览将要删除的文件（干跑模式）
git clean -n

# 删除未跟踪的文件
git clean -f

# 删除未跟踪的文件和目录
git clean -fd

# 删除未跟踪的文件（包括被 .gitignore 忽略的）
git clean -fx

# 交互式清理
git clean -i
```

`git clean` 是一个危险操作，因为被删除的文件无法恢复。建议先使用 `-n`（干跑模式）预览将要删除的文件，确认无误后再执行。

## 4. 🤔 如何使用 GPG 签名签署工作？

Git 支持使用 GPG 密钥对提交和标签进行签名，以验证提交者的身份：

```bash
# 查看已有的 GPG 密钥
gpg --list-secret-keys --keyid-format=long

# 配置 Git 使用的 GPG 密钥
git config --global user.signingkey YOUR_KEY_ID

# 创建签名提交
git commit -S -m "签名提交"

# 设置默认对所有提交签名
git config --global commit.gpgsign true

# 创建签名标签
git tag -s v1.0.0 -m "签名标签"

# 验证提交的签名
git log --show-signature

# 验证标签的签名
git tag -v v1.0.0
```

在 GitHub 等平台上，签名提交会显示 "Verified" 标记，表明提交确实来自声称的作者。

## 5. 🤔 如何使用 git grep 搜索代码？

`git grep` 可以在仓库中快速搜索文本，比系统的 `grep` 命令更快，因为它只搜索被 Git 跟踪的文件：

```bash
# 搜索包含关键字的文件
git grep "TODO"

# 显示行号
git grep -n "TODO"

# 只显示匹配的文件名
git grep -l "TODO"

# 统计每个文件的匹配次数
git grep -c "TODO"

# 在指定分支中搜索
git grep "TODO" main

# 搜索时使用正则表达式
git grep -e "function.*init"

# 搜索同时匹配多个条件的文件
git grep -e "import" --and -e "React"

# 在指定目录中搜索
git grep "TODO" -- src/
```

## 6. 🤔 如何使用 git bisect 定位错误提交？

`git bisect` 使用二分查找法在提交历史中快速定位引入错误的提交。你只需告诉 Git 哪个提交是"好的"（没有 bug），哪个是"坏的"（有 bug），Git 会自动检出中间的提交让你测试。

```bash
# 开始二分查找
git bisect start

# 标记当前提交为有问题的
git bisect bad

# 标记一个已知没问题的提交
git bisect good abc1234

# Git 会检出中间的提交，你测试后标记结果
git bisect good  # 如果这个提交没问题
git bisect bad   # 如果这个提交有问题

# 找到问题提交后，结束二分查找
git bisect reset
```

还可以使用脚本自动化测试：

```bash
# 使用测试脚本自动二分查找
git bisect start HEAD abc1234
git bisect run npm test
```

Git 会自动运行测试脚本，根据退出码判断每个提交的好坏，直到找到引入错误的提交。
