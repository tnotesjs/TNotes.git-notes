# [0032. 子模块与子树](https://github.com/tnotesjs/TNotes.git-notes/tree/main/notes/0032.%20%E5%AD%90%E6%A8%A1%E5%9D%97%E4%B8%8E%E5%AD%90%E6%A0%91)

<!-- region:toc -->

- [1. 🎯 本节内容](#1--本节内容)
- [2. 🫧 评价](#2--评价)
- [3. 🤔 如何在仓库中嵌入子模块？](#3--如何在仓库中嵌入子模块)
- [4. 🤔 子模块如何更新、克隆与问题处理？](#4--子模块如何更新克隆与问题处理)
  - [4.1. 克隆包含子模块的仓库](#41-克隆包含子模块的仓库)
  - [4.2. 更新子模块](#42-更新子模块)
  - [4.3. 常见问题](#43-常见问题)
  - [4.4. 在子模块中工作](#44-在子模块中工作)
- [5. 🤔 如何使用 git subtree 管理依赖？](#5--如何使用-git-subtree-管理依赖)
  - [5.1. 更新子树](#51-更新子树)
  - [5.2. 推送修改回外部仓库](#52-推送修改回外部仓库)
  - [5.3. 子模块 vs 子树](#53-子模块-vs-子树)

<!-- endregion:toc -->

## 1. 🎯 本节内容

- 在仓库中嵌入子模块（`git submodule`）
- 子模块的更新、克隆与问题处理
- 使用子树（`git subtree`）管理依赖

## 2. 🫧 评价

- todo

## 3. 🤔 如何在仓库中嵌入子模块？

Git 子模块（Submodule）允许你在一个 Git 仓库中嵌入另一个 Git 仓库，适合管理项目的外部依赖。

```bash
# 添加子模块
git submodule add https://github.com/user/library.git libs/library

# 添加子模块并指定分支
git submodule add -b main https://github.com/user/library.git libs/library
```

添加子模块后，Git 会：

1. 在指定路径克隆子模块仓库
2. 创建 `.gitmodules` 文件记录子模块信息
3. 在 `.git/config` 中注册子模块

`.gitmodules` 文件内容示例：

```ini
[submodule "libs/library"]
    path = libs/library
    url = https://github.com/user/library.git
    branch = main
```

子模块在父仓库中以特定的提交引用形式存在，父仓库只记录子模块应该检出的提交哈希。

## 4. 🤔 子模块如何更新、克隆与问题处理？

### 4.1. 克隆包含子模块的仓库

```bash
# 方式一：克隆时自动初始化子模块
git clone --recurse-submodules https://github.com/user/project.git

# 方式二：先克隆，再初始化子模块
git clone https://github.com/user/project.git
cd project
git submodule init
git submodule update
```

### 4.2. 更新子模块

```bash
# 更新子模块到父仓库记录的提交
git submodule update

# 更新子模块到远程最新提交
git submodule update --remote

# 更新所有子模块
git submodule update --remote --merge

# 递归更新嵌套子模块
git submodule update --init --recursive
```

### 4.3. 常见问题

```bash
# 子模块目录为空（忘记初始化）
git submodule init && git submodule update

# 删除子模块
git submodule deinit libs/library
git rm libs/library
rm -rf .git/modules/libs/library

# 修改子模块的远程 URL
git config submodule.libs/library.url https://new-url.git
git submodule sync
```

### 4.4. 在子模块中工作

```bash
# 进入子模块目录
cd libs/library

# 在子模块中切换分支、提交
git switch main
git pull

# 回到父仓库，提交子模块引用的更新
cd ../..
git add libs/library
git commit -m "更新子模块到最新版本"
```

## 5. 🤔 如何使用 git subtree 管理依赖？

`git subtree` 是子模块的替代方案，它将外部仓库的内容直接合并到主仓库中，不需要额外的初始化步骤。

```bash
# 添加远程仓库引用
git remote add library https://github.com/user/library.git

# 将外部仓库作为子树添加到指定目录
git subtree add --prefix=libs/library library main --squash
```

### 5.1. 更新子树

```bash
# 拉取外部仓库的更新
git subtree pull --prefix=libs/library library main --squash
```

### 5.2. 推送修改回外部仓库

```bash
# 将子树目录的修改推送回外部仓库
git subtree push --prefix=libs/library library main
```

### 5.3. 子模块 vs 子树

| 特性     | 子模块（Submodule）          | 子树（Subtree）          |
| -------- | ---------------------------- | ------------------------ |
| 仓库结构 | 独立的嵌套仓库               | 合并到主仓库             |
| 克隆     | 需要额外初始化               | 自动包含                 |
| 版本控制 | 记录子仓库的提交引用         | 完整包含文件             |
| 复杂度   | 较高，需要理解子模块工作流   | 较低，使用更直观         |
| 适用场景 | 大型独立库，需要独立版本控制 | 紧耦合的依赖，简化工作流 |
