# 配置 VSCode

!!! info "提示"
    本教程适用于所有基于 Visual Studio Code 的编辑器，例如原汁原味的 VSCode，新晋的 AI 驱动的 IDE (如：Cursor、Trae)。

!!! note "重要"
    本文章以 Debian GNU/Linux 13 (trixie) 作为范例。如果您的操作系统不同 (如：Windows、macOS)，则其中的某些步骤可能不适合你。

# 1. 安装 VSCode
以原版 VSCode 为例，运行下列命令安装 Code (OSS)：
```bash
# apt install code
```
!!! warning "注意"
    如果您使用非 root 用户登录，请在此命令及之后涉及安装软件包的命令之前加 `sudo`，否则可能会安装失败。

# 2. 启动 VSCode
在桌面/应用程序列表内找到 `Code`，启动它。

# 3. 安装小米快应用插件
**最最重要的一步**。确保你的 Code 是打开状态，然后按下 Ctrl+P 依次输入下列命令：
- aiot-ux: `ext install vela.aiot-ux`
- aiot-core: `ext install vela.aiot-core`
- aiot-project: `ext install vela.aiot-project`
- aiot-devtools: `ext install vela.aiot-devtools`
- aiot-emulator: `ext install vela.aiot-emulator`
- aiot-manifest-visualization: `ext install vela.aiot-manifest-visualization`

如果你的 Code 是 English 的，看不懂，那么可以安装一个语言包：
- 简体中文: `ext install MS-CE_vscode.vscode-language-pack-zh-hans`
- Japanese bro: `ext install MS-CE_vscode.vscode-language-pack-ja`

# 4. 打开 MBNovel 的源码文件夹
在左上角选择“打开文件夹”，选中你刚刚克隆的`MBNovel`文件夹，不需要双击，直接在右下角点击“打开”即可。

# 5. 装模拟器（VSCode坑点注意）
VSCode 居然不支持顶部 banner！只能手动开 Vela 的模拟器管理界面：
1.  **Ctrl+Shift+P** 呼出命令面板
2.  输入 `aiot: open device manager Page` → 回车  
    （这时会打开设备管理页）

接着点左上角的 **「新建」**，注意几个配置：
- **名称**：随便起，但**别用中文/空格/符号**（字母数字下划线最稳）
- **镜像**：选 `vela-watch-5.0`
- **预设**：挑 `xiaomi_band_pro`  
  *(难绷：预设名一会儿横杠`-`一会儿下划线`_`，精神分裂啊！我偏好驼峰命名，比如`xiaomiBandPro`)*

# 6. 搞个签名证书（快应用必备）

> 这步生成装包用的签名证书，跟着敲命令就行，注意替换花括号里的内容！

**操作步骤：**

1.  **进项目根目录**  
    打开终端，先切到你的MBNovel文件夹里：
    ```bash
    cd MBNovel/
    ```

2.  **创建并进入sign目录**  
    新建个专门放证书的文件夹，进去：
    ```bash
    mkdir sign && cd sign  # 两个命令一气呵成
    ```

3.  **生成证书（重点！）**  
    把下面这坨命令**整段复制**到终端，记得替换花括号内容：
    ```bash
    openssl req -x509 -newkey rsa:4096 \
      -keyout private.pem -out certificate.pem \
      -sha256 -days 3650 -nodes \
      -subj "/C=CN/ST={省份}/L={城市}/O={组织名}/OU={部门}/CN={域名}"
    ```
    **参数替换指南（别慌，都是装13的）：**
    - `{省份}`：填拼音或英文，比如 `Shanghai` / `Guangdong`  
      *(别写"火星省"，OpenSSL 不认！)*
    - `{城市}`：比如 `Beijing` / `Shenzhen`
    - `{组织名}`：随便编，比如 `mbnovel` / `MyAwesomeStudio`
    - `{部门}`：更随便，`dev` / `rnd` 甚至 `chaos` 都行
    - `{域名}`：**最重要！** 填 `example.com` 或你自己的域名（有就秀出来）

!!! tip "偷懒技巧"
    直接把下面这段改改用（记得替换，大括号里的只是示例，实际还是要根据你的情况或者上一步的解释来填写自定义内容哦）：
    ```bash
    -subj "/C=CN/ST=Guangdong/L=Shenzhen/O=mbnovel/OU=dev/CN=mbnovel.example.com"
    ```

4.  **锁死证书权限**  
    生成完执行这个，防止手滑泄露：
    ```bash
    chmod 600 private.pem certificate.pem
    ```

这时 `sign/` 目录下会出现 `private.pem` 和 `certificate.pem` 两个文件，就是你的签名证书了。把它们当传家宝收好，后续打包要用！

**这样，VSCode 就配置完成啦！**
