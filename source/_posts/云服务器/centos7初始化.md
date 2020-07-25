---
title: centos7初始化配置
top: false
cover: false
toc: true
mathjax: true
p: 云服务器/centos7初始化
date: 2020-05-29 15:17:25
password:
summary:
tags: [云服务器]
categories: linux
---

​	`centos7`初始化配置。

<!-- more -->

## 添加用户

```bash
#1. 新建用户
adduser username
#2. 设置密码
passwd username

```

## 添加sudu权限

```bash
#1 找到sudoers文件夹
whereis sudoers
#2 打开文件并编辑
vim /etc/sudoers
#3 找到root    ALL=(ALL)    ALL一行 在下面添加新增的用户按照root的格式
username    ALL=(ALL)    ALL
#4 保存退出
:wq!
#5最后一步 刷新权限
systemctl restart sshd
```

## 禁止root远程登录

```bash
#找到sshd_config文件
whereis ssh
#编辑文件 
vim /etc/ssh/sshd_config
//找到PermitRootLogin yes一行 将yes修改为no
PermitRootLogin no
#保存退出
:wq!
#刷新权限
systemctl restart sshd
#此时root用户已经无法登录
```

## fail2ban防暴力破解

> fail2ban可以监视你的系统日志，然后匹配日志的错误信息（正则式匹配）执行相应的屏蔽动作（一般情况下是防火墙），而且可以发送e-mail通知系统管理员！
>
> fail2ban运行机制：简单来说其功能就是防止暴力破解。工作的原理是通过分析一定时间内的相关服务日志，将满足动作的相关IP利用iptables加入到dorp（丢弃）列表一定时间。
>
> 官方地址： http://www.fail2ban.org/wiki/index.php/Main_Page

- 安装

  ```
  yum install shorewall gamin-python shorewall-shell shorewall-perl shorewall-common python-inotify python-ctypes fail2ban
  ```

- 配置`fail2ban`

  ```bash
  #1. 编辑配置文件 jail.local  并实现防暴力破解　
  vim /etc/fail2ban/jail.d/jail.local
  ```

  文件内容

  ```bash
  #defalut这里是设定全局设置，如果下面的监控没有设置就以全局设置的值设置。
  [DEFAULT]
  # 用于指定哪些地址ip可以忽略 fail2ban 防御,以空格间隔。
  ignoreip = 127.0.0.1/8
  # 客户端主机被禁止的时长（默认单位为秒）
  bantime  = 3600
  # 过滤的时长（秒）
  findtime  = 600
  # 匹配到的阈值（次数）
  maxretry = 3
  
  
  [ssh-iptables]
  # 是否开启
  enabled  = true
  # 过滤规则
  filter   = sshd
  # 动作
  action   = iptables[name=SSH, port=ssh, protocol=tcp]
  # 日志文件的路径
  logpath  = /var/log/secure
  # 匹配到的阈值（次数）
  maxretry = 3
  ```

   在这里需要注意一点就是：我们上面的action设置的时候，port=ssh，如果我们更改了sshd服务的端口号，我能需要在这里设置对应的端口号，否则配置不生效。 

- 启动 暂停 停止

  ```
  systemctl   start/restart/stop/status   fail2ban
  ```

- 防暴力破解测试

  - 测试

    ```bash
    #1. 首先要让配置文件生效
    fail2ban-client reload
    ```

    **测试：故意输入错误密码3次，再进行登录时，会拒绝登录**

    **警告：测试前注意修改被禁时长！！！！上面配置文件中时间为1个小时！！！！**

    ```bash
    [root@121~]# ssh 192.168.1.121
    root@192.168.1.121's password: 
    Permission denied, please try again.
    root@192.168.1.121's password: 
    Permission denied, please try again.
    root@192.168.1.121's password: 
    Permission denied (publickey,password).
    [root@121~]# ssh 192.168.1.121
    ssh: connect to host 192.168.1.121 port 22: Connection refused
    ```

  - 查看当前被禁止登陆的ip 

    ```bash
    [root@121]# fail2ban-client status ssh-iptables 
    Status for the jail: ssh-iptables
    |- filter
    |  |- File list:    /var/log/secure     #日志文件路径
    |  |- Currently failed:    0        #当前失败次数
    |  `- Total failed:    3            #总失败次数
    `- action
       |- Currently banned:    1        #当前禁止的ip数量
       |  `- IP list:    192.168.1.112        #当前禁止的ip
       `- Total banned:    1        #禁止的ip总数
    ```

  - 查看当前使用配置文件

    ```bash
    #1. 查看配置文件列表
    [root@VM_0_16_centos jail.d]$ sudo fail2ban-client status
    Status
    |- Number of jail:	1
    `- Jail list:	ssh-iptables
    #2. 查看指定配置文件情况 与上面相同
    sudo fail2ban-client status ssh-iptables 
    ```

    