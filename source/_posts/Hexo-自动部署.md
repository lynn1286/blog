---
title: Hexo 自动部署
date: 2020-09-02 10:41:18
categories: Hexo
tags: 博客搭建
---

# 前言

最近Git Pages 好像因为被墙的原因导致网速感人，遂决定重新部署个人博客到阿里云服务器，因为菜所以磕磕碰碰 ， 记录下 ，为以后健忘症的我埋下点东西，以后方便查阅。

<br />

# 物料准备
- 一台服务器。（作者使用的是阿里云服务器）

- 一台电脑。（作者使用的是 Mac Pro）

- 带上耳机开始撸。

<br />

# 什么是Hexo

Hexo是一个快速、简洁且高效的博客框架。

Hexo使用Markdown解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

它的官网地址是： https://hexo.io/zh-cn/

<br />

# 搭建本地Hexo环境

安装Node：
进入Node官网下载安装，各位小伙伴这个我就不细说了吧。

Npm：
只要你安装了Node ，那么 npm 也会安装好的。

验证是否安装成功，打开终端输入：
```bash
  node --version
  npm --version
```

如果命令行有输出版本那么恭喜小伙伴安装成功，如果输出了： 
```bash
  command not found: node
  command not found: npm
```

可能是环境没变量没有配置好 或者安装出了问题，因为这不是本文重点所以请各位小伙伴自行搜索解决，实在不行可以私聊作者协助解决。

安装git（mac 系统自带 git ，所以不需要安装）:
进入Git官网下载安装，这个我也不细说了。
完成安装后，进入Hexo官网查看安装命令。
```bash
  npm install hexo-cli -g
```
意思就是要我们全局安装 hexo-cli ， 终端执行安装。
当然你不想全局安装 hexo文档也有局部安装，具体查看Hexo文档。
安装好hexo脚手架之后，我们利用它来初始化一个项目。
```bash
  hexo init <文件夹名称>
```
等hexo初始化好项目之后，我们使用vscode打开项目，在根目录下打开终端执行：
```bash
  npm install
```
安装好项目的各种依赖之后呢，就可以预览我们的项目了，执行：
```bash
  hexo s
```
到这里，我们本地的环境 已经搭建完毕并且也准备了一个项目。

# 搭建服务器环境
作者这里使用的是阿里云服务器，本地ssh 远程登录到阿里云服务器之后，安装以下软件：
1. Nginx
2. Git

那么我们先来安装 Git：
```bash
  yum install git 
```
安装之后 ，命令  `git --version` 检查是否安装成功。
接下来创建 git 用户， 注意 这里是创建 系统用户， 名字叫 git ，当然 你也可以取其他的名称，创建这个用户主要是用来 实现自动化部署的， 虽然服务器就只有我一个人使用好像多此一举，但是 装逼下没啥大问题。
```bash
  sudo adduser git 
```
我们现在创建了一个 git 用户， 接下来的操作都跟它有关， 但是到目前为止我们先不管它，当然你也可以先跳过这一步骤。
接着往下， 我们的博客文件肯定需要有个地方来存放没毛病吧，作者选择在 `/home` 目录下创建存放博客的目录，地址为 `/home/www/hexo`  。
```bash
  mkdir -p /home/www/hexo
```
# 安装Nginx

注：以下步骤都在 服务器上操作的哦。

准备好这些之后，我们要搞点复杂的东西了，安装配置nginx：
安装 EPEL 源：
```bash
  sudo yum install -y epel-release 
```
更新yum：
```bash
  sudo yum -y update 
```
接着安装 Nginx：
```bash
  sudo yum install -y nginx
```
安装成功后，默认的网站目录为：`/usr/share/nginx/html`

默认的配置文件为：`/etc/nginx/nginx.conf`

自定义配置文件目录为: `/etc/nginx/conf.d/`

如果你的服务器打开了防火墙，你需要运行下面的命令，打开80和443端口。
```bash
  sudo firewall-cmd --permanent --zone=public --add-service=http
  sudo firewall-cmd --permanent --zone=public --add-service=https
  sudo firewall-cmd --reload
```
启动Nginx：
```bash
  systemctl start nginx
```
开机启动Nginx：
```bash
  systemctl enable nginx
```
校验Nginx是否安装成功，你可以打开你的公网地址进行访问，看看能否访问到nginx的默认网页，如果不行 ，抱歉 ，自行搜下解决办法 或者联系作者协助安装。

# 配置Nginx
作者还配置了https ， 所以这里再讲一下如何配置免费的 https 。

安装Https免费证书(以阿里云域名为例)

一键安装acme.sh：
```bash
  curl https://get.acme.sh | sh # 安装
  echo 'alias acme.sh=~/.acme.sh/acme.sh' >> ~/.bashrc # 环境变量
  source ~/.bashrc # 更新生效配置
```

