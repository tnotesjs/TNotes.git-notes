# [0029. 调试与恢复](https://github.com/tnotesjs/TNotes.git-notes/tree/main/notes/0029.%20%E8%B0%83%E8%AF%95%E4%B8%8E%E6%81%A2%E5%A4%8D)

<!-- region:toc -->

- [1. 🎯 本节内容](#1--本节内容)
- [2. 🫧 评价](#2--评价)
- [3. 🤔 如何使用 git blame 追溯代码行？](#3--如何使用-git-blame-追溯代码行)
- [4. 🤔 如何使用 git reflog 找回丢失的提交？](#4--如何使用-git-reflog-找回丢失的提交)
- [5. 🤔 如何恢复已删除的分支？](#5--如何恢复已删除的分支)

<!-- endregion:toc -->

## 1. 🎯 本节内容

- 使用 `git blame` 追溯代码行
- 找回丢失的提交（`git reflog` 的妙用）
- 恢复已删除的分支

## 2. 🫧 评价

- todo

## 3. 🤔 如何使用 git blame 追溯代码行？

`git blame` 可以逐行显示文件中每一行的最后修改信息，帮助你追溯代码的来源和修改历史：

```bash
# 查看文件每一行的作者和提交信息
git blame file.txt

# 查看指定行范围（第 10 到 20 行）
git blame -L 10,20 file.txt

# 从指定行开始显示 N 行
git blame -L 10,+5 file.txt

# 显示邮箱而非作者名
git blame -e file.txt

# 忽略空白字符的变更
git blame -w file.txt

# 检测行的移动和复制来源
git blame -C file.txt
```

输出格式示例：

```
abc1234 (Alice 2024-03-15 10:30:00 +0800  5) function init() {
def5678 (Bob   2024-04-02 14:20:00 +0800  6)   console.log("hello");
abc1234 (Alice 2024-03-15 10:30:00 +0800  7) }
```

每行前面显示提交哈希、作者、日期和行号，方便定位具体是谁在什么时候修改了某一行代码。

## 4. 🤔 如何使用 git reflog 找回丢失的提交？

`git reflog` 记录了 HEAD 和分支引用的每一次变动，即使提交被重置或分支被删除，也能通过 reflog 找回：

```bash
# 查看 HEAD 的引用日志
git reflog

# 查看指定分支的引用日志
git reflog show feature

# 查看带时间的引用日志
git reflog --date=relative
```

输出示例：

```
abc1234 HEAD@{0}: commit: 最新提交
def5678 HEAD@{1}: rebase: 变基操作
ghi9012 HEAD@{2}: reset: 重置到之前的提交
jkl3456 HEAD@{3}: commit: 被重置掉的提交
```

找回丢失的提交：

```bash
# 查看丢失提交的内容
git show jkl3456

# 方法一：将 HEAD 重置到丢失的提交
git reset --hard jkl3456

# 方法二：将丢失的提交挑选到当前分支
git cherry-pick jkl3456

# 方法三：从丢失的提交创建新分支
git branch recovered jkl3456
```

reflog 默认保留 90 天的记录，过期后会被垃圾回收清理。

## 5. 🤔 如何恢复已删除的分支？

不小心删除了分支？只要 reflog 中还有记录，就可以恢复：

```bash
# 1. 查看 reflog 找到分支被删除前的最后一次提交
git reflog

# 2. 使用找到的哈希值重新创建分支
git branch feature abc1234

# 3. 切换到恢复的分支验证
git switch feature
```

也可以通过搜索 reflog 中的分支名来定位：

```bash
# 搜索包含分支名的 reflog 记录
git reflog | grep feature

# 或者查看所有分支的 reflog
git reflog show --all
```

预防措施：

- 在删除分支前确认分支已合并（使用 `git branch -d` 而非 `-D`）
- 定期将重要分支推送到远程仓库作为备份
- 如果分支已推送到远程，可以直接从远程重新检出
