macOS 下 N1 盒子降级及刷 Armbian

## 先决条件

1. N1 盒子
2. 双公 USB 线(如果电脑有 type-c 口可以用 一头公 USB 一头 type-c 线代替)
3. 网线
4. HDMI 线
5. U 盘（推荐使用 USB2.0）
6. 鼠标
7. 电脑(windows 或者 macOS)
8. 显示器(没有显示可以用 USB 视频采集卡 + OBS)

## 背景

在 macOS 对 N1 盒子进行降级及刷入 Armbian 系统

## 降级

1. N1 接网线连接网络，确保电脑和盒子处于同一网段
2. N1 接 USB 双公头线连接到电脑，注意使用靠近 HDMI 接口的 USB 口
3. N1 接鼠标
4. N1 接 HDMI 线连接显示器，上电，记录下来 N1 盒子的 ip
5. 鼠标连续点击 N1 盒子版本号位置 4 下，打开盒子的 adb 服务（版本号低于 2.20，不需要降级）
6. mac 安装 adb，终端下执行 `brew install android-platform-tools`
7. 进入 fastboot 模式, 终端下执行

   ```
    adb connect $N1_IP          # $N1_IP 为 步骤 3 的 ip 地址
    adb shell reboot fastboot
   ```

8. 刷入 boot 等固件，等 N1 重启后，在终端分别执行执行（文件在降级包中）

   ```
    fastboot flash boot boot.img
    fastboot flash bootloader bootloader.img
    fastboot flash recovery recovery.img

    # 如果出现 < waiting for any device > 则排除双公头线是不是接通或者换一条或者重启
   ```

9. 重启, 终端执行 fastboot reboot

## 制作系统盘

1. 下载 Armbian 镜像 https://github.com/ophub/amlogic-s9xxx-armbian/releases （选择 s905d, 也可直接使用项目提供的
   镜像）
2. U 盘接入，使用 balenaEtcher 制作镜像

## 启动系统盘

1. 把鼠标拔下来，刷机线别拔。这块需要注意的是要留出离 HDMI 接口近的那个接口，因为实践中发现只有这个 USB 口可以加载 U 盘里的系统
2. 在端上输入`adb shell reboot update` 别急着回车
3. 一只手准备敲回车，另一只手准备好插入 U 盘; 敲击回车; 然后另一只手再以迅雷不及掩耳响叮当之势在黑屏的那一刹那把 U 盘插进去, 如果没有进入 Linux 系统，请重试
4. 如果显示器显示进入系统，则可以进行修改账号和密码 (默认账号 root，密码 1234)

## 将系统写入 EMMC

1. 进入系统后执行 `nand–sata-install`
2. 按提示选择或填写相关信息即可

## 参考链接

[Mac 下给斐讯 N1 盒子刷机 Linux 系统 Armbian](https://blog.newnius.com/burn-linux-os-armbian-to-phicomm-n1-under-mac.html)
