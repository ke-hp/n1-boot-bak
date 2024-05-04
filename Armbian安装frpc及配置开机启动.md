# Armbian 安装 frpc 及配置开机启动

## 背景

在 Armbian 中安装 frp 客户端即 frpc, 并配置配置开机启动, 文档目的是安装及配置开机启动, 非 frp 详细教程

## 安装

1. 下载 [frp](https://github.com/fatedier/frp/releases), 选择与 frps 相匹的版本, 选 arm64 架构(例 frp_0.51.3_linux_arm64.tar)
2. 解压 `tar -zxvf frp_0.51.3_linux_arm64.tar`
3. 移动 `mkdir /opt/frpc && mv frp_0.48.0_linux_arm64/frpc /opt/frpc/`
4. 配置 `cd /opt/frpc && vim frpc.ini`
5. 启动 `/opt/frpc/frpc -c /opt/frpc/frpc.ini`

## 配置开机启动

### 方式一

创建 frpc.service 服务文件

```
vim /etc/systemd/system/frpc.service
```

```
[Unit]
# 服务名称, 可自定义
Description = frp client
After = network.target syslog.target
Wants = network.target

[Service]
Type = simple
Restart=on-failure
RestartSec=60s
# 启动frps的命令, 需修改为您的frpc的安装路径
ExecStart = /opt/frpc/frpc -c /opt/frpc/frpc.ini

[Install]
WantedBy = multi-user.target
```

配置开机自启动

```
systemctl enable frpc
```

systemd 命令管理 frpc

```
# 启动frpc
systemctl start frpc

# 停止frpc
systemctl stop frpc

# 重启frpc
systemctl restart frpc

# 查看frpc状态
systemctl status frpc
```

### 方式二

开机启动脚本中添加命令

添加配置

```
vim /lib/systemd/system/rc.local.service
```

在后面添加

```
[Install]
WantedBy=multi-user.target
Alias=rc-local.service
```

在/etc/rc.local 中添加开机启动脚本

```
vim /etc/rc.local
```

exit 0 前添加

```
nohup /opt/frpc/frpc -c /opt/frpc/frpc.ini >output 2>&1 &

```

## 参考链接

1. [Armbian 上配置 frp 客户端](https://www.erballoon.vip/2023/03/23/armbianspzfrpkhd/)
2. [开启 armbian 系统的开机启动脚本 rc.local](https://www.right.com.cn/forum/thread-352044-1-1.html)
