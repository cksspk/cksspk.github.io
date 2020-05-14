---
title: 多台电脑使用Hexo
p: hexo/多电脑使用hexo
date: 2020-05-14 09:12:09
tags:
---

 &ensp;&ensp;一般使用hexo都是在一台电脑进行写作，但是有时候在公司，在家或者其他地方想要提交博客，原始方式就不方便，我们一般将hexo通过git部署，因此可以git特性实现同一仓库管理源文件和博客页面。
<!-- more -->

## 简单原理

先简单说说Hexo生产的静态博客的特点，首先它是一个静态博客生成工具，可以根据你的配置和md文件生成一系列的html、css、js等文件，组成一个站点，部署到github pages，这样网站就可以访问了。

```undefined
hexo d
```

hexo的部署命令，其实就是：

1. 生成站点有关文件到 `.deploy_git` 
2. 把它初始化为git目录，并根据你的配置指定remote和branch(一般是master)
3. 进行`git commit`，并把修改push到指定的remote branch
4. 命令执行完成后，到你的github仓库，你会发现master分支上的内容和'.deploy_git'中一样

## 步骤如下

1. 创建仓库 cksspk/cksspk.github.io

2. 根据提示将仓库拉取到本地，并且添加新的分支`hexo`

   ```bash
   #1. 拉取分支提交
   echo "# hello " >> READ.md
   git init
   git add .
   git commit -m "init"
   git remote add origin  https://github.com/cksspk/cksspk.github.io.git
   git push -u origin master
   
   #2. 创建新的分支
   git checkout -b hexo
   git push origin hexo
   ```

3. 设置默认分支

    在该仓库`->Settings->Branches->Default branch`中将默认分支 

4. 克隆`source`分支

   ```bash
   #1. 将新建的source分支克隆到本地
   # 使用ssh免密 git clone git@github.com:cksspk/cksspk.github.io.git
   git clone https://github.com/cksspk/cksspk.github.io.git
   #2. 进入该目录
   cd cksspk.github.io
   #3. 在Git Bash中查看分支,分支应该为 branch
   git branch
   ```

5. 上传部署文件

   - 把除了.git 文件夹外的所有文件都删掉

   - 将本地博客中除了  `.deploy_git `文件全部复制到 `cksspk.github.io`目录

   - 可以修改 ` .gitignore ` 用来忽略一些不需要的文件 ，如果没有的话，自己新建一个，在里面写上如下，表示这些类型文件不需要git：

     ```shell
     .DS_Store
     Thumbs.db
     db.json
     *.log
     node_modules/
     public/
     .deploy*/
     ```

      注意，如果你之前克隆过theme中的主题文件，那么应该把主题文件中的`.git`文件夹删掉，因为git不能嵌套上传 
     
   - 上传到远程

     ```bash
     git add . 
     git commit -m "add hexo"
     git push 
     ```

     

##  同步到其他电脑

- 在新电脑上克隆`username.github.io`仓库的`hexo`分支到本地，此时本地`git`仓库处于`hexo`分支
- 切换到`username.github.io`目录，执行`npm install`(由于仓库有一个.gitignore文件，里面默认是忽略掉 `node_modules`文件夹的，也就是说仓库的`hexo`分支并没有存储该目录，所以需要`install`下)如果`node_modules`文件没有丢失, 可不执行该操作
- 需要注意的是每次更新博客之后, 都要把相关修改上传到`hexo`分支
- 每次换电脑更新博客的时候, 在修改之前最好也要`git pull`拉取一下最新的更新

