# [0003. git clone 报 RPC failed 错误](https://github.com/tnotesjs/TNotes.git-notes/tree/main/notes/0003.%20git%20clone%20%E6%8A%A5%20RPC%20failed%20%E9%94%99%E8%AF%AF)

<!-- region:toc -->

- [1. git clone => ❌ RPC failed](#1-git-clone---rpc-failed)
- [2. 其他解决方案](#2-其他解决方案)
  - [2.1. 检查网络连接](#21-检查网络连接)
  - [2.2. 分段克隆](#22-分段克隆)
  - [2.3. 使用 SSH 克隆](#23-使用-ssh-克隆)
  - [2.4. 检查防火墙和代理设置](#24-检查防火墙和代理设置)
  - [2.5. 更新 Git](#25-更新-git)

<!-- endregion:toc -->

- 解决办法：`git config --global http.sslVerify false`

## 1. git clone => ❌ RPC failed

```shell
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

## 2. 其他解决方案

### 2.1. 检查网络连接

确保你的网络连接稳定。如果可能，尝试切换到更稳定的网络环境（例如，从移动数据切换到 Wi-Fi）。

### 2.2. 分段克隆

如果上述方法仍然无效，你可以尝试分段克隆。首先克隆一个较浅的历史记录，然后再逐步获取更多的历史记录。

```sh
# 浅克隆
git clone --depth 1 https://github.com/Tdahuyou/electron.git

# 进入克隆的仓库目录
cd electron

# 获取更多历史记录
git fetch --depth=100
# 你可以逐渐增加深度，直到获取到你需要的所有历史记录。
```

### 2.3. 使用 SSH 克隆

如果 HTTPS 方式仍然有问题，可以尝试使用 SSH 方式克隆仓库。首先确保你在 GitHub 上添加了 SSH 密钥，并且在本地安装了 SSH 密钥。

```sh
# 生成 SSH 密钥（如果还没有生成的话）：
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

# 将公钥添加到 GitHub。
# 登录 GitHub 账户，在设置中添加新的 SSH 密钥。

# 使用 SSH URL 克隆仓库
git clone git@github.com:Tdahuyou/electron.git
```

### 2.4. 检查防火墙和代理设置

确保没有防火墙或代理设置阻止了 Git 的操作。如果使用了代理，确保配置正确：

```sh
git config --global http.proxy http://your_proxy_server:port
git config --global https.proxy http://your_proxy_server:port
```

如果不需要代理，取消代理设置：

```sh
git config --global --unset http.proxy
git config --global --unset https.proxy
```

### 2.5. 更新 Git

确保你使用的是最新版本的 Git。有时旧版本的 Git 可能会有已知的问题，更新到最新版本可以解决这些问题。

```sh
# 在 Windows 上，可以从 Git 官方网站下载最新版本的安装程序
# 在 Linux 或 macOS 上，可以使用包管理器更新 Git
```
