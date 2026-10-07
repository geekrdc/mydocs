---
title: Git 安装与配置
---

## 一、Windows 安装教程

### 1、下载安装包

前往 Git 官网下载对应版本的安装包：[git-scm.com/download/win](https://git-scm.com/download/win)，页面会自动识别系统位数，下载完成后双击运行。

### 2、安装过程

安装向导中大部分选项保持默认即可，关键步骤说明：

- **Select Components**：勾选需要的组件，默认即可。
- **Choosing the default editor**：选择默认编辑器，可切换为 VS Code 等习惯的编辑器。
- **Adjusting your PATH environment**：推荐选择 `Git from the command line and also from 3rd-party software`，以便在 CMD/PowerShell 中直接使用 git 命令。
- **Choosing HTTPS transport backend**：推荐选择 `Use the OpenSSL library`。
- **Configuring the line ending conversions**：推荐选择 `Checkout as-is, commit Unix-style line endings`。

其余步骤一路点击 `Next` 直至 `Install` 完成。

### 3、验证安装

安装完成后打开 `Git Bash` 或命令行窗口，执行：

```shell lineNumbers
git --version
```

能正常输出版本号即表示安装成功。

## 二、Mac 安装教程

Mac 可通过以下任一方式安装 Git。

### 1、使用 Xcode Command Line Tools（系统自带方式）

```shell lineNumbers
xcode-select --install
```

执行后会弹出安装窗口，点击「安装」并等待完成即可。

### 2、使用 Homebrew（推荐）

若尚未安装 Homebrew，可先参考官网安装：[brew.sh](https://brew.sh)，然后执行：

```shell lineNumbers
brew install git
```

安装完成后执行以下命令，能正常输出版本号即表示安装成功：

```shell lineNumbers
git --version
```

## 三、通用配置

以下配置步骤 Windows 与 Mac 通用。Windows 用户请在 **Git Bash** 中执行；若使用 **CMD**，查看公钥时请将 `cat ~/.ssh/id_ed25519.pub` 替换为 `type %USERPROFILE%\.ssh\id_ed25519.pub`。

### 1、配置全局用户信息

```shell lineNumbers
git config --global user.name "yourname"
git config --global user.email "youremail@example.com"
```

建议同时设置默认分支名，省去每次初始化仓库时的改名与警告：

```shell lineNumbers
git config --global init.defaultBranch main
```

查看配置结果：

```shell lineNumbers
git config --global --list
```

### 2、生成 SSH 密钥

```shell
ssh-keygen -t ed25519 -C "youremail@example.com"
```

后续三个提示（密钥保存路径、密码短语、确认密码短语）直接回车采用默认值即可。

查看生成的公钥：

```shell
cat ~/.ssh/id_ed25519.pub
```

:::tip 提示
若公司的 Git 服务较旧、不支持 ed25519 算法，可改用兼容性更好的 RSA：`ssh-keygen -t rsa -b 4096 -C "youremail@example.com"`。
:::

### 3、将公钥添加到代码托管平台

复制上一步输出的公钥内容（以 `ssh-ed25519` 开头），登录对应平台添加：

- **GitHub**：`Settings` → `SSH and GPG keys` → `New SSH key`，粘贴公钥并保存。
- **Gitee**：`设置` → `SSH 公钥` → 粘贴公钥并保存。
- **GitLab**：`Preferences` → `SSH Keys` → 粘贴公钥并保存。

### 4、测试连接

以 GitHub 为例，验证 SSH 是否配置成功：

```shell lineNumbers
ssh -T git@github.com
```

首次连接会提示是否信任主机，输入 `yes` 回车。看到 `Hi <username>! You've successfully authenticated` 即表示配置成功。

## 四、常见问题

### clone 或 push 速度很慢

多为网络原因。使用 HTTPS 方式时，可为 Git 单独配置代理（端口按实际代理软件修改）：

```shell lineNumbers
git config --global http.proxy http://127.0.0.1:7890
git config --global https.proxy http://127.0.0.1:7890
```

取消代理：

```shell lineNumbers
git config --global --unset http.proxy
git config --global --unset https.proxy
```

:::tip 提示
使用 SSH 方式时，上面的 http.proxy 不会生效，需要单独配置 SSH 的代理，建议直接参考下一条改用 443 端口。
:::

### SSH 连接 GitHub 超时

若 `ssh -T git@github.com` 长时间无响应，可能是 22 端口受限，可改走 443 端口。在 `~/.ssh/config` 文件（没有则新建）中添加：

```shell
Host github.com
  HostName ssh.github.com
  Port 443
  User git
```

保存后再次执行 `ssh -T git@github.com` 测试。

### Windows 下换行符警告

执行 git 命令时若出现 `LF will be replaced by CRLF` 提示，属于 Windows 换行符自动转换的正常行为，可忽略。若希望仓库统一保持 LF，可执行：

```shell lineNumbers
git config --global core.autocrlf input
```
