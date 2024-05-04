macOS 下 N1 盒子降级及刷 Armbian

## 先决条件

1. N1 盒子
2. 双公 USB 线(如果电脑有 type-c 口可以用一头公 USB 一头 type-c 线代替)
3. 网线
4. HDMI 线
5. U 盘（推荐使用 USB2.0）
6. 鼠标
7. 电脑（windows 或者 macOS）
8. 显示器（没有显示可用 USB 视频采集卡 + OBS）

## 背景

在 macOS 下对 N1 盒子进行降级及刷入 Armbian 系统，在 windows 下则使用「官方降级工具」

## 降级

1. N1 接网线接路由器，确保电脑网络及盒子网络处于同一网段
2. N1 接鼠标（注意：使用靠近 HDMI 接口的 USB 口）
3. N1 接 USB 双公头线连接到电脑
4. N1 接 HDMI 线连接显示器
5. 上电，记录 N1 盒子 ip
6. 鼠标连续点击 N1 盒子版本号位置 4 下，打开盒子的 adb 服务（版本号低于 2.20，不需要降级）
7. mac 安装 adb，终端下执行
   ```
    brew install android-platform-tools
   ```
8. 进入 fastboot 模式，在终端下分别执行
   N1_IP 替换为步骤 5 的 ip 地址

   ```
   adb connect $N1_IP

   adb shell reboot fastboot
   ```

9. 刷入 boot 等固件，等 N1 重启后，在终端分别执行执行（文件在降级包中）

   ```
   fastboot flash boot boot.img

   fastboot flash bootloader bootloader.img

   fastboot flash recovery recovery.img
   ```

   如果出现 < waiting for any device > 则排除双公头线是不是接通或者换一条或者重启

10. 重启，终端执行
    ```
     fastboot reboot
    ```

## 制作系统盘

1. [下载 Armbian 镜像](https://github.com/ophub/amlogic-s9xxx-armbian/releases)（选择 s905d）
2. mac 接入 U 盘，使用 balenaEtcher 制作镜像

## 启动系统盘

1. 鼠标拔出（注意：双公 USB 线勿动，留出离 HDMI 接口近的那个接口在后面步骤插入 U 盘）
2. 在端上预输入如下命令，先别执行

   ```
      adb shell reboot update
   ```

3. 准备敲回车和准备插入 U 盘, 敲击回车, 在显示器黑屏的那一刹那把 U 盘插入; 如果没有进入 Linux 系统，请重试
4. 进入系统，修改账号和密码（默认账密 root 1234）

## 将系统写入 EMMC

1. 进入系统后执行
   ```
       nand–sata-install
   ```
2. 按提示选择或填写相关信息即可

## 参考链接

[Mac 下给斐讯 N1 盒子刷机 Linux 系统 Armbian](https://blog.newnius.com/burn-linux-os-armbian-to-phicomm-n1-under-mac.html)
