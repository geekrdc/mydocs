---
title: Git 安装与配置
---

## 一、Windows 安装教程

1、下载安装包

前往 Git 官网下载对应版本的安装包：[https://git-scm.com/download/win](https://git-scm.com/download/win)，页面会自动识别系统位数，下载完成后双击运行。

2、安装过程

安装向导中大部分选项保持默认即可，关键步骤说明：

- **Select Components**：勾选需要的组件，默认即可。
- **Choosing the default editor**：选择默认编辑器，可切换为 VS Code 等习惯的编辑器。
- **Adjusting your PATH environment**：推荐选择 `Git from the command line and also from 3rd-party software`，以便在 CMD/PowerShell 中直接使用 git 命令。
- **Choosing HTTPS transport backend**：推荐选择 `Use the OpenSSL library`。
- **Configuring the line ending conversions**：推荐选择 `Checkout as-is, commit Unix-style line endings`。

其余步骤一路点击 `Next` 直至 `Install` 完成。

3、验证安装

安装完成后打开 `Git Bash` 或命令行窗口，执行：

```shell lineNumbers
git --version
```

能正常输出版本号即表示安装成功。

## 二、Mac 安装教程

Mac 可通过以下任一方式安装 Git。

1、使用 Xcode Command Line Tools（系统自带方式）

```shell lineNumbers
xcode-select --install
```

执行后会弹出安装窗口，点击「安装」并等待完成即可。

2、使用 Homebrew（推荐）

若尚未安装 Homebrew，可先参考官网安装：[https://brew.sh](https://brew.sh)，然后执行：

```shell lineNumbers
brew install git
```

安装完成后执行以下命令，能正常输出版本号即表示安装成功：

```shell lineNumbers
git --version
```

## 三、通用配置

以下配置步骤 Windows 与 Mac 通用。Windows 用户请在 **Git Bash** 中执行；若使用 **CMD**，查看公钥时请将 `cat ~/.ssh/id_ed25519.pub` 替换为 `type %USERPROFILE%\.ssh\id_ed25519.pub`。

1、配置全局用户名

```shell lineNumbers
git config --global user.name "yourname"
git config --global user.email "youremail@example.com"
```

查看配置结果：

```shell lineNumbers
git config --global --list
```

2、生成SSH秘钥

```shell
ssh-keygen -t ed25519 -C "youremail@example.com"
```

持续按回车即可。

查看生成的秘钥：

```shell
cat ~/.ssh/id_ed25519.pub
```

3、将公钥添加到代码托管平台

复制上一步输出的公钥内容（以 `ssh-ed25519` 开头），登录对应平台添加：

- **GitHub**：`Settings` → `SSH and GPG keys` → `New SSH key`，粘贴公钥并保存。
- **Gitee**：`设置` → `SSH 公钥` → 粘贴公钥并保存。
- **GitLab**：`Preferences` → `SSH Keys` → 粘贴公钥并保存。

4、测试连接

以 GitHub 为例，验证 SSH 是否配置成功：

```shell lineNumbers
ssh -T git@github.com
```

首次连接会提示是否信任主机，输入 `yes` 回车。看到 `Hi <username>! You've successfully authenticated` 即表示配置成功。
