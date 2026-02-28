# [0002. 尝试变更到远程仓库时遇到 Permission denied (publickey) 错误](<https://github.com/tnotesjs/TNotes.git-notes/tree/main/notes/0002.%20%E5%B0%9D%E8%AF%95%E5%8F%98%E6%9B%B4%E5%88%B0%E8%BF%9C%E7%A8%8B%E4%BB%93%E5%BA%93%E6%97%B6%E9%81%87%E5%88%B0%20Permission%20denied%20(publickey)%20%E9%94%99%E8%AF%AF>)

<!-- region:toc -->

- [1. 💻 尝试将代码推送到 GitHub 时遇到 Permission denied (publickey). 错误的解决流程](#1--尝试将代码推送到-github-时遇到-permission-denied-publickey-错误的解决流程)

<!-- endregion:toc -->

## 1. 💻 尝试将代码推送到 GitHub 时遇到 Permission denied (publickey). 错误的解决流程

```bash
# 错误示例
$ git push -u origin main
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

当你尝试将代码推送到 GitHub 时遇到 `Permission denied (publickey).` 错误，这通常意味着你的 SSH 密钥没有被正确配置或者没有被添加到 GitHub 账户中。以下是一些解决这个问题的步骤：

```bash
# 检查是否已经生成了 SSH 密钥
# 打开终端并输入以下命令来查看现有的 SSH 密钥。
ls -al ~/.ssh
```

```bash
# 如果没有 SSH 密钥，生成一个新的
# 如果你没有看到任何 `.pub` 文件（例如 `id_rsa.pub` 或 `id_ed25519.pub`），你需要生成一个新的 SSH 密钥对。
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
# 按照提示操作，可以选择保存位置和设置密码。
```

```bash
# 将 SSH 公钥添加到 GitHub
# 将新生成的公钥内容复制到剪贴板。
cat ~/.ssh/id_rsa.pub
# 或者如果你使用的是 ed25519
cat ~/.ssh/id_ed25519.pub
# 登录到你的 GitHub 账户，进入 Settings -> SSH and GPG keys 页面，点击 New SSH key 按钮，粘贴你的公钥，并保存。
```

```bash
# 测试 SSH 连接
# 在终端中运行以下命令来测试与 GitHub 的连接。
ssh -T git@github.com
# 如果一切正常，你会看到一条欢迎信息，说明你已经成功通过 SSH 认证。
```

```bash
# 确保远程 URL 正确
# 确认你的仓库远程 URL 是正确的。你可以使用以下命令查看当前的远程 URL。
git remote -v
# 如果 URL 不正确，可以使用以下命令修改它。
git remote set-url origin git@github.com:Tdahuyou/algorithm_.git
```

```bash
# 再次尝试推送
# 当所有设置都完成后，再次尝试推送代码。
git push -u origin main
```

```bash
# 如果上述步骤之后问题仍然存在，请确保你的 SSH 代理正在运行并且你的密钥已经被添加到 SSH 代理中。你可以通过以下命令来启动 SSH 代理并添加密钥。
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
# 或者
ssh-add ~/.ssh/id_ed25519
# 如果还是无法解决问题，可能需要检查是否有其他安全软件或网络设置阻止了 SSH 连接。
```
