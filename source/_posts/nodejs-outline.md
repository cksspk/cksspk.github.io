---
title: nodeJs离线环境下安装配置
date: 2019-06-26 01:21:05
tags: 
- nodejs
categories: [nodeJs]
---

# 一、引言

这段时间由于工作原因要使用到openlayers，然后去网上找资料，发现一个神奇的东西， [gitbook](https://www.gitbook.com/)（有可能链接点不开，qiang你懂得）。*GitBook 是一个基于 Node.js 的命令行工具，可使用 Github/Git 和 Markdown 来制作精美的电子书，GitBook 并非关于 Git的教程   ————百度百科 。* 然后我就心痒痒，想在公司（内网）使用，于是开始了离线环境下搭建之旅。

# 二、安装配置

1. 首先下载安装NodeJs [官网](https://nodejs.org/en/)、[百度云【提取码：emrm】](https://pan.baidu.com/s/1XRyaezEQJ9DcYfmO2nLE7A)
2. 配置模块全局安装路径，方便找到位置，而且模块文件非常之多，放在c盘肯定不好

 先查看路径

```
npm config ls
```
设置全局模块路径

```
npm config set prefix"D:\Program Files\nodejs\node_global" 
```

 设置缓存路径

```
npm config set cache"D:\Program Files\nodejs\node_cache"
```


设置完之后可以同通过命令查看，配置信息

```
npm config ls
```


然后就可以愉快的通过命令下载模块，比如

```
npm install -g gitbook-cli
```


## **问题一：**

下载完之后通过gitbook -V命令提示：

**‘gitbook’不是内部或外部命令，也不是可运行的程序或批量文件**

此时需要配置环境变量，将新的全局模块路径添加到环境变量！！在path中追加

```
D:\Program Files\nodejs\node_global;
```

## 问题二：

在内网条件下安装nodeJs，并且将联网机中 node_global 及node_cache文件夹拷贝到开发机，通过上述命令设置好全局路径，通过gitbook -V命令显示 CLI version 2.3.2 ，然后卡住，报错，没有gitbook版本信息

解决方法：手动拷贝将**联网机c盘用户文件**下的**.gitbook**文件拷贝到**开发机c盘用户目录**。

# gitbook命令

安装完后可通过命令初始化一本书

```
gitbook init
```


发布书籍

```
gitbook serve
```


 具体还有很多有操作可以参考这[这篇文章](http://gitbook.zhangjikai.com/#前言)，里面很多gitbook相关信息及资源

# 总结：

通过此次发现nodeJs还是挺有有意思的，有非常多的模块，比如快速发布本地服务的http-server模块。而且它的文件形式比较容易理解。最后，在复制 node_global 文件夹的时候，windows或提示文件夹名称过长，这个时候需要用解压缩软件对文件夹压缩操作。。。这也算一个有点了坑的地方！！！！！