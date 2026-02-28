# [0042. 主流 Git 平台使用](https://github.com/tnotesjs/TNotes.git-notes/tree/main/notes/0042.%20%E4%B8%BB%E6%B5%81%20Git%20%E5%B9%B3%E5%8F%B0%E4%BD%BF%E7%94%A8)

<!-- region:toc -->

- [1. 🎯 本节内容](#1--本节内容)
- [2. 🫧 评价](#2--评价)
- [3. 🤔 GitHub 的核心功能有哪些？](#3--github-的核心功能有哪些)
  - [3.1. Issues](#31-issues)
  - [3.2. GitHub Actions](#32-github-actions)
  - [3.3. GitHub Pages](#33-github-pages)
- [4. 🤔 GitLab CI/CD 的基础用法是什么？](#4--gitlab-cicd-的基础用法是什么)
- [5. 🤔 其他 Git 平台有哪些？](#5--其他-git-平台有哪些)
  - [5.1. Gitee（码云）](#51-gitee码云)
  - [5.2. Bitbucket](#52-bitbucket)
  - [5.3. 自托管方案](#53-自托管方案)

<!-- endregion:toc -->

## 1. 🎯 本节内容

- GitHub 核心功能（Issues, Actions, Pages）
- GitLab CI/CD 基础
- Gitee / Bitbucket 等其他平台

## 2. 🫧 评价

- todo

## 3. 🤔 GitHub 的核心功能有哪些？

GitHub 是全球最大的代码托管平台，除了基本的 Git 仓库托管外，还提供了丰富的协作和自动化功能。

### 3.1. Issues

Issues 是 GitHub 的问题跟踪系统，用于管理 bug 报告、功能需求和任务：

- 支持标签（Labels）分类，如 `bug`、`enhancement`、`documentation`
- 支持里程碑（Milestones）进行版本规划
- 支持指派（Assignees）分配责任人
- 支持与 Pull Request 关联（在 PR 中使用 `Closes #123` 自动关闭 Issue）

### 3.2. GitHub Actions

GitHub Actions 是内置的 CI/CD 工具，通过 YAML 文件定义工作流：

```yaml
# .github/workflows/ci.yml
name: CI
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: npm install
      - run: npm test
```

### 3.3. GitHub Pages

GitHub Pages 可以直接从仓库部署静态网站：

- 从 `main` 分支或 `gh-pages` 分支发布
- 支持自定义域名
- 自动构建 Jekyll 站点
- 可配合 GitHub Actions 部署其他框架（如 VitePress、Next.js）

## 4. 🤔 GitLab CI/CD 的基础用法是什么？

GitLab 是一个自托管友好的 Git 平台，其 CI/CD 功能通过 `.gitlab-ci.yml` 配置：

```yaml
# .gitlab-ci.yml
stages:
  - build
  - test
  - deploy

build:
  stage: build
  script:
    - npm install
    - npm run build
  artifacts:
    paths:
      - dist/

test:
  stage: test
  script:
    - npm test

deploy:
  stage: deploy
  script:
    - npm run deploy
  only:
    - main
```

GitLab 的特色功能：

- **GitLab Runner**：可以在自己的服务器上运行 CI/CD 任务
- **容器注册表**：内置 Docker 镜像仓库
- **代码审查**：Merge Request（类似 GitHub 的 Pull Request）
- **内置 Wiki**：项目文档管理
- **DevOps 全链路**：从计划、编码到监控的完整工具链

## 5. 🤔 其他 Git 平台有哪些？

### 5.1. Gitee（码云）

国内最大的代码托管平台，特点：

- 服务器在国内，访问速度快
- 提供 Gitee Pages 静态网站托管
- 支持从 GitHub 同步仓库
- 免费支持私有仓库
- 提供企业版，适合国内团队

### 5.2. Bitbucket

Atlassian 旗下的 Git 托管平台：

- 与 Jira、Confluence 等 Atlassian 产品深度集成
- Bitbucket Pipelines 提供 CI/CD 功能
- 免费支持 5 人以内的私有仓库
- 适合已使用 Atlassian 生态的团队

### 5.3. 自托管方案

| 平台      | 特点                          |
| --------- | ----------------------------- |
| GitLab CE | 功能完整，资源占用较大        |
| Gitea     | 轻量级，Go 语言编写，部署简单 |
| Gogs      | 比 Gitea 更轻量，功能较少     |

选择建议：开源项目首选 GitHub；国内团队可选 Gitee；企业内部部署考虑 GitLab CE 或 Gitea。
