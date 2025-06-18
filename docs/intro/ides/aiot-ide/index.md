# 配置 AIoT IDE

!!! note "重要"
    本文章以 Debian GNU/Linux 13 (trixie) 作为范例。如果您的操作系统不同 (如：Windows、macOS)，则其中的某些步骤可能不适合你。

# 1. 下载 AIoT IDE
由于 CS 小米不直接把软件包放 Debian 软件仓库内，因此我们需要手动去下载安装包。
进去 [小米自己的私有云](https://kpan.mioffice.cn/webfolder/ext/j6SfQsarf8I%40?n=0.18700074913007825)，提取码输 **99E6**，然后进入 `1.6.0/` 目录，下载里面的 `AIot IDE Ubuntu.deb`。
!!! info "提示"
    用 Windows 的 bro 下载 `AIot IDE Windows.exe`，Intel 芯片的 Mac 用户下载 `AIot IDE Mac x64.zip`，果子 M 芯片的 Mac 用户下载 `AIot IDE Mac arm.zip`。双击安装即可 (Mac 需要解压后双击里面的 .dmg 安装包安装)。

# 2. 安装 AIoT IDE 
一行命令，丝滑。
```bash
# apt install -y /path/to/your/AIot\ IDE\ Ubuntu.deb
```
!!! warning "注意"
    如果您使用非 root 用户登录，请在此命令及之后涉及安装软件包的命令之前加 `sudo`，否则可能会安装失败。

# 3. 启动 AIoT IDE & 打开项目
安装完毕后，在你的桌面环境或者应用程序列表里找到新安装的 AIoT IDE，点开它。
界面和 VSCode 长得差不多，毕竟是亲兄弟（魔改版）。

接着，把咱们的 MBNovel 项目文件夹给打开：
在IDE左上角菜单栏找到类似“文件(File)” -> “打开文件夹(Open Folder)...”的选项，然后选中你之前克隆好的 `MBNovel` 文件夹，点“打开”。

# 4. 装模拟器 (AIoT IDE 省心多了！)

还记得之前用VSCode时，因为没banner搞得手忙脚乱开模拟器管理吗？在AIoT IDE里，小米给咱们整了个大大的惊喜！

**操作流程简直不要太友好：**

1.  **顶部Banner大法好！**  
    AIoT IDE 被小米魔改后，终于能显示顶部Banner了！启动IDE后，你应该能在顶部看到一个带有各种功能按钮的区域。找到那个写着**“模拟器”**的按钮，直接点它！
    （终于不用满世界找命令了，感动哭！）

2.  **侧边栏闪亮登场！**  
    点击Banner上的“模拟器”按钮后，IDE的右侧会biu地一下弹出一个新的 **侧边栏 (Sidebar)**，这个也是小米特供的。这个侧边栏就是模拟器管理的核心区域了。

3.  **“新建”模拟器，就这么简单！**  
    在那个新弹出的模拟器侧边栏里，一眼就能看到一个 **“新建”**的按钮。果断点下去！

4.  **配置模拟器参数 (老熟人了)**  
    点完“新建”后，会弹出一个配置界面，这里的步骤就和咱们在VSCode里折腾的差不多了，填就完事儿：
    *   **名称**：老规矩，随便起，但记得用**字母数字下划线组合，别搞中文、空格或特殊字符**，免得后面出幺蛾子。
    *   **镜像**：毫不犹豫，选 `vela-watch-5.0`。
    *   **预设**：还是那个让人又爱又恨的 `xiaomi_band_pro`。  
        *（大声BB：这里的命名还是那么“精神分裂”，横杠下划线齐飞。不过看在IDE这么方便的份上，忍了！)*

搞定这些配置后，点个“确定”或“创建”，模拟器就开始创建了。是不是比之前在VSCode里手动敲命令方便多了？小米这波魔改，值得点赞！

# 5. 搞个签名证书（快应用必备步骤，和VSCode一致）
!!! tip "提示"
    这一步是生成快应用打包时必需的签名证书。如果你在配置VSCode时已经为MBNovel项目生成过一次，并且放在`MBNovel/sign/`目录下了，**那么这里就可以跳过！** 证书是通用的，不用每个IDE都搞一遍。
    如果是全新配置，或者想重新生成，那就跟着下面的步骤来。

**操作步骤：**

1.  **进项目根目录**  
    打开IDE内置的终端（通常在底部，或者按快捷键 ``Ctrl+` `` 调出），确保当前路径是在你的MBNovel项目文件夹里：
    ```bash
    cd /path/to/your/MBNovel  # 如果终端打开时不在项目目录，先cd过去
    ```

2.  **创建并进入sign目录**  
    新建个专门放证书的文件夹，进去：
    ```bash
    mkdir sign && cd sign  # 一个命令搞定，省事儿
    ```

3.  **生成证书（重点！跟VSCode那边一样）**  
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

这样，AIoT IDE 的环境配置就基本完成了！是不是感觉小米这套 IDE 在模拟器这块儿确实贴心了不少？
