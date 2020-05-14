---
title: 树莓派相关设置
p: raspberryPi/baseCommand
date: 2020-01-22 19:10:18
tags: 树莓派
category: raspberryPi
---

树莓派硬件为3b+，系统版本debian9.9

查看树莓派系统参数命令如下

```bash
getconf LONG_BIT        # 查看系统位数
uname -a            # kernel 版本
/opt/vc/bin/vcgencmd  version   # firmware版本
strings /boot/start.elf  |  grep VC_BUILD_ID    # firmware版本
cat /proc/version       # kernel
cat /etc/os-release     # OS版本资讯
cat /etc/issue          # Linux distro 版本
cat /etc/debian_version     # Debian版本编号
```

### 配置wifi连接

1. 打开文件

   ```bash
   vim /etc/wpa_supplicant/wpa_supplicant.conf
   ```

2. 增加配置

   ```bash
   network={
       ssid="wifi名称"
       psk="wifi密码"
       priority=4 连接优先级，数字越大优先级越高（不可以是负数）
       #scan_ssid:连接隐藏WiFi时需要指定该值为1
   }
   ```

3. 拔网线，重启

   ```bash
   reboot
   ```

### 配置静态IP

路由器的WiFi一般为自动分配，如果经常开关机都要去路由器上查看IP地址，很麻烦，因此这种情况配置静态IP就很方便

1. 打开文件

   ```bash
   vim /etc/dhcpcd.conf
   ```

2. 增加配置

   ```bash
   # 指定接口 eth0
   interface eth0
   # 指定静态IP，/24表示子网掩码为 255.255.255.0
   static ip_address=192.168.1.20/24
   # 路由器/网关IP地址
   static routers=192.168.1.1
   # 手动自定义DNS服务器
   static domain_name_servers=114.114.114.114
   ```

3. 重启

   ```bash
   reboot
   ```

   

