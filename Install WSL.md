# 安装WSL 2

## 条件

+ Windows 10 2004 及更高版本或 Windows 11
+ x86_64 架构处理器

## 安装

> 参考链接: https://learn.microsoft.com/zh-cn/windows/wsl/install

> 建议先从 Microsoft Store 中安装“终端”应用，这个应用允许多个标签页，便于管理。

1. 以管理员权限打开 Windows PowerShell：
    + Windows 10: `Win` + `X`，点击“Windows PowerShell(管理员)(A)”
    + Windows 11: 

2. 运行以下命令：
    ```PowerShell
    wsl --install
    ```
    这样默认安装Ubuntu 22.04，如果你想安装其他发行版，使用`-d`选项指定发行版：
    ```PowerShell
    wsl --install -d ubuntu-22.04
    ```

3. 配置

打开安装好的 Ubuntu 22.04，上面会显示新的命名与设置密码，进行设置。配置好后就可以使用了。

## 换源
与普通的 Ubuntu 相同，都是修改`/etc/apt/sources.list`文件。你可以使用 Linux 终端的文本编辑器进行修改，或者在 Windows 下的文件管理器定位到对应文件进行修改。具体内容参考[清华大学开源软件站的指导](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)。

## 设置访问USB设备

> 参考链接: https://learn.microsoft.com/zh-cn/windows/wsl/connect-usb

1. 从 GitHub 上下载 USB/IP 开源项目`usbipd-win`
    ```
    https://github.com/dorssel/usbipd-win/releases/tag/v4.2.0
    ```

    选择下载`usbipd-win_4.2.0.msi`，并在 Windows 环境下运行、安装。

2. 检查 USB 设备
    在 PowerShell(管理员) 中运行以下命令，这将列出所有连接到 Windows 的 USB 设备。
    ```PowerShell
    usbipd list
    ```

3. 记住想连接设备的`BUSID`，然后运行以下命令。运行命令后，请再次使用命令`usbipd list`验证设备是否已共享。
    ```PowerShell
    usbipd bind --busid <BUSID>
    ```

4. 保证 Ubuntu 虚拟机正在运行，执行以下命令：
    ```PowerShell
    usbipd attach --wsl --busid <busid>
    ```
    请注意，只要 USB 设备连接到 WSL，Windows 将无法使用它。
