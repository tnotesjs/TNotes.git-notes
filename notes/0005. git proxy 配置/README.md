# [0005. git proxy 配置](https://github.com/tnotesjs/TNotes.git-notes/tree/main/notes/0005.%20git%20proxy%20%E9%85%8D%E7%BD%AE)

<!-- region:toc -->

- [1. 📒 常见的超时报错 443 日志](#1--常见的超时报错-443-日志)
- [2. 💻 查看代理配置 => git config --get http.proxy](#2--查看代理配置--git-config---get-httpproxy)
- [3. 💻 设置代理配置 => git config http.proxy 代理地址](#3--设置代理配置--git-config-httpproxy-代理地址)
- [4. 💻 取消代理配置 => git config --global --unset http.proxy](#4--取消代理配置--git-config---global---unset-httpproxy)
- [5. 💻 验证配置 => git config --list](#5--验证配置--git-config---list)

<!-- endregion:toc -->

## 1. 📒 常见的超时报错 443 日志

```shell
# 在执行某些需要和远程仓库打交道的 git 命令时，可能会出现类似下面这样的超时错误：
unable to access 'https://xxx/': Failed to connect to github.com port 443 after 21106 ms: Could not connect to server

# 这类错误往往可以通过设置代理来解决。
```

## 2. 💻 查看代理配置 => git config --get http.proxy

`git config --get http.proxy` 是一个 Git 命令，用于获取当前配置的 HTTP 代理设置。这个命令会显示 Git 在进行 HTTP 或 HTTPS 通信时使用的代理服务器地址。

- `git config`：这是 Git 的配置命令，用于读取和设置 Git 的配置选项。
- `--get`：这个选项告诉 Git 获取指定配置项的值。
- `http.proxy`：这是要获取的配置项名称，表示 Git 使用的 HTTP 代理服务器地址。

假设你已经设置了 HTTP 代理，运行 `git config --get http.proxy` 可能会返回类似以下的结果：

```bash
http://127.0.0.1:7897
```

这表示 Git 当前配置为通过 `http://127.0.0.1:7897` 这个代理服务器来进行 HTTP 和 HTTPS 通信。

Git 的配置有多个层次，按优先级从高到低排列如下：

1. 本地仓库配置：位于 `.git/config` 文件中，仅对当前仓库有效。
2. 全局配置：位于用户主目录下的 `.gitconfig` 文件中，对所有仓库有效。
3. 系统配置：位于系统级别的配置文件中（例如在 Windows 上通常是 `C:\Program Files\Git\mingw64\etc\gitconfig`），对所有用户和仓库有效。

你可以使用不同的参数来查看不同层次的配置：

```shell
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

```shell
# 设置 HTTP 代理
# 如果你想设置设置 HTTP 代理，可以使用以下命令：
git config --local http.proxy http://127.0.0.1:7897
git config --local https.proxy http://127.0.0.1:7897
git config --global http.proxy http://127.0.0.1:7897
git config --global https.proxy http://127.0.0.1:7897
git config --system http.proxy http://127.0.0.1:7897
git config --system https.proxy http://127.0.0.1:7897
```

## 4. 💻 取消代理配置 => git config --global --unset http.proxy

```shell
git config --local --unset http.proxy
git config --local --unset https.proxy
git config --global --unset http.proxy
git config --global --unset https.proxy
git config --system --unset http.proxy
git config --system --unset https.proxy
```

## 5. 💻 验证配置 => git config --list

- 可以通过 `git config --list` 查看所有配置项，确认 `http.proxy` 和 `https.proxy` 是否正确设置。
- 也可以通过 `git config --get http.proxy`、`git config --get http.proxy` 来查看代理设置。
