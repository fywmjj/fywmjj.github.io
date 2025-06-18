# 配置环境
!!! abstract "TL;DR"
    本文讲述了如何在电脑上克隆 MBNovel 仓库，并库配置 Node.js 环境。

---


!!! note "重要"
    本文章以 Debian GNU/Linux 13 (trixie) 作为范例。如果您的操作系统不同 (如：Windows、macOS)，则其中的某些步骤可能不适合你。

!!! warning "警告"
    使用 MBNovel 进行开发前，请确保你具有最基础的 HTML5、CSS3、JavaScript (最好会 ES5/6) 技能，避免在后续开发中遇到困难。

# 1. 安装 Node.js
首先更新你的软件源。打开一个终端 (或切换到 tty)，运行下列命令：
```bash
# apt update
```
!!! tip "提示"
    在国内网络环境，如果这个过程异常慢，可以先按下 Ctrl+C (^C) 停止命令，然后用你喜欢的文本编辑器编辑 `/etc/apt/sources.list` (俗称"换源")，将里面的内容替换为下面的内容以提升 "拉包" 速度。
    ```
    # 清华大学 (TUNA) 源，你也可以使用你喜欢的
    # 可以把所有的 `testing` 字段替换为稳定的 `trixie`，但我这里追求版本最新，就不改了。有需要的可以修改
    deb https://mirrors.tuna.tsinghua.edu.cn/debian/ testing main contrib non-free non-free-firmware
    # deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ testing main contrib non-free non-free-firmware
    
    deb https://mirrors.tuna.tsinghua.edu.cn/debian/ testing-updates main contrib non-free non-free-firmware
    # deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ testing-updates main contrib non-free non-free-firmware
    
    deb https://mirrors.tuna.tsinghua.edu.cn/debian/ testing-backports main contrib non-free non-free-firmware
    # deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ testing-backports main contrib non-free non-free-firmware
    
    # 以下安全更新软件源包含了官方源与镜像站配置，如有需要可自行修改注释切换
    deb https://security.debian.org/debian-security testing-security main contrib non-free non-free-firmware
    # deb-src https://security.debian.org/debian-security testing-security main contrib non-free non-free-firmware
    ```

然后运行下列命令：
```bash
# apt install nodejs-lts npm
```
!!! warning "注意"
    如果您使用非 root 用户登录，请在此命令及之后涉及安装软件包的命令之前加 `sudo`，否则可能会安装失败。

!!! tip "提醒"
    **本框架最低要求 Node.js 版本 大于等于 18.0.0**。如果你的发行版提供了过旧的版本，请将 `nodejs-lts` 替换为 `nodejs`，这将牺牲稳定性，但能获得最新版本。

当安装完毕后，执行下列命令确认 Node.js 的安装状态：
```bash
# node -v
# npm -v
```
如果这两个命令都有输出，则说明安装成功了。

# 2. 安装 OpenSSL
OpenSSL 在大多数 Linux 发行版和 macOS 上都已预装。您可以运行下面的命令查看 OpenSSL 的版本：
```bash
# openssl -v
```
如果没有安装，使用下面的命令安装：
```bash
# apt install openssl
```
*Windows 端的 OpenSSL 安装方法自行搜索。这里不再赘述。*
!!! warning "警告"
    在 Windows 上安装玩 OpenSSL 后，需要将 OpenSSL 的可执行文件目录添加到系统的**环境变量**中，否则 OpenSSL 无法使用。
     打开 "控制面板"，进入 "系统"→"高级系统设置"→"环境变量"，找到名为 `PATH` 的列表项，双击，然后在弹出的窗口中单击 "新建"，输入 OpenSSL 的安装路径。然后重启你的电脑以确保更改生效。

# 3. 克隆 MBNovel 源代码
先确保安装了 `git`。大多数情况下，git 是不会预装的，运行下列命令来安装：
```bash
# apt install git
```
安装完毕后，克隆 MBNovel 源代码。
```
# git clone -b {branch_name} https://github.com/fywmjj/MBNovel.git
```
!!! info "信息"
    `{branch_name}` 是一个占位符。
    MBNovel 提供多种 "flavor"，供不同需求的开发者选择使用。以下是可选的值：
    - `stable` MBNovel 的稳定版本。它包含了最稳定的功能，并且包含最少的 bugs。唯一的缺点是**更新速度较慢**。
    - `mainline` MBNovel 的最新版本。它包含一些 stable 版本没有的前瞻功能，包括 *快速读档/存档* *对话回顾* *交互时震动* 等功能，并且对设置页面进行了全新的设计，还新增了关于页面。它的功能进行过一些基本的测试。
    - `testing` MBNovel 的开发版本。所有的 commits 最开始都会汇集到此分支，它包含了最新的功能和最多的 bugs。我们**不建议在发布版本中使用此分支的源代码，且不对其稳定性负责**。

!!! tip "提示"
    在国内网络环境下，开发者可能会遇到克隆速度慢甚至直接 `connection closed before handshake` `connection closed during handshake` `SSL:handshake:err:(...)` `TCP connection timeout` 等问题，此时可更换镜像源解决问题。执行下列命令：
    ```bash
    # git clone -b {branch_name} https://github.moeyy.xyz/https://github.com/fywmjj/MBNovel.git
    ```

克隆完毕后，进入源代码目录：
```bash
# cd MBNovel/
```

然后，运行下列命令安装项目依赖：
```bash
# npm install --legacy-peer-deps
```

当这行命令执行成功后，就可以关掉你的终端了。
**搞定！环境搭好了，接下来我们看看怎么配编辑器/IDE吧。**
