---
title: Hexo初始化及 Next主题配置
date: 2019-10-27 23:23:48
categories: 
- Hexo
---




hexo初始化安装及主题配置

<!-- more -->

在Hexo中有两个很重要的名为`_config.yml`的文件，其中一个在站点安装的根目录下，另一个在主题目录下。

前者提供了Hexo自身的一些基本配置信息，后者为你所安装的主题的相关配置。

为了方便区分，我们把前者称为**站点配置文件**，后者称为**主题配置文件**。

Hexo的版本设置有差异，本次使用版本`hexo: 5.3.0`，`hexo-cli: 4.2.0`，next主题版本为`7.8.0`

### 安装Hexo

- 安装hexo-cli

  ```shell
  npm install hexo-cli -g
  ```

- 使用vscode打开，执行

  ```shell
  #1. 初始化
  hexo init
  #2. 更新依赖 
  npm install
  #3. 运行
  hexo s
  ```

### 使用Next主题

- 在博客根目录执行`git命令`

  ```bash
  git clone https://github.com/theme-next/hexo-theme-next themes/next
  ```

- 修改`主题配置文件`

  ```yaml
  theme: next
  ```

  ### 修改语言

  - 编辑`站点配置文件`，将`language`设置成所需要的语言

  - 具体支持语言可以查看 [官方说明](http://theme-next.iissnan.com/getting-started.html#select-language)

  - 例如设置成简体中文

    ```yaml
    language: zh-CN
    ```

  ### 修改布局

  - 编辑`主题配置文件`

  - 修改 `Schemes`配置

    ```yaml
    # Schemes 四种布局
    # scheme: Muse
    # scheme: Mist
    # scheme: Pisces
    scheme: Gemini
    ```

  
### 添加标签页

- 在博客根目录进入`cmd`执行命令
  
  ```yaml
    hexo new page tags
  ```
  
  - 编辑`source/tags/index.md`文件
  
    ```yaml
    title: 标签
    date: 2019-06-30 02:59:16
    type: tags
    ```
  
  ### 添加分类
  
- 在博客根目录下进入`cmd`执行命令
  
  ```
    hexo new page categories
  ```
  
- 编辑`source/categories/index.md`文件
  
  ```
    title: 分类
  date: 2019-06-30 03:01:37
    type: "categories"
  ```

    
  
  ### 设置菜单

  - 编辑`主题配置文件`

  - 修改`menu`配置

    ```yaml
    home: / || home	首页
      #about: /about/ || user	关于
    tags: /tags/ || tags	标签页
      categories: /categories/ || th	分类页
    archives: /archives/ || archive	归档页
      # schedule: /schedule/ || calendar
    #sitemap: /sitemap.xml || sitemap
      #commonweal: /404/ || heartbeat
  
    ```
  
  ### 设置头像
  
  - 编辑`主题配置文件`,修改`avatar`字段，设置头像的地址
  
  - 头像地址有两种

    - 完整的互联网URL

    - 站点下的地址

      - 可以是`source/uploads`/目录下
    - 或者是`source/images/`目录下
  
      ```yaml
      avatar: /uploads/kenan.jpg
      ```
  
  
  

  
### 设置网站Favicon图标

  - 编辑`主题配置文件`
  
- 首先选择想要设置的图片通过favicon转换网站制作，如：[比特虫](http://www.bitbug.net/)，制作一个32 * 32.icon
  
- 将图片改名为：favicon.ico, 放到/themes/next/source/images目录下
  
  - 修改`favicon`配置：
  
    ```yml
    favicon:
      small: /images/favicon-16x16-next.png
      medium: /images/favicon.ico 		#修改这一行
    apple_touch_icon: /images/apple-touch-icon-next.png
      safari_pinned_tab: /images/logo.svg
    ```
  

###  添加本地搜索

- 在博客根目录下进入`cmd`中执行命令

```shell
# 1. 下载依赖
npm install hexo-generator-searchdb --save
```
- 编辑`站点配置文件`，添加以下配置

```yml
  search:
    path: search.xml
    field: post
    format: html
    limit: 10000
```

- 编辑`主题配置文件`，修改`local_search`属性，将false修改为true


```yaml
local_search:
  enable: true
```


### 生成新的文章

- -p：表示path 路径

- hexo/hexo：表示在`_posts`目录下生成 `hexo/hexo.md`文件（有一层文件夹）

```bash
hexo new page -p hexo/hexo
```

- ### 按日期创建文件夹

  - 修改配置文件`_config.yml` 下的 `new_post_name`

  - 通过日期来管理文章组合

    | 变量     | 描述                               |
    | -------- | ---------------------------------- |
    | :title   | 标题（小写，空格将会被替换为短杠） |
    | :year    | 建立的年份，比如， 2015            |
    | :month   | 建立的月份（有前导零），比如， 04  |
    | :i_month | 建立的月份（无前导零），比如， 4   |
    | :day     | 建立的日期（有前导零），比如， 07  |
    | :i_day   | 建立的日期（无前导零），比如， 7   |

  - 参考配置

    ```yml
    new_post_name: :year/:month/:title.md 
    ```

  - 创建新的文章

    ```bash
    hexo n hexo
    ```

## 文章分类

​	`categoryies`这个选项有两种配置

- **子分类**  

  下面的分类配置会将该文章放到 `Sports/Baseball` 这个分类下。

  ```bash
  # 方式一
  categories:
    - Sports
    - Baseball
  # 方式二
  categories: [Sports,Baseball]
  ```

  上面两种写法最终的效果都是一样的，都是将文章放在了一个子分类目录下。

- **多个分类**

  将文章同时分到两个或者多个不同的类目下

  ```bash
  categories:
    - [Sports]
    - [Baseball]
  ```

  只需要用中括号将独立的分类括起来即可，这样上面的文章就会被分类在 `Sports` 和 `Baseball` 这两个不同的目录中了。

  扩展一下，如果我们要将其分类到 `Sports/Baseball` 和 `Play` 两个不同的目录下（一个是子目录，一个是一级目录），我们该怎么写呢？如下：

  ```bash
  categories:
    - [Sports,Baseball]
    - [Play]
  ```

## 文章标签

​	将文章添加多个 `tag`，这里也有两种写法

  ```bash
# 方式一
tags: [jQuery, 表格， 表单验证]
# 方式二
tags:
- jQuery
- 表格
- 表单验证
  ```



### 设置文章摘要

​	在内容中插入一行

```bash
 <!-- more -->
```

![](hexo使用文章使用图片/1-1.png)

​	效果如下

​	![](hexo使用文章使用图片/1-2.png)

### 生成本地生成静态文件：`hexo g`

### 将本地静态文件推送至Github：`hexo d`

### 在文章中使用图片

- 在hexo根目录配置文件`_config.yml`设置属性

  ```
   post_asset_folder:true
  ```

- 在hexo的目录下执行

  ```
  npm install https://github.com/CodeFalling/hexo-asset-image --save   
  ```

- 使用图片

  ```
  ![logo](hexo使用文章使用图片/a.jpg)
  ```
 ![logo](hexo使用文章使用图片/a.jpg)
  