---
title: Hexo创建博客以及github搭建博客流程
date: 2020-09-01 11:32:20
categories: Hexo
tags: 博客搭建
---

## 使用hexo 脚手架搭建模板

### 1. 安装

既然要使用hexo， 那么肯定少不了hexo 的[官方文档](https://hexo.io/zh-cn/)。

安装hexo的前提（这个就不多废话啦，没有安装的点击进入安装就行）：

- [Node.js](http://nodejs.org/) (Should be at least nodejs 6.9)
- [Git](http://git-scm.com/)

好了，安装好之后，我们就开始全局安装hexo了：

    npm install -g hexo-cli

安装好之后，我们就开始建立我们的网站啦，接下来命令行输入：

    hexo init 项目名称
    cd 项目名称
    npm install

上面三个命令很简单啦，第一步就是初始化我们的项目，项目名称随便你取，初始化完毕之后，我们进入这个项目，或者你可以忽略第二部，利用编辑器打开你的项目进行依赖安装，生成的目录结构如下：

    node_modules: 依赖包
    scaffolds：生成文章的一些模板
    source：用来存放你的文章
    themes：主题
    _config.yml: 博客的配置文件

接下来再执行命令：

    hexo g
    hexo server

执行完毕后，控制台会输出端口4000的地址，打开我们就能看到博客了。

___

## 2. 配置

那么，接下来我们就是需要配置我们的网站：

我们打开_config.yml文件：

    ```yml
        # Hexo Configuration
        ## Docs: https://hexo.io/docs/configuration.html
        ## Source: https://github.com/hexojs/hexo/

        # Site  这里修改成你们的东西即可
        title: Chalice Lee  
        subtitle: 前端博客,JavaScript,html5,css3,Jquery,NodeJs
        description: 前端博客,JavaScript,html5,css3,Jquery,NodeJs
        keywords: 前端博客,JavaScript,html5,css3,Jquery,NodeJs
        author: Chalice Lee
        language: zh
        timezone:

        # URL
        ## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
        url: https://chalicelee92.github.io/ // 这里是网站url
        root: /  // 根目录
        permalink: :year/:month/:day/:title/
        permalink_defaults:

        # Directory
        source_dir: source
        public_dir: public
        tag_dir: tags
        archive_dir: archives
        category_dir: categories
        code_dir: downloads/code
        i18n_dir: :lang
        skip_render:

        # Writing
        new_post_name: :title.md # File name of new posts
        default_layout: post
        titlecase: false # Transform title into titlecase
        external_link: true # Open external links in new tab
        filename_case: 0
        render_drafts: false
        post_asset_folder: false
        relative_link: false
        future: true
        highlight:
        enable: true
        line_number: true
        auto_detect: false
        tab_replace:

        # Home page setting
        # path: Root path for your blogs index page. (default = '')
        # per_page: Posts displayed per page. (0 = disable pagination)
        # order_by: Posts order. (Order by date descending by default)
        index_generator:
        path: ''
        per_page: 10
        order_by: -date

        # Category & Tag
        default_category: uncategorized
        category_map:
        tag_map:

        # Date / Time format
        ## Hexo uses Moment.js to parse and display date
        ## You can customize the date format as defined in
        ## http://momentjs.com/docs/#/displaying/format/
        date_format: YYYY-MM-DD
        time_format: HH:mm:ss

        # Pagination
        ## Set per_page to 0 to disable pagination
        per_page: 10
        pagination_dir: page

        # Extensions
        ## Plugins: https://hexo.io/plugins/
        ## Themes: https://hexo.io/themes/
        theme: hexo-theme-aircloud-master   // 这里是更换主题

        # Deployment
        ## Docs: https://hexo.io/docs/deployment.html
        deploy:
        type: git
        repo: https://github.com/chalicelee92/chalicelee92.github.io.git
        branch: master
    ```

具体配置解释可以查看官网文档。

___

### 3. 主题更换

下面跟大家说下如何更换[主题](https://hexo.io/themes/)，进入官网的主题页面，选择自己喜欢的风格，进入仓库后，把代码 **clone** 下来，然后复制粘贴到 **theme** 这个文件夹里面，然后再 **_config.yml** 配置文件里面配置**theme**字段，指定你下载主题的文件夹名字然后重启服务器，就可以了。

___

### 4. 书写文章

到此为止，我们的网站基本可以看到啦，接下来就是说说，如何写文章啦，命令行输入：

    hexo new [layout] <title>

- layout ：默认为_posts

- title: 要创建的文章名字

然后你会发现，在/source/_posts下面，多了你创建的文件，然后，我们就可以愉快的书写你的博客日志啦。

书写完毕后输入命令：

    hexo clean
    hexo g
    hexo d

最后给大家案例一篇文章：本人小白技术，看不懂的可以移步到：

<https://blog.csdn.net/sinat_37781304/article/details/82729029>

___

### 主题使用详细介绍之 aircloud 主题的使用

接下来简单的说下如何使用Hexo主题(每个主题都有不同的配置,具体看作者的教程配置)：

选择好自己喜欢的Hexo 主题之后，把主题给clone 下来，然后复制粘贴到theme文件夹内，这个很简单，也就不多讲了。

那么接下来我们是要配置这个主题的内容啊，就比如aircloud这个主题，配置文件则是如下：

    ```json
        # Hexo Configuration
        ## Docs: https://hexo.io/docs/configuration.html
        ## Source: https://github.com/hexojs/hexo/

        # Site
        title: Chalice Lee
        subtitle: 前端博客,JavaScript,html5,css3,Jquery,NodeJs
        description: 前端博客,JavaScript,html5,css3,Jquery,NodeJs
        keywords: 前端博客,JavaScript,html5,css3,Jquery,NodeJs
        author: Chalice Lee
        language: zh
        timezone:

        # Site settings
        # 网站综合内容设置：
        # SEOTitle: Chalice Lee的博客 | Chalice Lee Blog
        # email: ChaliceLee92@163.com
        # description: "Chalice Lee的博客"
        # keyword: "前端博客"

        # URL
        ## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
        url: https://chalicelee92.github.io/
        root: /
        permalink: :year/:month/:day/:title/
        permalink_defaults:

        # Directory
        source_dir: source
        public_dir: public
        tag_dir: tags
        archive_dir: archives
        category_dir: categories
        code_dir: downloads/code
        i18n_dir: :lang
        skip_render:

        # Writing
        new_post_name: :title.md # File name of new posts
        default_layout: post
        titlecase: false # Transform title into titlecase
        external_link: true # Open external links in new tab
        filename_case: 0
        render_drafts: false
        post_asset_folder: false
        relative_link: false
        future: true
        highlight:
        enable: true
        line_number: true
        auto_detect: false
        tab_replace:

        # Home page setting
        # path: Root path for your blogs index page. (default = '')
        # per_page: Posts displayed per page. (0 = disable pagination)
        # order_by: Posts order. (Order by date descending by default)
        index_generator:
        path: ''
        per_page: 10
        order_by: -date

        # Category & Tag
        default_category: uncategorized
        category_map:
        tag_map:

        # Date / Time format
        ## Hexo uses Moment.js to parse and display date
        ## You can customize the date format as defined in
        ## http://momentjs.com/docs/#/displaying/format/
        date_format: YYYY-MM-DD
        time_format: HH:mm:ss

        # Pagination
        ## Set per_page to 0 to disable pagination
        per_page: 10
        pagination_dir: page

        # Extensions
        ## Plugins: https://hexo.io/plugins/
        ## Themes: https://hexo.io/themes/
        theme: hexo-theme-aircloud-master

        # Deployment
        ## Docs: https://hexo.io/docs/deployment.html
        deploy:
        type: git
        repo: https://github.com/chalicelee92/chalicelee92.github.io.git
        branch: master

        sidebar-avatar: img/avatar.png

        # SNS settings  这里配置后，则会在网站底部出现对应的社交图标
        # 一些社交平台地址，支持以下几种：
        # weibo_username:
        # zhihu_username:
        github_username:    ChaliceLee92
        # twitter_username:
        # facebook_username:  
        # linkedin_username:  

        # Friends
        # 友情链接
        # friends: [
        #     {
        #         title: "",
        #         href: ""
        #     },{
        #         title: "",
        #         href: ""
        #     },{
        #         title: "",
        #         href: ""
        #     }
        # ]

        # Build settings
        anchorjs: true                          # if you want to customize anchor. check out line:181 of `post.html`

        #comment:
        #  type: gitment
        #  id: your-id-created-by-https://github.com/settings/applications/new
        #  secret: your-secret-created-by-https://github.com/settings/applications/new
        #  owner: aircloud
        #  repo: hexo-aircloud-blog

        # 赞赏功能
        donate:
        img: img/WechatIMG181.jpeg
        content: 您的赞赏是对我最大的鼓励

        # 文章样式(是否首行缩进)：
        post_style:
        indent: default

        # 头像圆角
        avatar_style:
        radius: true

        # 搜索功能
        search:
        path: search.json
        field: post
    ```

具体可以查看主题包的Readme , 都会有详细的使用说明的。

___

### github 搭建博客

首先在GitHub上创建一个仓库，仓库的名字应该要对应你的GitHub用户名，例如我的仓库名则设置为：

ChaliceLee92.github.io

后缀是.github.io , 接下来就简单了，创建好仓库之后，把仓库关联你本地的项目，然后再提交到仓库主分支，过了一段时间，则可以访问你的GitHub仓库地址了。

访问地址为：<https://chalicelee92.github.io/>

___

### 备忘录

然后书写文章，文章书写完毕后打包：

    hexo new 文章名称 // 创建文章

    hexo clean // 清除之前的打包
    hexo g  // 打包
    hexo serve // 启动项目


    hexo clean // 清除之前的打包
    hexo g // 打包
    hexo d // 发布到GitHub
