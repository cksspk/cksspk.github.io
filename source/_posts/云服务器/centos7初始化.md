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
#2 修改权限
chmod 777 /etc/sudoers
#3 打开文件并编辑
vim /etc/sudoers
#4 找到root    ALL=(ALL)    ALL一行 在下面添加新增的用户按照root的格式
username    ALL=(ALL)    ALL
#5 保存退出
:wq
#6第四步 修改权限为只读
chmod 444 /etc/sudoers
#7最后一步 刷新权限
systemctl restart sshd
```

## 禁止root远程登录

```bash
#找到sshd_config文件
whereis ssh
#获取最高权限
chmod 777 /etc/ssh/sshd_config
//编辑文件 找到PermitRootLogin yes一行 将yes修改为no
PermitRootLogin no
#保存退出
:wq
#修改权限为只读
chmod 444 /etc/ssh/sshd_config
#刷新权限
systemctl restart sshd
#此时root用户已经无法登录
```

