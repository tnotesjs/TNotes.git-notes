# [0017. 如何取消 github 上某个 OAuth 应用的访问权限](https://github.com/Tdahuyou/TNotes.git-notes/tree/main/notes/0017.%20%E5%A6%82%E4%BD%95%E5%8F%96%E6%B6%88%20github%20%E4%B8%8A%E6%9F%90%E4%B8%AA%20OAuth%20%E5%BA%94%E7%94%A8%E7%9A%84%E8%AE%BF%E9%97%AE%E6%9D%83%E9%99%90)

<!-- region:toc -->

- [1. 📝 概述](#1--概述)
- [2. 💻 在 Settings 中的 Applications 面板中撤销应用的授权（推荐）](#2--在-settings-中的-applications-面板中撤销应用的授权推荐)
- [3. 💻 使用 API 撤销令牌](#3--使用-api-撤销令牌)

<!-- endregion:toc -->

## 1. 📝 概述

- 要取消 GitHub 上某个 OAuth 应用的访问权限，可通过网页操作或 API 实现。

## 2. 💻 在 Settings 中的 Applications 面板中撤销应用的授权（推荐）

- 登录 GitHub，点击右上角头像 → **Settings**（设置）。
- 左侧菜单选择 **Applications**。
  - ![图 0](https://cdn.jsdelivr.net/gh/Tdahuyou/imgs@main/2025-07-26-00-20-53.png)
- 选择 **Authorized OAuth Apps**（已授权的 OAuth 应用）。
  - ![图 1](https://cdn.jsdelivr.net/gh/Tdahuyou/imgs@main/2025-07-26-00-31-26.png)
- 找到目标应用，点击 **Revoke**（撤销）并确认。
- 取消后应用将立即失去访问权限，且无法再获取你的私有数据。

## 3. 💻 使用 API 撤销令牌

若需以编程方式撤销单个访问令牌（如用户退出应用时），可调用 GitHub API：

```bash
curl -X DELETE \
  -H "Accept: application/vnd.github.v3+json" \
  -u "CLIENT_ID:CLIENT_SECRET" \
  https://api.github.com/applications/CLIENT_ID/token \
  -d '{"access_token":"ACCESS_TOKEN"}'
```

- **参数说明**：
  - `CLIENT_ID`/`CLIENT_SECRET`：OAuth 应用的凭证。
  - `ACCESS_TOKEN`：需撤销的令牌。
- **注意**：此操作仅使当前令牌失效，不影响其他授权。