生成Https证书：
```bash
  export Ali_Key="**********"  # 在阿里云 AccessKey 管理页面获取 
  export Ali_Secret="**********"  # 在阿里云 AccessKey 管理页面获取
  acme.sh --issue --dns dns_ali -d domain # domain 是你的域名，没有d的话使用公网ip就可以
```

关于 Ali_key 和 Ali_Secret 的获取：

点击阿里云后台，右上角用户头像，菜单中选择accesskeys。
查看AccessKey ID 和 Access Key Secret，即对应上面的Ali_Key 和 Ali_Secret。

domain 对应该账户下购买的阿里云域名。

拷贝证书到nginx：
```bash
  mkdir -p /etc/nginx/ssl # 先创建 ssl 文件夹用来存放 https 的 key 等文件

  # domain 是你的域名 没有的话使用 ip 也可以
  acme.sh --install-cert -d domain \
  --key-file       /etc/nginx/ssl/domain.key  \
  --fullchain-file /etc/nginx/ssl/domain.cer \
  --reloadcmd     "service nginx force-reload" 
```

https证书拷贝是否成功 ， 可以进入 /etc/nginx/ssl 文件夹 查看是否有文件存在 。

目前https证书在 60 天以后会自动更新, 你无需任何操作. 今后有可能会缩短这个时间, 不过都是自动的, 你不用关心.

接着 删除或者注释 `/etc/nginx/nginx.conf` 中的server部分代码。

然后在 `/etc/nginx/conf.d` 创建自定义配置文件 `default.conf` ，写入以下内容：
```bash
  server {
      listen 80;
      listen 443 ssl;
      server_name  chalicelee.xin www.chalicelee.xin; # 改成 你自己的域名
  ​
      location / {
          root   /home/www/hexo; # 如果你的存放地址跟我一致则不需要改动
          index  index.html index.htm;
      }
  ​
      ssl on;
      ssl_certificate /etc/nginx/ssl/chalicelee.xin.cer; # https 的 cer 文件
      ssl_certificate_key /etc/nginx/ssl/chalicelee.xin.key; # https 的 key 文件
      ssl_session_timeout  5m;
      ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
      ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4:!DH:!DHE;
      ssl_prefer_server_ciphers  on;
  ​
      error_page 497  https://$host$uri?$args; # 强制http跳转到https
  }
```

至此 ， Nginx 配置完毕，接下来我们要部署我们的 hexo 博客了。

# 配置 git 用户

还记得前面我们创建了一个系统用户 git 吧 ， 接下来我们要配置下它：

修改git用户的权限：
```bash
  chmod 740 /etc/sudoers
```
然后打开文件 ： `/etc/sudoers` ， 增加 git 用户 。

![git](../img/WX20200901-162034.png)


修改后记得把权限改回来哦。
```bash
  chmod 400 /etc/sudoers
```
设置git用户密码 （可选）
```bash
  sudo passwd git
```
然后在终端输入你的密码。

至此 完成了 Git 用户的创建，接下来我们向 Git 用户添加公钥，就像配置 Github 那样。

# 配置 ssh 公钥

我们拉取代码的时候有配置过仓库的ssh 实现免密码登录即可拉取仓库代码的经历吧，这一步其实是一样的原理。

总体思路：
1. 先在你本地生成 密钥对 。

2. 然后把生成的公钥上传到服务器。

3. 把上传的公钥内容 copy 到 authorized_keys 文件中。

接下来我们开始吧。

本地生成密钥对

各位小伙伴注意：这一步操作是在本地 进行的哦。

本地打开你的终端 ， 输入命令：
```bash
  cd ～ # 进入根目录
  cd .ssh # 进入 .ssh 目录
  ssh-keygen # 生成密钥对
```
相信如果有配置过ssh的同学 ，都应该会有 `.ssh` 目录存在 ，它是个隐藏目录 ，所以想要发现它 ，必须 加入 `ls -a` 。

如果你发现你没有这个目录，也别紧张 ，新建`.ssh`目录就好了。

密钥对生成之后 为私钥设置权限：
```bash
  chmod 700 ~/.ssh
  chmod 600 ~/.ssh/id_rsa 
```
至此 ，密钥对生成完毕。

上传公钥至服务器

各位小伙伴注意：这一步是在服务器 终端操作的。

mac 可以使用 `scp` 命令 上传服务器，各位小伙伴实在觉得这一步有点麻烦，那么可以试一下本地打开 `.ssh` 目录下的 `id_rsa.pub` 文件 ， 把它的内容 `copy` 过来，然后 阿里云远程登录到服务器，然后 切换到 git 用户。
```bash
  su git # 切换到git 用户
  cd ~ # 进入根目录
  mkdir .ssh  # 如果没有这个文件夹则创建 如果有则忽略这个命令
```
创建好 `.ssh` 文件夹后 ，进入到这个文件夹 ， 创建 `authorized_keys` 文件 ，并把 刚刚你`copy`的公钥内容粘贴进去保存。

