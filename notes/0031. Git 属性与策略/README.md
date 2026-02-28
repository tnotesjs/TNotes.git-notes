# [0031. Git 属性与策略](https://github.com/tnotesjs/TNotes.git-notes/tree/main/notes/0031.%20Git%20%E5%B1%9E%E6%80%A7%E4%B8%8E%E7%AD%96%E7%95%A5)

<!-- region:toc -->

- [1. 🎯 本节内容](#1--本节内容)
- [2. 🫧 评价](#2--评价)
- [3. 🤔 如何配置 Git 属性？](#3--如何配置-git-属性)
- [4. 🤔 什么是合并策略与自定义合并驱动？](#4--什么是合并策略与自定义合并驱动)
  - [4.1. 内置合并策略](#41-内置合并策略)
  - [4.2. 自定义合并驱动](#42-自定义合并驱动)
- [5. 🤔 如何在导出仓库时忽略某些文件？](#5--如何在导出仓库时忽略某些文件)

<!-- endregion:toc -->

## 1. 🎯 本节内容

- 配置 Git 属性（`.gitattributes`）
- 合并策略与自定义合并驱动
- 导出仓库时忽略某些文件（`export-ignore`）

## 2. 🫧 评价

- todo

## 3. 🤔 如何配置 Git 属性？

Git 属性通过 `.gitattributes` 文件配置，可以针对特定路径定义 Git 的处理方式。该文件通常放在仓库根目录下，并纳入版本控制。

```gitattributes
# 设置文本文件的换行符处理
*.txt text
*.sh text eol=lf
*.bat text eol=crlf

# 标记二进制文件（不进行文本差异比较）
*.png binary
*.jpg binary
*.pdf binary

# 自定义差异比较方式
*.md diff=markdown

# 设置合并策略
database.xml merge=ours

# 语言统计（用于 GitHub 语言识别）
*.js linguist-detectable
docs/** linguist-documentation
```

常用属性说明：

| 属性       | 说明                             |
| ---------- | -------------------------------- |
| `text`     | 按文本文件处理（自动转换换行符） |
| `binary`   | 按二进制文件处理                 |
| `eol=lf`   | 强制使用 LF 换行符               |
| `eol=crlf` | 强制使用 CRLF 换行符             |
| `diff`     | 指定差异比较的驱动程序           |
| `merge`    | 指定合并策略                     |

## 4. 🤔 什么是合并策略与自定义合并驱动？

Git 在合并时支持多种策略，可以通过 `.gitattributes` 为特定文件指定合并策略：

### 4.1. 内置合并策略

```bash
# 使用指定策略合并
git merge -s recursive feature
git merge -s ours feature
git merge -s octopus branch1 branch2 branch3
```

常见的合并策略：

| 策略                | 说明                                  |
| ------------------- | ------------------------------------- |
| `recursive`（默认） | 递归三方合并，适用于大多数场景        |
| `ours`              | 保留当前分支的版本，忽略对方的修改    |
| `theirs`            | 作为 recursive 的选项，保留对方的版本 |
| `octopus`           | 用于同时合并多个分支                  |

### 4.2. 自定义合并驱动

在 `.gitattributes` 中指定文件的合并驱动：

```gitattributes
# 对数据库配置文件使用 ours 策略（始终保留本地版本）
config/database.yml merge=ours
```

需要先注册合并驱动：

```bash
git config merge.ours.driver true
```

这样在合并时，`config/database.yml` 会始终保留当前分支的版本，避免合并冲突。

## 5. 🤔 如何在导出仓库时忽略某些文件？

使用 `export-ignore` 属性可以在通过 `git archive` 导出仓库时排除特定文件和目录：

```gitattributes
# 导出时忽略以下文件和目录
.gitignore export-ignore
.gitattributes export-ignore
.github/ export-ignore
tests/ export-ignore
docs/ export-ignore
*.test.js export-ignore
.env.example export-ignore
```

使用 `git archive` 创建归档：

```bash
# 创建 tar.gz 归档
git archive --format=tar.gz -o release.tar.gz HEAD

# 创建 zip 归档
git archive --format=zip -o release.zip HEAD

# 导出指定目录
git archive HEAD src/ | tar -x
```

`export-subst` 属性可以在导出时对文件进行变量替换：

```gitattributes
VERSION.txt export-subst
```

在文件中使用占位符：

```
Version: $Format:%H$
Date: $Format:%ci$
```

导出时 Git 会将占位符替换为实际的提交信息。
