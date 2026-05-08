# Windows 上安装 Zero CLI 并配置 DeepSeek 官方 API（零基础）

面向：**从未用过命令行**、**电脑里还没有 Node.js / npm / Bun** 的 Windows 用户。按顺序做即可；遇到报错先看文末「常见问题」。

---

## 你会得到什么

- 在终端里运行的 **Zero CLI**（命令名：`zero`）
- 使用 **DeepSeek 官方**接口（`https://api.deepseek.com/anthropic`）和你的 **API Key** 对话、写代码

说明：本教程用 **Node.js 自带的 npm** 安装 Zero CLI，无需单独安装 Bun。

---

## 第一步：安装 Node.js（自带 npm）

Zero CLI 需要 **Node.js 18 或更高版本**。从官网安装即可，同时会装上 **npm**（用来安装命令行工具）。

1. 用浏览器打开：**https://nodejs.org/**
2. 下载 **LTS（长期支持版）** 的 Windows 安装包（`.msi`）。
3. 双击安装，一路 **Next**。建议勾选安装向导里类似 **「Automatically install the necessary tools」** 的选项（若有）；其它保持默认即可。
4. 安装结束后 **重启电脑一次**（或至少注销再登录），避免终端读不到新的环境变量。

---

## 第二步：打开终端（命令行窗口）

下面任选一种方式。

### 方法 A：Windows 11「终端」

1. 按键盘 **Win** 键（窗口键）。
2. 输入 **终端** 或 **Terminal**。
3. 打开 **终端** 或 **Windows Terminal**。

### 方法 B：PowerShell

1. 按 **Win**。
2. 输入 **PowerShell**。
3. 打开 **Windows PowerShell**（或 **终端** 里的 PowerShell 标签页）。

### 方法 C：命令提示符（cmd）

1. 按 **Win + R**。
2. 输入 `cmd`，回车。

**提示：** 后文统称这类窗口为「终端」。安装 Zero CLI **不需要**管理员权限，用普通方式打开即可；除非你很清楚自己在做什么，否则不要习惯性使用「以管理员身份运行」。

---

## 第三步：确认 Node 和 npm 已就绪

在终端里 **一行一行** 输入下面命令，每行后面按 **回车**：

```text
node -v
```

应看到类似 `v22.x.x` 或 `v20.x.x`（只要是 **18 及以上**即可）。

```text
npm -v
```

应看到类似 `10.x.x` 的版本号。

若提示「不是内部或外部命令」，说明 **PATH 未生效**：请先 **关掉所有终端窗口**，再重新打开终端试一次；仍不行则重启电脑后再试。

---

## 第四步：全局安装 Zero CLI

在终端执行：

```bash
npm install -g @paean-ai/zero-cli
```

等待下载结束。若最后一屏没有红色报错，通常即成功。

验证安装：

```bash
zero --version
```

能显示版本号即可。

---

## 第五步：申请 DeepSeek 官方 API Key

1. 浏览器打开：**https://platform.deepseek.com/api_keys**
2. 登录 DeepSeek 开放平台账号。
3. 创建 **API Key**，复制保存到记事本（**不要**发给他人或截图发到公开网络）。

密钥页面（官方）：https://platform.deepseek.com/api_keys

---

## 第六步：把 DeepSeek 配置进 Zero CLI

在终端执行，把命令里的 `你的密钥粘贴到这里` **整个占位符**换成你上一步复制的 Key（通常 **不需要**加引号；请勿把密钥发到聊天、截图或公开仓库）：

```bash
zero provider set deepseek --api-key 你的密钥粘贴到这里
```

成功后会写入本机配置与缓存。

可选：查看当前提供商状态：

```bash
zero provider
zero provider status --json
```

说明：

- 配置与缓存一般在用户目录下的 **`.zero`** 文件夹（例如 `C:\Users\你的用户名\.zero`）。无需手动编辑，除非你熟悉 JSON 配置。
- 若你之前用过 **Paean 账号登录**，之后又配置了 DeepSeek，以 **`zero provider`** 显示的当前提供商为准；需要切回其它登录方式时可查阅本仓库根目录 **`README.md`** 里的 **`zero provider clear`** 说明。

---

## 第七步：启动 Zero CLI

在终端进入你希望工作的文件夹（示例：用户文档下的项目目录）。

若终端标题显示 **PowerShell**，使用：

```powershell
cd $env:USERPROFILE\Documents
```

若是 **命令提示符（cmd）**，使用：

```bat
cd %USERPROFILE%\Documents
```

然后启动：

```bash
zero
```

首次使用若出现权限、网络或模型相关提示，按屏幕说明操作即可。

常用自检命令：

```bash
zero provider list
zero provider use deepseek
```

（`use` 用于在 **已经 `set` 过**、缓存里已有密钥时快速切换，无需再次输入 Key。）

---

## 切换模型档位（可选）

Zero 用 **pro / flash / lite**（或兼容写法 **opus / sonnet / haiku**）对应不同模型。DeepSeek 官方预设默认值以 **`flash`** 为主力模型；具体映射见本仓库根目录 **`README.md`** 中与 DeepSeek 相关的说明（或与 npm 包文档保持一致）。

在交互界面里通常可用内置命令或设置切换档位；脚本或非交互模式请查阅 **`README.md`** 与 **`zero --help`**。

---

## 常见问题

### 1. 输入 `zero` 提示「不是内部或外部命令」

npm 全局命令目录可能不在 PATH。可先尝试：

```bash
npm bin -g
```

若该命令不可用，可用：

```bash
npm config get prefix
```

全局可执行文件通常在 `<prefix>\` 或 `<prefix>\bin`，以及 Windows 下常见的 `%AppData%\npm`。把相应目录加入 Windows **环境变量 → Path**。修改后 **重新打开终端**。

也可临时用完整路径运行（示例，按你机器实际路径调整）：

```bash
"%AppData%\npm\zero.cmd" --version
```

### 2. `npm install -g` 报权限错误

不要用管理员强行覆盖目录权限。优先检查是否装在公司策略限制的目录；可配置 npm 全局前缀到用户目录（需一定动手能力），或咨询本单位 IT。

### 3. 公司网络 / 代理导致 npm 无法下载

需要在系统或 npm 上配置 HTTP 代理；可向网络管理员索要代理地址后，搜索「npm 配置 proxy」按文档设置。

### 4. DeepSeek 请求失败

- 核对 Key 是否复制完整（首尾无多余空格，整条密钥一行粘贴）。
- 核对本机时间是否正确。
- 稍后重试；若持续失败，到 DeepSeek 控制台查看用量与风控提示。

### 5. 想用环境变量代替命令行传 Key（脚本 / CI）

可读本仓库 **`README.md`** 中关于 **`ZERO_PROVIDER_API_KEY`** 的说明；修改环境变量后需重新启动终端或 Zero 进程。

---

## 卸载（可选）

若不再需要全局命令：

```bash
npm uninstall -g @paean-ai/zero-cli
```

用户配置仍在 `.zero` 目录；如需一并删除，可在文件资源管理器中删除该文件夹（请先退出 Zero CLI）。

---

## 延伸阅读

- 英文总览与提供商列表：**本仓库根目录 [`README.md`](../README.md)**
- 产品与品牌细则（上游）：[zero-cli `docs/zero-cli-spec.md`](https://github.com/a8e-ai/zero-cli/blob/main/docs/zero-cli-spec.md)
