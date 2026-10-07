---
title: 前端环境安装
---

本文涵盖 NVM、Node、NRM、Yarn、pnpm 的安装与配置，Windows 与 Mac 的差异已分别说明，可按需查阅对应小节。

## 一、NVM 安装与配置

NVM（Node Version Manager）用于在同一台机器上安装和切换多个 Node 版本。Windows 与 Mac/Linux 使用的是两个不同的项目，命令基本一致，但安装方式不同，请按自己的系统选择。

### Windows（nvm-windows）

1、下载安装包

前往官方发布页下载 `nvm-setup.exe`：[nvm-windows Releases](https://github.com/coreybutler/nvm-windows/releases)。

:::tip 提示
安装 nvm-windows 前，请先卸载系统中已有的 Node，避免版本管理冲突。
:::

2、安装

双击运行安装包，依次选择 nvm 的安装目录和 Node 的存放目录，两个路径都不要包含中文和空格。

3、验证安装

安装完成后重新打开命令行窗口，执行：

```shell lineNumbers
nvm version
```

能正常输出版本号即表示安装成功。

### Mac / Linux（nvm-sh）

1、执行安装脚本

版本号以[官方仓库](https://github.com/nvm-sh/nvm)为准，当前最新为 `v0.40.8`：

```shell lineNumbers
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.8/install.sh | bash
```

2、配置环境变量

安装脚本一般会自动写入配置。若安装后提示 `command not found: nvm`，请手动将以下内容加入 `~/.zshrc`（zsh）或 `~/.bashrc`（bash）：

```shell lineNumbers
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

保存后执行以下命令使配置生效：

```shell lineNumbers
source ~/.zshrc
```

3、验证安装

```shell lineNumbers
nvm --version
```

### 配置国内镜像（可选）

用于加速 Node 与 npm 的下载。

Windows：打开 nvm 安装目录下的 `settings.txt`，追加以下两行：

```shell lineNumbers
node_mirror: https://npmmirror.com/mirrors/node/
npm_mirror: https://npmmirror.com/mirrors/npm/
```

Mac / Linux：在 `~/.zshrc` 或 `~/.bashrc` 中追加：

```shell lineNumbers
export NVM_NODEJS_ORG_MIRROR=https://npmmirror.com/mirrors/node/
```

### 常用命令

```shell
nvm ls                 # 查看已安装的 Node 版本
nvm ls-remote --lts    # 查看可安装的远程 LTS 版本
nvm install 24         # 安装指定大版本
nvm use 24             # 切换到指定版本
nvm alias default 24   # 设置默认版本（Mac / Linux）
nvm uninstall 24       # 卸载指定版本
```

:::tip 提示
nvm-windows 若无法识别 `nvm install 24` 这类简写，请先用 `nvm list available` 查看完整版本号，再执行如 `nvm install 24.21.0`。
:::

## 二、Node 安装

推荐使用上一步安装好的 nvm 来安装 Node，方便后续多版本切换。

### 使用 NVM 安装（推荐）

1、安装并切换到目标版本

以 Node 24（当前 LTS）为例：

```shell lineNumbers
nvm install 24
nvm use 24
```

Mac / Linux 也可以直接安装最新的 LTS 版本：

```shell lineNumbers
nvm install --lts
```

2、验证安装

```shell lineNumbers
node -v
npm -v
```

Node 会自带 npm 包管理器，两条命令都能输出版本号即表示安装成功。

:::info 版本选择
Node 的偶数版本（如 22、24）为 LTS 长期支持版，维护周期约 30 个月，推荐日常使用；奇数版本为尝鲜版，维护周期仅半年。每 12 个月会轮换出新的 LTS，具体以 [nodejs.org](https://nodejs.org) 首页标注为准。
:::

### 直接安装（不使用 NVM）

前往官网下载对应系统的安装包：[nodejs.org](https://nodejs.org)，推荐选择 LTS 版本。

- **Windows**：下载 `.msi` 安装包，双击后一路点击 `Next` 即可。
- **Mac**：下载 `.pkg` 安装包双击安装，或使用 Homebrew 执行 `brew install node`。

### 配置 npm 国内镜像

安装完成后，将 npm 源切换为国内镜像可显著提升下载速度：

```shell lineNumbers
npm config set registry https://registry.npmmirror.com
```

查看当前使用的镜像源：

```shell lineNumbers
npm config get registry
```

## 三、NRM 镜像源管理

NRM（NPM Registry Manager）是一个 npm 镜像源管理工具，可以方便地查看、切换和测速各个源，无需手动修改 registry 配置。

1、安装

```shell lineNumbers
npm install -g nrm
```

2、验证安装

```shell lineNumbers
nrm -V
```

3、查看所有镜像源

带 `*` 号的为当前正在使用的源：

```shell lineNumbers
nrm ls
```

4、切换镜像源

```shell lineNumbers
nrm use taobao
```

`taobao` 源现已指向 `registry.npmmirror.com`；如需切回官方源，执行 `nrm use npm`。

:::warning 注意
发布 npm 包前请先切回官方源（`nrm use npm`），否则会因镜像源只读导致发布失败。
:::

5、测试各源响应速度

```shell lineNumbers
nrm test
```

### 常用命令

```shell
nrm current            # 查看当前使用的源
nrm add <name> <url>   # 添加自定义源
nrm del <name>         # 删除自定义源
```

## 四、Yarn

Yarn 是主流的 npm 替代方案之一，具备更快的安装速度和可靠的依赖锁定机制。

:::info 版本说明
Yarn 有两个大版本：**1.x（Classic）** 通过 npm 全局安装，**4.x（Berry）** 为官方当前维护的版本，通过 Corepack 启用。两者日常命令基本一致，但 Berry 不再支持 `yarn global`，全局包需改用 `npm` 或 `pnpm` 管理。
:::

1、安装

方式一，通过 npm 全局安装（Classic 版本）：

```shell lineNumbers
npm install -g yarn
```

方式二，使用 Node 16.10+ 自带的 Corepack（可启用 Berry 版本）：

```shell lineNumbers
corepack enable
corepack prepare yarn@stable --activate
```

2、验证安装

```shell lineNumbers
yarn -v
```

3、配置国内镜像

Classic 版本直接执行：

```shell lineNumbers
yarn config set registry https://registry.npmmirror.com
```

:::tip 提示
Berry 版本改由项目内的 `.yarnrc.yml` 管理配置，在文件中添加 `npmRegistryServer: "https://registry.npmmirror.com"` 即可。
:::

### 常用命令

```shell
yarn init                  # 初始化项目，生成 package.json
yarn install               # 安装全部依赖（可简写为 yarn）
yarn add <package>         # 添加生产依赖
yarn add -D <package>      # 添加开发依赖
yarn global add <package>  # 全局安装（仅 Classic 支持）
yarn remove <package>      # 移除依赖
yarn run <script>          # 运行脚本（可简写为 yarn <script>）
```

## 五、pnpm

pnpm 通过硬链接共享全局依赖存储，安装速度快、磁盘占用小，且严格隔离依赖（避免引入未声明的"幽灵依赖"），是目前新项目的主流选择之一。

1、安装

方式一，通过 npm 全局安装：

```shell lineNumbers
npm install -g pnpm
```

方式二，Mac / Linux 使用 Homebrew：

```shell lineNumbers
brew install pnpm
```

方式三，使用 Node 自带的 Corepack 启用：

```shell lineNumbers
corepack enable
```

2、验证安装

```shell lineNumbers
pnpm -v
```

3、配置国内镜像

```shell lineNumbers
pnpm config set registry https://registry.npmmirror.com
```

### 常用命令

```shell
pnpm init               # 初始化项目，生成 package.json
pnpm install            # 安装全部依赖（可简写为 pnpm i）
pnpm add <package>      # 添加生产依赖
pnpm add -D <package>   # 添加开发依赖
pnpm add -g <package>   # 全局安装
pnpm remove <package>   # 移除依赖
pnpm run <script>       # 运行脚本（可简写为 pnpm <script>）
pnpm store prune        # 清理不再使用的全局存储
```
