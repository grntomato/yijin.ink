+++
date = '2026-11-21T19:57:37+08:00'
draft = false
title = '复活落寞小站之路(一)恢复 Git 工作流'
author = "Yijin Wang"
tags = ['Git', 'SSH']
categories = ['学习录']
slug = 'restore-git-workflow'
+++

## 前言

由于近期都在用另一个电脑，导致更文就必须重新拉回我的小站代码到本地..... 清除这些障碍，让自己重新记录吧 (ᗒᗣᗕ)՞ ⚔

> 此文记录我的 Git 工作流恢复过程，首先问询了 ChatGPT，同时阅览了 Git、Github 的 docs 来确认及加深印象。遇到无法理解的部分，我会标识，以及认为自己还需理解、描述不清的地方会以” 引用 “的方式将 ChatGPT 的说明贴入，方便日后理解。其余均个人复述、总结，加深记忆。

## 一、重新认识我的小站

终端上的 `user.name`  `user.email` 的提示、反复输入多次、邮箱好像会影响提交者的头像是否关联到我自己的账户..... 但已经记不得怎么查看当前这台机子是否已配置过这些，也不确定这些到底是什么，有什么作用。我顺着这个开始启程....

### 1.1 回忆落寞小站的构建方式

AI 和我确认 `user.name`  `user.email` 确实是 Git 的配置，那这条路没错咯 (❁´◡\`❁)

建这个小站的时候，我还不会梯子，我一直用的微软的 steam++ 来浏览 Github，但它无法正常通过 HTTPS (443 端口) 和 SSH (22 端口) 连接 GitHub，因此我选择先将本地代码 push 到 Gitee 仓库，再通过 Gitee 的自动镜像服务同步到我的 Github 仓库中，最后由 Cloudflare 从 Github 仓库中拉取代码并构建网站。

### 1.2 这次需要恢复什么

嗯... 不清晰的地方有：

1. Git 里的 `user.name`  `user.email` 配置
2. 连接 Github ?
3. 这次是 push 到 Gitee 还是 Github？(后来决定还是按照之前的路径)
4. 提交命令...?
5. 字体子集的自动化

## 二、恢复 Git 基础环境

### 2.1 更新 Git 版本 / 安装 Git

我是通过 scoop 管理的 git 版本，所以先一键更新：`scoop update git`

如果还未安装 git，可以选择各种包管理工具安装，如 scoop、winget、chocolatey ，或者直接去 git 官网安装。

安装后， `git --version` ，返回版本号即安装成功 (・̀ᴗ・́)و

e.g. 返回 `git version 2.56.1.windows.1`

### 2.2 检查并配置 Git 用户名、邮箱

`user.name` 和 `user.email` 是 Git 在创建提交记录 (commit) 时，写入的一组**作者身份信息**。这两个配置用于标识 Git Commit 中的作者信息，并不等同于 GitHub 的登录凭据，也不负责 SSH 身份认证。

查看 **Git 全局配置** ：` git config --global --list`

由于返回中没有显示 `user.name` 及 `user.email`，所以键入以下命令配置用户名与邮箱：

```bash
git config --global user.name "Yijin"
git config --global user.email "xxx@example.com"
```

关于 `git-config` 的用法：

1. [Git - First-Time Git Setup](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup)
2. [Git - git-config Documentation](https://git-scm.com/docs/git-config)
3. 输入 `git help config` 可以打开 git config 帮助文档。

```
# 指令解释图
git config --global user.name "Yijin"
│   │      │         │         │
│   │      │         │         └─ 设置的值
│   │      │         └─ 配置项名称
│   │      └─ 配置作用范围
│   └─ 功能
└─ 程序
```

可查看设置好的 **Git 全局配置文件**：`git config --global --edit`，确认之前的配置：

Git bash 自动进入 vim ，关于 vim 退出时的用法：

`:q` 退出 (没有修改时)

`:q!` 不保存，强制退出

`:w`  意为 `write`  保存

`:wq` 保存并退出

`:x` 有修改则保存，然后退出

## 三、恢复 SSH 连接

这里可以直接参考 GitHub docs 中直接给出的步骤与解释：
[Connecting to GitHub with SSH - GitHub Docs](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)

### 3.1 给电脑生成一个专门的 SSH 密钥对

**新建一个 SSH 密钥对**：

`ssh-keygen -t ed25519 -C "Yijin-Thinkpad-E14"`，生成 2 个文件，一个私钥文件 `id_ed25519`，一个公钥文件 `id_ed25519.pub`

`keygen` 为 Key Generator (密钥生成器) 的缩写。

`-t` 为 type，用于指定密钥算法。

`-C` 为 comment，对生成的 SSH 密钥附加一段注释、说明。所以我把当前电脑的型号作为注释，方便日后还能认得这个密钥是哪个设备的。

私钥文件自己保留，不可随意分享，公钥文件则添加给要连接的平台，比如 Github 或者 Gitee。`-C` 添加的注释会以明文形式显示在 SSH 公钥文件末尾，方便我们识别密钥属于哪台设备。同时，注释也会保存在私钥文件的内部格式中；如果设置了 Passphrase，私钥文件中的相关数据会受到加密保护。

### 3.2 Passphrase 与 SSH Agent

接着会提示 `Enter file in which to save the key:` 如果不更改保存路径，可以再次按 `enter` ，会继续提示输入 `passphrase` ，这个密码短语类似私钥的第二道保险，在每次使用 私钥需要再次输入 `passphrase` 来解锁。而 **SSH Agent** 是一个**密钥管理进程**，可以在内存中暂存已经解锁的密钥，供 SSH 客户端反复使用。如此，也不用反复输入 `passphrase` 了。

> 工作过程
>
> 1. 你输入 Passphrase，解锁私钥。
> 2. SSH Agent 在内存中保存可用的密钥。
> 3. Git 调用 SSH 连接 GitHub 或 Gitee。
> 4. SSH Agent 提供密钥，无需反复输入口令。

关于如何把 **SSH 私钥加入 SSH Agent** 中，Github 也已说明：[Generating a new SSH key and adding it to the ssh-agent - GitHub Docs](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#adding-your-ssh-key-to-the-ssh-agent)

1. 先确认 ssh-agent 是运行
2. 将 SSH 私钥添加到 ssh-agent 中：`ssh-add c:/Users/YOU/.ssh/id_ed25519`

**关于 OpenSSH 工具**：

| 程序               | 作用                          |
| ---------------- | --------------------------- |
| `ssh.exe`        | SSH Client，负责建立 SSH 连接      |
| `ssh-agent.exe`  | 管理已加载的私钥                    |
| `ssh-add.exe`    | 向 Agent 添加或移除密钥             |
| `ssh-keygen.exe` | 生成密钥对、处理密钥及签名相关操作           |
| `sshd.exe`       | SSH Server，接受 SSH 连接并进行身份认证 |

### 3.3 Windows 双 OpenSSH 环境冲突

这里可能会遇到一个问题，我也遇到了 👉 [Troubleshooting SSH agent conflicts in Windows](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#troubleshooting-ssh-agent-conflicts-in-windows)

我个人理解是，Winodows 系统提供的 OpenSSH 客户端与 Git for Windows 里自带的是不同的两套 OpenSSH。

例如，我在 PowerShell 中使用 Windows OpenSSH 的 ssh-add 将私钥加载到 Windows ssh-agent 后，而 Git Bash 使用另一套 SSH 客户端及 Agent 环境，就无法直接使用我在 windows ssh-agent 里已经加载的密钥。

因此，可以通过 Git 的 `core.sshCommand` 指定 Windows OpenSSH 的 ssh.exe，并通过 Git Bash 的 `alias` 将 ssh 和 ssh-add 指向 Windows OpenSSH 对应的程序，从而统一 SSH 客户端和 Agent 的使用环境。

```bash
# 第一步 指定 Git 使用 Windows SSH 客户端
git config --global core.sshCommand "C:/Windows/System32/OpenSSH/ssh.exe"

# 第二步 在 Git Bash 设置别名
alias ssh='/c/Windows/System32/OpenSSH/ssh.exe'
alias ssh-add='/c/Windows/System32/OpenSSH/ssh-add.exe'
```

为了让 alias 在每次启动 Git Bash 时自动生效，需要将上述第二步写入 `~/.bashrc` 文件，如可以使用 `notepad.exe ~/.bashrc` 打开配置文件写入设置别名的配置。

| 设置                | 解决什么问题                                            |
| ----------------- | ------------------------------------------------- |
| `core.sshCommand` | Git 执行 Push、Fetch 时使用 Windows SSH                 |
| `alias ssh`       | 在 Git Bash 直接输入 ssh 时使用 Windows SSH               |
| `alias ssh-add`   | 在 Git Bash 直接输入 ssh-add 时使用 Windows SSH 的 ssh-add |

### 3.4 将公钥添加给 Github / Gitee

由于之前这个的时候，我还未有机会使用代理，所以我是先 push 到 Gitee 的仓库，然后利用 Gitee 的自动镜像服务镜像到我的 Github 仓库。所以我还需要使用 SSH 协议连接到 Gitee。

`clip < ~/.ssh/id_ed25519.pub` 复制公钥内容至 Windows 剪贴板，并添加到 Github 的 SSH Keys (如果使用的 Gitee，即 SSH 公钥) 中。选择 New SSH key (Gitee 是 点击 SSH 公钥就显示了添加公钥界面)

### 3.5 测试 SSH 连接至 Github / Gitee

`ssh -T git@github.com`

`ssh -T git@gitee.com`

会看到：

```
> The authenticity of host 'github.com (IP ADDRESS)' can't be established.
> ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
> Are you sure you want to continue connecting (yes/no)?
```

这里需要比对一下给出的指纹是不是 GitHub 服务器的 SSH Host Key Fingerprint (主机密钥指纹)：[GitHub's public key fingerprint](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints)，如果是的话，输入 `yes`

显示以下就代表成功了 ✺◟(͡° ͜ʖ ͡°)◞✺

```
> Hi USERNAME! You've successfully authenticated, but GitHub does not
> provide shell access.
```
