---
title: 基本软件安装
p: raspberryPi/softwareInstall
date: 2020-03-15 13:43:57
tags: 软件安装
category: raspberryPi
---

### JAVA环境（jdk1.8）

1. #### 下载jdk

   1. 进入java[官网](https://www.oracle.com/java/technologies/javase-jdk8-downloads.html)
   2. 选择 `Linux ARM 32 Hard Float ABI` 版本下载
   3. 通过`xshell`软件上传至树莓派

2. #### 解压配置环境

   1. 在软件目录解压

      ```bash
      tar -zxvf jdk-8u241-linux-arm32-vfp-hflt.tar.gz
      ```

   2. 编辑配置文件

      ```bash
      vim /etc/profile
      ```

   3. 在后面写入

      ```
      export JAVA_HOME=/opt/java/jdk1.8.0_241
      export PATH=$JAVA_HOME/bin:$PATH
      export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
      ```

   #### 3.保存验证

   1. 修改文件后立即生效

      ```bash
      source /etc/profile
      ```

   2. 验证，输入 `java -version`，查看打印输出

      ```bash
      java -version
      ```

      ![验证](softwareInstall/yanzheng.jpg)

### 安装SVN服务器

##### 服务器配置

1. 安装svn服务器

   ```bash
   sudo apt-get install subversion -y
   ```

2. 创建仓库, 使用/home/pi/svn/svn_ckss作为仓库根路径

   ```bash
   svnadmin create /home/pi/svn/svn_ckss
   ```

3. 修改配置文件

   ```bash
   vim /home/pi/svn/svn_ckss/conf/svnserve.conf 
   ```

   ![验证](softwareInstall/svn_1.png)

   ​	**去掉注释，一定要删除空格！！！ 让配置顶格写！！！**

4. 配置用户信息

   ```bash
   vim /home/pi/svn/svn_ckss/conf/passwd 
   ```

   ![验证](softwareInstall/svn_2.jpg)

5. 修改权限配置

   ```bash
   vim /home/pi/svn/svn_ckss/conf/authz 
   ```

   ![验证](softwareInstall/svn_3.png)

6. 启动服务

   ```bash
   svnserve -d -r /home/pi/svn/svn_ckss
   ```

7. 查看启动

   ```bash
   ps -ef | grep svnserve
   ```

   启动成功

   ```bash
   root     19497     1  0 12:54 ?        00:00:00 svnserve -d -r /home/pi/svn/svn_taoge/
   root     19530 19172  0 13:13 pts/0    00:00:00 grep svnserve
   ```

   

8. 停止服务

   ```
   killall svnserve
   ```

如果使用防火墙需要将3690端口开放

##### 客户端配置

客户端通过***tortoiseSVN***进行连接，下载地址 https://tortoisesvn.net/downloads.html

另外，中文语言包需要通版本保持一致

输入SVN服务器地址 :svn://192.168.0.108进行检出



![验证](softwareInstall/svn_4.png)

