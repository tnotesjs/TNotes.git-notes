# [0036. 常见工作流实战](https://github.com/tnotesjs/TNotes.git-notes/tree/main/notes/0036.%20%E5%B8%B8%E8%A7%81%E5%B7%A5%E4%BD%9C%E6%B5%81%E5%AE%9E%E6%88%98)

<!-- region:toc -->

- [1. 🎯 本节内容](#1--本节内容)
- [2. 🫧 评价](#2--评价)
- [3. 🤔 个人项目的开发流程是怎样的？](#3--个人项目的开发流程是怎样的)
  - [3.1. 基本流程](#31-基本流程)
  - [3.2. 使用分支管理功能](#32-使用分支管理功能)
  - [3.3. 版本发布](#33-版本发布)
- [4. 🤔 团队特性开发流程是怎样的？](#4--团队特性开发流程是怎样的)
  - [4.1. 完整流程](#41-完整流程)
  - [4.2. 提交消息规范](#42-提交消息规范)
- [5. 🤔 开源项目的贡献流程是怎样的？](#5--开源项目的贡献流程是怎样的)
  - [5.1. 准备工作](#51-准备工作)
  - [5.2. 开发与提交](#52-开发与提交)
  - [5.3. 提交 PR](#53-提交-pr)
  - [5.4. 处理审查反馈](#54-处理审查反馈)

<!-- endregion:toc -->

## 1. 🎯 本节内容

- 个人项目开发流程
- 团队特性开发流程
- 开源项目贡献流程

## 2. 🫧 评价

- todo

## 3. 🤔 个人项目的开发流程是怎样的？

个人项目的 Git 工作流相对简单，核心目标是记录开发历史和方便回滚。

### 3.1. 基本流程

```bash
# 1. 初始化项目
git init my-project
cd my-project

# 2. 日常开发循环
git add .
git commit -m "完成首页布局"

# 3. 关联远程仓库
git remote add origin https://github.com/user/my-project.git
git push -u origin main
```

### 3.2. 使用分支管理功能

即使是个人项目，也建议使用分支来开发新功能：

```bash
# 创建功能分支
git switch -c feature/user-auth

# 开发完成后合并
git switch main
git merge feature/user-auth
git branch -d feature/user-auth

# 推送到远程
git push
```

### 3.3. 版本发布

```bash
# 使用标签标记版本
git tag -a v1.0.0 -m "第一个正式版本"
git push origin v1.0.0
```

个人项目的关键原则：频繁提交，每个提交只做一件事，写清晰的提交消息。

## 4. 🤔 团队特性开发流程是怎样的？

团队协作中，功能开发的典型流程基于 Feature Branch 工作流：

### 4.1. 完整流程

```bash
# 1. 同步最新代码
git switch main
git pull origin main

# 2. 创建功能分支（以 Issue 编号命名）
git switch -c feature/123-user-login

# 3. 开发并频繁提交
git add .
git commit -m "feat: 添加登录表单组件"
git commit -m "feat: 实现登录接口调用"
git commit -m "test: 添加登录功能单元测试"

# 4. 保持与主分支同步
git fetch origin
git rebase origin/main

# 5. 推送功能分支
git push -u origin feature/123-user-login

# 6. 在平台上创建 Pull Request

# 7. 代码审查通过后合并

# 8. 清理分支
git switch main
git pull
git branch -d feature/123-user-login
```

### 4.2. 提交消息规范

团队通常采用 Conventional Commits 规范：

| 类型       | 说明                   |
| ---------- | ---------------------- |
| `feat`     | 新功能                 |
| `fix`      | 修复 bug               |
| `docs`     | 文档更新               |
| `style`    | 代码格式（不影响逻辑） |
| `refactor` | 重构                   |
| `test`     | 测试相关               |
| `chore`    | 构建/工具/依赖更新     |

## 5. 🤔 开源项目的贡献流程是怎样的？

向开源项目贡献代码的标准流程：

### 5.1. 准备工作

```bash
# 1. Fork 项目到自己的账户（在 GitHub 上操作）

# 2. 克隆自己的 Fork
git clone https://github.com/your-name/project.git
cd project

# 3. 添加上游仓库
git remote add upstream https://github.com/original/project.git
```

### 5.2. 开发与提交

```bash
# 4. 同步上游最新代码
git fetch upstream
git rebase upstream/main

# 5. 创建主题分支
git switch -c fix/typo-in-readme

# 6. 修改代码并提交
git add .
git commit -m "docs: 修复 README 中的拼写错误"

# 7. 推送到自己的 Fork
git push origin fix/typo-in-readme
```

### 5.3. 提交 PR

1. 在 GitHub 上访问原始仓库
2. 点击 "Compare & pull request"
3. 填写清晰的 PR 描述
4. 等待代码审查和 CI 检查

### 5.4. 处理审查反馈

```bash
# 根据反馈修改代码
git add .
git commit -m "fix: 根据审查意见修正格式"
git push origin fix/typo-in-readme
```

贡献时需注意：阅读项目的 CONTRIBUTING.md，遵循项目的代码风格和提交规范，保持 PR 的范围尽量小且专注。
