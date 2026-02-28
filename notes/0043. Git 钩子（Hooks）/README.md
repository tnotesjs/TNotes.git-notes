# [0043. Git 钩子（Hooks）](https://github.com/tnotesjs/TNotes.git-notes/tree/main/notes/0043.%20Git%20%E9%92%A9%E5%AD%90%EF%BC%88Hooks%EF%BC%89)

<!-- region:toc -->

- [1. 🎯 本节内容](#1--本节内容)
- [2. 🫧 评价](#2--评价)
- [3. 🤔 常用的客户端钩子有哪些？](#3--常用的客户端钩子有哪些)
  - [3.1. pre-commit](#31-pre-commit)
  - [3.2. commit-msg](#32-commit-msg)
  - [3.3. 其他常用客户端钩子](#33-其他常用客户端钩子)
- [4. 🤔 常用的服务端钩子有哪些？](#4--常用的服务端钩子有哪些)
  - [4.1. pre-receive](#41-pre-receive)
  - [4.2. post-receive](#42-post-receive)
  - [4.3. update](#43-update)
- [5. 🤔 如何利用钩子实现自定义工作流与自动化？](#5--如何利用钩子实现自定义工作流与自动化)
  - [5.1. 使用 Husky 管理钩子](#51-使用-husky-管理钩子)
  - [5.2. 常见的自动化场景](#52-常见的自动化场景)
  - [5.3. 跳过钩子](#53-跳过钩子)

<!-- endregion:toc -->

## 1. 🎯 本节内容

- 客户端钩子（pre-commit, commit-msg）
- 服务端钩子（pre-receive, post-receive）
- 自定义工作流与自动化

## 2. 🫧 评价

- todo

## 3. 🤔 常用的客户端钩子有哪些？

客户端钩子在开发者本地触发，用于在特定 Git 操作前后执行自定义脚本。钩子脚本位于 `.git/hooks/` 目录下。

### 3.1. pre-commit

在执行 `git commit` 之前触发，常用于代码检查：

```bash
#!/bin/sh
# .git/hooks/pre-commit

# 运行代码格式检查
npx lint-staged

# 如果脚本返回非零退出码，提交将被中止
```

### 3.2. commit-msg

在提交消息编辑完成后触发，可用于验证提交消息格式：

```bash
#!/bin/sh
# .git/hooks/commit-msg

commit_msg=$(cat "$1")
pattern="^(feat|fix|docs|style|refactor|test|chore): .+"

if ! echo "$commit_msg" | grep -qE "$pattern"; then
    echo "提交消息不符合规范！格式：type: description"
    exit 1
fi
```

### 3.3. 其他常用客户端钩子

| 钩子                 | 触发时机       | 常见用途             |
| -------------------- | -------------- | -------------------- |
| `pre-commit`         | 提交前         | 代码检查、格式化     |
| `prepare-commit-msg` | 编辑提交消息前 | 自动生成消息模板     |
| `commit-msg`         | 提交消息写入后 | 验证消息格式         |
| `post-commit`        | 提交完成后     | 通知、日志记录       |
| `pre-push`           | 推送前         | 运行测试             |
| `pre-rebase`         | 变基前         | 阻止对已推送分支变基 |

## 4. 🤔 常用的服务端钩子有哪些？

服务端钩子在远程仓库接收推送时触发，可以用于强制执行项目策略：

### 4.1. pre-receive

在接收推送的所有引用之前触发，可以用于拒绝不符合规则的推送：

```bash
#!/bin/sh
# hooks/pre-receive

while read oldrev newrev refname; do
    # 禁止向 main 分支强制推送
    if [ "$refname" = "refs/heads/main" ]; then
        forced=$(git rev-list "$newrev".."$oldrev" | wc -l)
        if [ "$forced" -gt 0 ]; then
            echo "禁止向 main 分支强制推送！"
            exit 1
        fi
    fi
done
```

### 4.2. post-receive

在所有引用更新完成后触发，常用于部署和通知：

```bash
#!/bin/sh
# hooks/post-receive

while read oldrev newrev refname; do
    if [ "$refname" = "refs/heads/main" ]; then
        echo "main 分支已更新，正在部署..."
        # 触发部署脚本
        /path/to/deploy.sh
    fi
done
```

### 4.3. update

与 `pre-receive` 类似，但针对每个引用单独触发，可以细粒度地控制每个分支的更新策略。

## 5. 🤔 如何利用钩子实现自定义工作流与自动化？

### 5.1. 使用 Husky 管理钩子

手动管理 `.git/hooks/` 中的脚本不方便团队共享。推荐使用 Husky 等工具：

```bash
# 安装 Husky
npm install husky -D

# 初始化
npx husky init
```

在 `.husky/pre-commit` 中配置：

```bash
npx lint-staged
```

在 `package.json` 中配置 `lint-staged`：

```json
{
  "lint-staged": {
    "*.{js,ts}": ["eslint --fix", "prettier --write"],
    "*.{css,scss}": ["prettier --write"]
  }
}
```

### 5.2. 常见的自动化场景

- 提交前：代码格式化、Lint 检查、单元测试
- 提交消息：验证 Conventional Commits 格式
- 推送前：运行完整测试套件
- 推送后：触发 CI/CD 流水线、发送通知

### 5.3. 跳过钩子

在特殊情况下可以跳过钩子执行：

```bash
# 跳过 pre-commit 和 commit-msg 钩子
git commit --no-verify -m "紧急修复"

# 跳过 pre-push 钩子
git push --no-verify
```
