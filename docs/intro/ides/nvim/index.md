# 配置 Neovim
如果你和我一样是 Arch Linux 大牛，并且不喜欢用任何 Electron 套壳的应用，那跟着我这个教程准没错！
!!! warning "警告"
    本教程中的部分内容 **可能** 不适用于 Vim，**无法** 在 Vi 上工作。
    差异的地方在下文中会标出来。
!!! note "重要"
    本文章以 Debian GNU/Linux 13 (trixie) 作为范例。如果您的操作系统不同 (如：Windows、macOS)，则其中的某些步骤可能不适合你。

# 1. 安装 Neovim/Vim
运行下列命令：
```bash
# apt install neovim # 或者如果是 vim - 'apt install vim'
# # Arch Linux：pacman -Syu Neovim # 或者如果是 vim - 'pacman -Syu vim'
```
!!! warning "注意"
    如果您使用非 root 用户登录，请在此命令及之后涉及安装软件包的命令之前加 `sudo`，否则可能会安装失败。

# 2. 修改配置
由于 Neovim 不识别小米的 .ux 文件，因此我们需要对 Neovim 的配置文件做一些手脚。
.ux 文件实际上就是把 HTML 的结构简化了一下，去掉了 `<html />` `<head />`，并且把 `<body />` 重命名为了 `<template />`，把 `<style />` 和 `<scripts />` 外移，因此我们可以直接让 Neovim 对 .ux 文件使用 HTML 的语法高亮。
使用你喜欢的文本编辑器 (当然是 Neovim 啦 (⁠ʘ⁠ᴗ⁠ʘ⁠✿⁠)) 新建/打开 `~/.config/nvim/init.lua`，粘贴下面的内容：
```lua
vim.api.nvim_create_autocmd({"BufRead", "BufNewFile"}, {
  pattern = "*.ux",
  callback = function()
    vim.bo.filetype = "html"
    -- vim.cmd("setlocal syntax=yaml")
  end,
})
```

!!! warning "警告"
    Neovim 的配置文件和 Vim 的配置文件不通用。如果你使用 Vim，你需要编辑 `.vimrc` (VimScript) 来实现：
    ```vimscript
    augroup UXasHTML
      autocmd!
      autocmd BufRead,BufNewFile *.ux setlocal filetype=html
      " autocmd BufRead,BufNewFile *.ux setlocal syntax=html
    augroup END
    ```

# 3. 模拟器 (?)
*什么模拟器，我们 Arch Linux 用户都是 `npm run release` 之后直接放真机上跑的，Node.js 版本 v24.1.0 是 Debian 的一辈子*

# 4. 搞个签名证书 (直接从 VSCode 那边复制的，改累了)
*尽量做了修改，如果有 VSCode 字眼直接忽略就是了*

**操作步骤：**

1.  **进项目根目录**  
    `:term` 打开终端，然后：
    ```bash
    cd /path/to/your/MBNovel  # 如果终端打开时不在项目目录，先cd过去
    ```

2.  **创建并进入sign目录**  
    新建个专门放证书的文件夹，进去：
    ```bash
    mkdir sign && cd sign  # 一个命令搞定，省事儿
    ```

3.  **生成证书（重点！）**  
    把下面这坨命令**整段复制**到终端，记得把花括号里的占位符替换成你自己的信息：
    ```bash
    openssl req -x509 -newkey rsa:4096 \
      -keyout private.pem -out certificate.pem \
      -sha256 -days 3650 -nodes \
      -subj "/C=CN/ST={省份}/L={城市}/O={组织名}/OU={部门}/CN={域名}"
    ```
    **参数替换指南（跟之前一样，主要是图个乐）：**
    - `{省份}`：填拼音或英文，比如 `Shanghai` / `Guangdong`  
      *(再次提醒：填"冥王星省"是过不了的！)*
    - `{城市}`：比如 `Beijing` / `Shenzhen`
    - `{组织名}`：随便编，比如 `mbnovel` / `MySuperDuperAppFactory`
    - `{部门}`：更随意，`dev` / `product` 或者干脆 `fun_department`
    - `{域名}`：**敲黑板！** 填 `example.com` 或者你自己的域名（有就大胆用，显得专业）。

!!! tip "偷懒技巧 (万能公式)"
    如果不想一个个想，可以直接复制下面这个模板，把里面的示例值改成你自己的（理解每个字段的含义更重要哦）：
    ```bash
    -subj "/C=CN/ST=Guangdong/L=Shenzhen/O=mbnovel/OU=dev/CN=mbnovel.example.com"
    ```

4.  **锁死证书权限 (安全第一)**  
    生成完毕后，执行下面命令，给你的私钥和证书文件上把锁，免得被坏人惦记：
    ```bash
    chmod 600 private.pem certificate.pem
    ```

执行完这些，你的 `MBNovel/sign/` 目录下就会安静地躺着 `private.pem` 和 `certificate.pem` 这两个宝贝文件了。它们就是你应用的“通行证”，后续打包构建就靠它俩了！

---

**这样，Neovim/Vim 的配置就完成了！ᕙ⁠(⁠＠⁠°⁠▽⁠°⁠＠⁠)⁠ᕗ**
