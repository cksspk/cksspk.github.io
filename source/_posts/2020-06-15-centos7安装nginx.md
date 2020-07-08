---
title: centos7安装nginx
top: false
cover: false
toc: true
date: 2020-06-15 16:54:32
password:
summary:
tags: [linux]
categories: [软件安装]
---

`centos7`安装`nginx`

<!-- more -->

# cenots7安装nginx

#### 安装依赖

1. gcc

   ```bash
   yum -y install gcc
   ```

2. pcre、pcre-devel安装

   ```bash
     yum install -y pcre pcre-devel
   ```

3. zlib安装

   ```bash
   yum install -y zlib zlib-devel
   ```

4. 安装openssl

   ```bash
   yum install -y openssl openssl-devel
   ```

#### 文件操作

- **上传服务器** [资源地址：nginx-1.10.0](https://pan.baidu.com/s/1GPt4McwvCjMB7fkQaO8Djg)

  ```bash
  nginx-1.10.0.tar.gz
  ```

- **解压**

  ```bash
  tar -zxvf nginx-1.10.0.tar.gz
  ```

- **配置**

  ```bash
  # 进入nginx目录
  cd nginx-1.10.0.tar.gz
  # 配置
  ./configure --prefix=/opt/nginx --sbin-path=/usr/bin/nginx
  # 编译安装
  make && make install
  ```

- **操作**

  nginx可以通过以下指令启动，停止

  - 启动 `nginx`
  - 停止 `nginx -s stop`
  - 重新加载 `nginx -s reload`

### 配置开机自启

 **第一步：进入到/lib/systemd/system/目录**

```bash
cd /lib/systemd/system/
```

**第二步：创建nginx.service文件，并编辑**

```bash
vim nginx.service
# 内容如下
[Unit]
Description=nginx service
After=network.target 
   
[Service] 
Type=forking 
ExecStart=/usr/bin/nginx
ExecReload=/usr/bin/nginx -s reload
ExecStop=/usr/bin/nginx -s quit
PrivateTmp=true 
   
[Install] 
WantedBy=multi-user.target
```

**第三步：加入开机自启动**

- 启动开机自启

  ```bash
  systemctl enable nginx
  ```

- 启动

  ```bash
  systemctl start nginx
  ```

- 查看状态

  ```bash
  systemctl status nginx
  ```

- 取消开机自启

  ```bash
  systemctl disable nginx
  ```

  