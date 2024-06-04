# Jetson Nano (Waveshare)

> 官方文档：https://www.waveshare.net/wiki/JETSON-NANO-DEV-KIT

官方支持的最高系统版本是 Ubuntu 18.04，自带Python 3.6.9。

## 准备

### 硬件
1. Jetson Nano 开发板
2. 5V 4A 电源适配器
3. 跳线帽（双母头导线也可）
4. Micro-USB数据线
5. 显示器（可使用HDMI或DP）
6. SD卡

### 软件
Debian系的Linux发行版，虚拟机或实体机均可。教程使用WSL。
> VMware Workstation on Windows、VMware Fusion on Mac(x86) 也已通过测试。所有Arm架构的设备（树莓派，MacBook on Apple Silicon）都没有通过测试。

## 下载资源
1. 进入你的Linux系统，创建一个新的目录来存放相关文件。
    ```shell
    mkdir Jetson
    cd Jetson
    ```
    如果没有，安装`wget`
    ```shell
    sudo apt-get install wget
    ```

2. 下载资源
    ```shell
    wget https://developer.nvidia.com/embedded/l4t/r32_release_v7.4/t210/jetson-210_linux_r32.7.4_aarch64.tbz2
    wget https://developer.nvidia.com/embedded/l4t/r32_release_v7.4/t210/tegra_linux_sample-root-filesystem_r32.7.4_aarch64.tbz2
    ```

3. 解压
    ```shell
    sudo tar -xjf jetson-210_linux_r32.7.4_aarch64.tbz2
    cd Linux_for_Tegra/rootfs/
    sudo tar -xjf ../../tegra_linux_sample-rootfilesystem_r32.7.4_aarch64.tbz2
    cd ../
    sudo ./apply_binaries.sh
    ```
    > 第二、三行的目的是将`rootfilesystem`解压到`Linux_for_Tegra/rootfs/`目录下。

## 修改设备树

> 让系统可以识别到SD卡。

1. 安装修改工具
    ```shell
    sudo apt-get install device-tree-compiler
    ```

2. 反编译二进制文件
    > 确保你在`Linux_for_Tegra`目录下。
    ```Shell
    cd kernel/dtb
    sudo dtc -I dtb -O dts -o tegra210-p3448-0002-p3449-0000-b00.dts tegra210-p3448-0002-p3449-0000-b00.dtb 
    ```

3. 修改设备树
    > 由于文件很长，使用终端编辑的话很费劲，所以建议用 Visual Studio Code 连接 WSL 后使用文本编辑器进行查找修改。
    > 如果你是带GUI的虚拟机或实体机的话，可以使用`gedit`进行修改。

4. 重新编译回二进制文件
    ```
    dtc -I dts -O dtb -o tegra210-p3448-0002-p3449-0000-b00.dtb tegra210-p3448-0002-p3449-0000-b00.dts
    ```

## 烧写系统

1. 用跳线帽或杜邦线连接`FC REC` 和 `GND`引脚。
2. 使用Micro-USB数据线连接电脑和Jetson Nano。如果需要，进行操作使 WSL 或虚拟机能够识别 Jetson Nano。
3. 确保你在`Linux_for_Tegra`目录下，执行以下命令：
    ```shell
    sudo ./flash.sh jetson-nano-emmc mmcblk0p1
    ```
4. 等吧，时间挺长的。

## 配置系统

1. 移除跳线帽，拔掉数据线。接上电源、键鼠和显示器。
2. 这里有友好的 GUI，按照步骤初始化系统吧。