然后修改 文件权限：
```bash
  chmod 600 ~/.ssh/authorized_keys
  chmod 700 ~/.ssh
```

重启服务器 ，然后本地试下 ssh 连接是否能够成功。
```bash
  ssh -v git@xxx.xxx.xxx.xxx（你的公网 IP）
```
注意：因为作者 是使用了 `git` 名称 ，所以这里连接的时候也是 `git` ，假如你的是别的名称， 应该换成对应的。

如果上述操作失败，那么各位小伙伴就要根据对应的操作系统来把 本地生成的公钥 上传到 服务器 然后在服务器`copy`公钥的内容到 `authorized_keys` 文件内。

但是讲道理应该是可以成功的。

哈哈，感觉很不负责任的话。





好了，至此 如果你能够通过 ssh 本地连接远程的 git 用户了 ， 那么服务端公钥的配置也结束了。


# 服务器配置git仓库

各位小伙伴注意，以下操作为服务器操作 ， 并且不是 `root` 用户 而是 `git` 用户哦。

通过前面的步骤，我们已经可以通过本地 `ssh` 连接到我们的服务器 `git` 用户， 那么接着我们使用`git`用户配置我们的`git`仓库吧。（好怪，有点后悔随大众 取名 为 `git` 了。）


登录服务器，进行操作：
```bash
  cd ~ # 进入根目录
  git init --bare hexo.git  # 初始化git裸仓库
```

详细说一下使用 `--bare` 参数的含义，使用 `--bare` 参数初始化的仓库，我们一般称之为裸仓库， 因为这样创建的仓库并不包含 工作区 ，  也就是说，我们并不能在这个目录下执行我们一般使用的 Git 命令。

接着我们 创建并 写入 post-receive 钩子文件：
```bash
  vi ~/hexo.git/hooks/post-receive
```
写入内容如下：
```bash
  git --work-tree=/home/www/hexo --git-dir=/home/git/hexo.git checkout -f
```
注意 文件的地址要保持一致哦。

授予钩子文件可执行权限。
```bash
  chmod +x ~/hexo.git/hooks/post-receive
  cd ~
  sudo chmod -R 777 /home/www/hexo
```
重启服务器实例。
至此，我们就完成了服务端的配置



# 项目中的配置

哟吼，配置接近尾声 ， 不激动， 慢慢来。

注意： 这一步骤的所有操作都在本地 刚刚我们初始化的 项目中。

打开我们刚刚初始化的项目， 然后 打开 hexo 的 配置文件： _config.yml ：
```bash
  # Deployment
  ## 修改 这个字段 repo 是你的 服务器 存放目录
  deploy:
    type: 'git'
    repo: 'git@47.102.126.177:/home/git/hexo.git'
    branch: 'master'
```

哦 ， 除了这里之外 ，还需要配置 url 字段:
```bash
  # URL
  ##  填写你的域名 或者 公网 ip
  url: http://chalicelee.xin
```

# Hexo 自动部署

以上步骤完成 ，我们就来测试下是否成功部署，打开我们初始化的项目，根目录打开终端，输入命令：
```bash
  hexo new 文章名称
```
这样我们的`source`文件夹下的 `_posts` 文件夹下会自动生成 `文章名称.md` 。
我们随便写点东西吧。

写完之后 ，我们终端执行命令：
```bash
  hexo cl
  hexo g
  hexo d
```
一个命令一个命令执行后 ， 我们打开我们的域名或者ip ，看看能否访问到我们初始化的博客页面， 看看我们新增的文章是否有显示吧。

如果有， 恭喜你， 自动部署已经成功，之后我们写文章 ，就可以直接在本地写完之后， 通过上述三个 hexo 命令 ，就可以发布到我们的服务器上了。

# 配置hexo 主题

默认的主题好像不太喜欢。

没关系 ，我们可以换一个， 打开hexo 官网， 找到主题， 直接选择一个自己喜欢的点击进入github ，然后下载下来在 放到我们的 themes 目录下，然后配置下 _config.yml ：
```bash
  # Extensions
  ## Plugins: https://hexo.io/plugins/
  ## Themes: https://hexo.io/themes/
  theme: fluid  # 这个 fluid 就是主题名称 一般是你下载的主题文件夹名称 具体看主题文档 写入
```

更多的主题配置 你可以查看你所下载的主题文档， 或者 去 hexo 文档查看配置。

# 总结

上述配置看起来复杂其实还算是很简单的，整体流程下来也没有遇到什么坑，但是有一点疑惑的是 nginx 配置代理后 ，网页打开 url 后 会多了个 /? 的问题，不知道怎么回事，懂的大佬请赐教。

