# [README.md](./0001.%20修改指定远程仓库的%20url/README.md)<!-- !======> SEPERATOR <====== -->
# [0001. 修改指定远程仓库的 url](https://github.com/Tdahuyou/git/tree/main/0001.%20%E4%BF%AE%E6%94%B9%E6%8C%87%E5%AE%9A%E8%BF%9C%E7%A8%8B%E4%BB%93%E5%BA%93%E7%9A%84%20url)

<!-- region:toc -->
- [1. 📒 使用 `git remote set-url` 命令](#1--使用-git-remote-set-url-命令)
- [2. 📒 直接编辑 `.git/config` 文件](#2--直接编辑-git/config-文件)
<!-- endregion:toc -->


## 1. 📒 使用 `git remote set-url` 命令

要修改 Git 仓库中远程 `origin` 的 URL，你可以使用 `git remote set-url` 命令。

```bas
# 可以先查看当前的远程仓库 URL，然后根据需要进行修改
git remote -v # 这将显示所有远程仓库（包括 origin）的 fetch 和 push URL。

# git 修改远程 origin 的 url 的命令是：
git remote set-url origin 新的URL
# 例如，如果新的远程仓库 URL 是 `https://github.com/username/new-repo.git`，那么命令将是：
git remote set-url origin https://github.com/username/new-repo.git

# 修改完毕之后，可以再次查看远程仓库的 URL，确保修改成功。
git remote -v
```

## 2. 📒 直接编辑 `.git/config` 文件

你也可以直接编辑本地仓库的 `.git/config` 文件来更改 `origin` 的 URL。找到 `[remote "origin"]` 部分，修改 `url` 行的内容。这种方法比较适合那些不熟悉命令行的人，但需要小心处理，以免破坏配置文件。

打开 `.git/config` 文件后，你会看到类似下面的内容：

```ini
[remote "origin"]
    url = 旧的URL
    fetch = +refs/heads/*:refs/remotes/origin/*
```

只需将 `url` 的值更改为新的 URL 即可。







# [README.md](./0002.%20尝试变更到远程仓库时遇到%20Permission%20denied%20(publickey)%20错误/README.md)<!-- !======> SEPERATOR <====== -->
# [0002. 尝试变更到远程仓库时遇到 Permission denied (publickey) 错误](https://github.com/Tdahuyou/git/tree/main/0002.%20%E5%B0%9D%E8%AF%95%E5%8F%98%E6%9B%B4%E5%88%B0%E8%BF%9C%E7%A8%8B%E4%BB%93%E5%BA%93%E6%97%B6%E9%81%87%E5%88%B0%20Permission%20denied%20(publickey)%20%E9%94%99%E8%AF%AF)

<!-- region:toc -->
- [1. 📒 错误示例](#1--错误示例)
- [2. 🤖 尝试将代码推送到 GitHub 时遇到 Permission denied (publickey). 错误，应该如何解决？](#2--尝试将代码推送到-github-时遇到-permission-denied-(publickey)-错误应该如何解决)
<!-- endregion:toc -->

## 1. 📒 错误示例

```bash
$ git push -u origin main
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

## 2. 🤖 尝试将代码推送到 GitHub 时遇到 Permission denied (publickey). 错误，应该如何解决？

当你尝试将代码推送到 GitHub 时遇到 `Permission denied (publickey).` 错误，**这通常意味着你的 SSH 密钥没有被正确配置或者没有被添加到 GitHub 账户中**。以下是一些解决这个问题的步骤：

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





# [README.md](./0003.%20git%20clone%20报%20RPC%20failed%20错误/README.md)<!-- !======> SEPERATOR <====== -->
# [0003. git clone 报 RPC failed 错误](https://github.com/Tdahuyou/git/tree/main/0003.%20git%20clone%20%E6%8A%A5%20RPC%20failed%20%E9%94%99%E8%AF%AF)

<!-- region:toc -->
- [1. 📝 Summary](#1--summary)
- [2. 📒 git clone => ❌ RPC failed](#2--git-clone-=>--rpc-failed)
- [3. 📒 其他手段](#3--其他手段)
  - [3.1. 检查网络连接](#31-检查网络连接)
  - [3.2. 分段克隆](#32-分段克隆)
  - [3.3. 使用 SSH 克隆](#33-使用-ssh-克隆)
  - [3.4. 检查防火墙和代理设置](#34-检查防火墙和代理设置)
  - [3.5. 更新 Git](#35-更新-git)
<!-- endregion:toc -->

## 1. 📝 Summary

- 解决办法：`git config --global http.sslVerify false`

## 2. 📒 git clone => ❌ RPC failed

```bash
# 克隆失败
git clone https://github.com/Tdahuyou/electron.git
# 输出日志：日志提示 RPC failed
# Cloning into 'electron'...
# remote: Enumerating objects: 871, done.
# remote: Counting objects: 100% (871/871), done.
# remote: Compressing objects: 100% (696/696), done.
# error: RPC failed; curl 56 Recv failure: Connection was reset ❌
# error: 6547 bytes of body are still expected
# fetch-pack: unexpected disconnect while reading sideband packet
# fatal: early EOF
# fatal: fetch-pack: invalid index-pack output

# AI 分析：
# 你遇到的错误 `RPC failed; curl 56 Recv failure: Connection was reset` 通常是由于网络连接不稳定或中断导致的。
# 这种情况通常发生在克隆大型仓库时，因为数据传输量较大，网络连接可能会在中途断开。

# 解决方案1：增加 Git 的缓冲区大小
# 你可以通过增加 Git 的缓冲区大小来减少这种问题的发生。使用以下命令来设置更大的缓冲区大小：
git config --global http.postBuffer 524288000
# 这会将缓冲区大小设置为 500MB。你可以根据需要调整这个值。

# 解决方案2：使用浅克隆
# 如果你不需要完整的提交历史记录，可以使用浅克隆来减少下载的数据量：
git clone --depth 1 https://github.com/Tdahuyou/electron.git
# 这样只会克隆最新的提交，而不是整个历史记录。

# 重试
git clone https://github.com/Tdahuyou/electron.git
# Cloning into 'electron'...
# remote: Enumerating objects: 871, done.
# remote: Counting objects: 100% (871/871), done.
# remote: Compressing objects: 100% (696/696), done.
# remote: Total 871 (delta 181), reused 822 (delta 132), pack-reused 0 (from 0)
# Receiving objects: 100% (871/871), 31.91 MiB | 10.49 MiB/s, done.
# Resolving deltas: 100% (181/181), done. ✅
```

## 3. 📒 其他手段

### 3.1. 检查网络连接

确保你的网络连接稳定。如果可能，尝试切换到更稳定的网络环境（例如，从移动数据切换到 Wi-Fi）。

### 3.2. 分段克隆

如果上述方法仍然无效，你可以尝试分段克隆。首先克隆一个较浅的历史记录，然后再逐步获取更多的历史记录。


```bash
# 浅克隆
git clone --depth 1 https://github.com/Tdahuyou/electron.git

# 进入克隆的仓库目录
cd electron

# 获取更多历史记录
git fetch --depth=100
# 你可以逐渐增加深度，直到获取到你需要的所有历史记录。
```

### 3.3. 使用 SSH 克隆

如果 HTTPS 方式仍然有问题，可以尝试使用 SSH 方式克隆仓库。首先确保你在 GitHub 上添加了 SSH 密钥，并且在本地安装了 SSH 密钥。

```bash
# 生成 SSH 密钥（如果还没有生成的话）：
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

# 将公钥添加到 GitHub。
# 登录 GitHub 账户，在设置中添加新的 SSH 密钥。

# 使用 SSH URL 克隆仓库
git clone git@github.com:Tdahuyou/electron.git
```

### 3.4. 检查防火墙和代理设置

确保没有防火墙或代理设置阻止了 Git 的操作。如果使用了代理，确保配置正确：

```bash
git config --global http.proxy http://your_proxy_server:port
git config --global https.proxy http://your_proxy_server:port
```

如果不需要代理，取消代理设置：

```bash
git config --global --unset http.proxy
git config --global --unset https.proxy
```

### 3.5. 更新 Git

确保你使用的是最新版本的 Git。有时旧版本的 Git 可能会有已知的问题，更新到最新版本可以解决这些问题。

```bas
# 在 Windows 上，可以从 Git 官方网站下载最新版本的安装程序
# 在 Linux 或 macOS 上，可以使用包管理器更新 Git
```




# [README.md](./0004.%20处理每次使用%20SSH%20密钥进行身份验证时都需要输入密码短语（passphrase）的问题/README.md)<!-- !======> SEPERATOR <====== -->
# [0004. 处理每次使用 SSH 密钥进行身份验证时都需要输入密码短语（passphrase）的问题](https://github.com/Tdahuyou/git/tree/main/0004.%20%E5%A4%84%E7%90%86%E6%AF%8F%E6%AC%A1%E4%BD%BF%E7%94%A8%20SSH%20%E5%AF%86%E9%92%A5%E8%BF%9B%E8%A1%8C%E8%BA%AB%E4%BB%BD%E9%AA%8C%E8%AF%81%E6%97%B6%E9%83%BD%E9%9C%80%E8%A6%81%E8%BE%93%E5%85%A5%E5%AF%86%E7%A0%81%E7%9F%AD%E8%AF%AD%EF%BC%88passphrase%EF%BC%89%E7%9A%84%E9%97%AE%E9%A2%98)

<!-- region:toc -->
- [1. 📒 问题分析](#1--问题分析)
- [2. 💻 使用 `ssh-agent`](#2--使用-ssh-agent)
- [3. 💻 使用无密码短语的密钥](#3--使用无密码短语的密钥)
<!-- endregion:toc -->

## 1. 📒 问题分析

当你每次使用 SSH 密钥进行身份验证时都需要输入密码短语（passphrase），这是因为你的私钥被保护了，以增加安全性。如果你希望避免每次都输入密码短语，可以考虑以下几种方法：

- 使用 `ssh-agent`
- 使用无密码短语的密钥

请注意，虽然这些方法提高了便利性，但可能会降低一定的安全性。确保你了解这些风险，并根据你的安全需求做出适当的选择。

## 2. 💻 使用 `ssh-agent`

`ssh-agent` 是一个守护进程，用于管理你的 SSH 密钥和密码短语。你可以将密钥添加到 `ssh-agent` 中，并在会话期间自动处理认证过程。

```bash
# 启动 `ssh-agent` 并添加密钥
# 首先，确保 `ssh-agent` 正在运行。你可以在终端中启动它：
eval "$(ssh-agent -s)"

# 然后，将你的私钥添加到 `ssh-agent` 中：

ssh-add -K ~/.ssh/id_rsa
# 如果你使用的是 ed25519 密钥，则为
ssh-add -K ~/.ssh/id_ed25519
# `-K` 参数会将密码短语保存到 macOS 的 Keychain 中，这样在重启后你也不需要重新输入密码短语。

# 自动启动 `ssh-agent`
# 为了确保 `ssh-agent` 在每次打开终端时都能自动启动，你可以将上面的命令添加到你的 shell 配置文件中（例如 `.bashrc`, `.zshrc` 或者 `.profile`）。
```

## 3. 💻 使用无密码短语的密钥

如果你不介意稍微降低安全性，可以选择生成一个新的没有密码短语的 SSH 密钥。这将允许你在没有任何提示的情况下使用该密钥。

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com" -N ""
# -N "" 表示不设置密码短语
```

然后将这个新的公钥添加到你的 GitHub 账户中，并更新本地 Git 配置来使用这个新密钥。



# [README.md](./0005.%20git%20proxy%20配置/README.md)<!-- !======> SEPERATOR <====== -->
# [0005. git proxy 配置](https://github.com/Tdahuyou/git/tree/main/0005.%20git%20proxy%20%E9%85%8D%E7%BD%AE)

<!-- region:toc -->
- [1. 📒 常见的超时问题](#1--常见的超时问题)
- [2. 💻 查看代理配置 => git config --get http.proxy](#2--查看代理配置-=>-git-config---get-httpproxy)
- [3. 💻 设置代理配置 => git config http.proxy 代理地址](#3--设置代理配置-=>-git-config-httpproxy-代理地址)
- [4. 💻 取消代理配置 => git config --global --unset http.proxy](#4--取消代理配置-=>-git-config---global---unset-httpproxy)
- [5. 💻 验证配置 => git config --list](#5--验证配置-=>-git-config---list)
<!-- endregion:toc -->

## 1. 📒 常见的超时问题

```bash
# 在执行某些需要和远程仓库打交道的 git 命令时，可能会出现类似下面这样的超时错误：
unable to access 'https://xxx/': Failed to connect to github.com port 443 after 21106 ms: Could not connect to server

# 这类错误往往可以通过设置代理来解决。
```

## 2. 💻 查看代理配置 => git config --get http.proxy

`git config --get http.proxy` 是一个 Git 命令，用于获取当前配置的 HTTP 代理设置。这个命令会显示 Git 在进行 HTTP 或 HTTPS 通信时使用的代理服务器地址。

- **`git config`**：这是 Git 的配置命令，用于读取和设置 Git 的配置选项。
- **`--get`**：这个选项告诉 Git 获取指定配置项的值。
- **`http.proxy`**：这是要获取的配置项名称，表示 Git 使用的 HTTP 代理服务器地址。

假设你已经设置了 HTTP 代理，运行 `git config --get http.proxy` 可能会返回类似以下的结果：

```bash
http://127.0.0.1:7897
```

这表示 Git 当前配置为通过 `http://127.0.0.1:7897` 这个代理服务器来进行 HTTP 和 HTTPS 通信。

Git 的配置有多个层次，按优先级从高到低排列如下：

1. **本地仓库配置**：位于 `.git/config` 文件中，仅对当前仓库有效。
2. **全局配置**：位于用户主目录下的 `.gitconfig` 文件中，对所有仓库有效。
3. **系统配置**：位于系统级别的配置文件中（例如在 Windows 上通常是 `C:\Program Files\Git\mingw64\etc\gitconfig`），对所有用户和仓库有效。

你可以使用不同的参数来查看不同层次的配置：

```bash
# 查看本地仓库配置
git config --local --get http.proxy
git config --local --get https.proxy

# 查看全局配置
git config --global --get http.proxy
git config --global --get https.proxy

# 查看系统配置
git config --system --get http.proxy
git config --system --get https.proxy
```

## 3. 💻 设置代理配置 => git config http.proxy 代理地址

```bash
# 设置 HTTP 代理
# 如果你想设置设置 HTTP 代理，可以使用以下命令：
$ git config --local http.proxy http://127.0.0.1:7897
$ git config --local https.proxy http://127.0.0.1:7897
$ git config --global http.proxy http://127.0.0.1:7897
$ git config --global https.proxy http://127.0.0.1:7897
$ git config --system http.proxy http://127.0.0.1:7897
$ git config --system https.proxy http://127.0.0.1:7897
```

## 4. 💻 取消代理配置 => git config --global --unset http.proxy

```bash
git config --local --unset http.proxy
git config --local --unset https.proxy
git config --global --unset http.proxy
git config --global --unset https.proxy
git config --system --unset http.proxy
git config --system --unset https.proxy
```

## 5. 💻 验证配置 => git config --list

- 可以通过 `git config --list` 查看所有配置项，确认 `http.proxy` 和 `https.proxy` 是否正确设置。
- 也可以通过 `git config --get http.proxy`、``git config --get http.proxy`` 来查看代理设置。

