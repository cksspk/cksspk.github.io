---
title: Hexo Next主题相关设置
categories: 
- Hexo
---
在Hexo中有两个很重要的名为`_config.yml`的文件，其中一个在站点安装的根目录下，另一个在主题目录下。

前者提供了Hexo自身的一些基本配置信息，后者为你所安装的主题的相关配置。

为了方便区分，我们把前者称为**站点配置文件**，后者称为**主题配置文件**。



### 使用Next主题

- 在博客根目录执行`git命令`

  ```bash
  git clone https://github.com/iissnan/hexo-theme-next themes/next
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
    language: zh-Hansyaml
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
  ```
  
  ### 设置首页预览和阅读全文
  
- 编辑``主题配置文件`	
  
  - 修改`auto_excerpt`的配置
  
    ```yaml
    auto_excerpt:
      enable: true
      length: 300
  ```
  
  ### 添加本地搜索
  
  - 在博客根目录下进入`cmd`中执行命令
  
    ```
    npm install hexo-generator-searchdb --save
    ```
  
  - 编辑`站点配置文件`，添加以下配置
  
    ```yaml
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
  