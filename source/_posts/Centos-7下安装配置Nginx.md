---
title: Centos 7下安装配置Nginx
date: 2020-09-01 11:41:09
categories: 服务器
tags: 阿里云
---

本文基于Centos 7下安装配置Nginx操作实践记录整理。

# 一、配置 EPEL源

```bash
    sudo yum install -y epel-release
    sudo yum -y update
```

# 二、安装Nginx

```bash
    sudo yum install -y nginx
```

安装成功后，默认的网站目录为： /usr/share/nginx/html

默认的配置文件为：/etc/nginx/nginx.conf

自定义配置文件目录为: /etc/nginx/conf.d/

# 三、开启端口80和443

如果你的服务器打开了防火墙，你需要运行下面的命令，打开80和443端口。

```bash
    sudo firewall-cmd --permanent --zone=public --add-service=http
    sudo firewall-cmd --permanent --zone=public --add-service=https
    sudo firewall-cmd --reload
```

如果你的服务器是阿里云ECS，你还可以通过控制台安全组，打开80和443端口，或者其他自定义端口。

具体操作路径： 阿里云ECS服务器 -》网络与安全 -》 安全组 -》 配置规则 -》 安全组规则 -》 入方向 -》 添加安全组规则

端口范围： 比如你要打开80端口，这里就填写 80/80 。

优先级： 优先级可选范围为1-100，默认值为1，即最高优先级。

# 四、操作Nginx

nginx 操作命令备注：

```bash
    # 1.启动 Nginx
    systemctl start nginx

    # 2.停止Nginx
    systemctl stop nginx

    # 3.重启Nginx
    systemctl restart nginx

    # 4.查看Nginx状态
    systemctl status nginx

    # 5.启用开机启动Nginx
    systemctl enable nginx

    # 6.禁用开机启动Nginx
    systemctl disable nginx
```

# 五、配置Nginx

1. 安装Https免费证书(以阿里云域名为例)

一键安装acme.sh

```bash
    curl https://get.acme.sh | sh
    echo 'alias acme.sh=~/.acme.sh/acme.sh' >> ~/.bashrc
    source ~/.bashrc
```

生成Https证书

```bash
    export Ali_Key="**********"  
    export Ali_Secret="**********"  
    acme.sh --issue --dns dns_ali -d domain
```

其中： 

点击阿里云后台，右上角用户头像，菜单中选择accesskeys。

查看AccessKey ID 和 Access Key Secret，即对应上面的Ali_Key 和 Ali_Secret。

domain 对应该账户下购买的阿里云域名。

拷贝证书到nginx

```bash
    mkdir -p /etc/nginx/ssl

    acme.sh --install-cert -d domain \
    --key-file       /etc/nginx/ssl/domain.key  \
    --fullchain-file /etc/nginx/ssl/domain.cer \
    --reloadcmd     "service nginx force-reload"
```

https证书拷贝成功。

目前https证书在 60 天以后会自动更新, 你无需任何操作. 今后有可能会缩短这个时间, 不过都是自动的, 你不用关心.

2. 配置nginx

删除/etc/nginx/nginx.conf中的server部分代码。

```vim
    server {
        ...
    }
```

在/etc/nginx/conf.d 创建自定义配置文件default.conf

```vim
    server {
        listen 80;
        listen 443 ssl;
        server_name  domain www.domain;
        location / {
            root /usr/share/nginx/html;
            index  index.html index.htm;
        }

        ssl on;
        ssl_certificate /etc/nginx/ssl/domain.crt;
        ssl_certificate_key /etc/nginx/ssl/domain.key;
        ssl_session_timeout  5m;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4:!DH:!DHE;
        ssl_prefer_server_ciphers  on;

        error_page 497  https://$host$uri?$args;
    }
```

其中:
root /usr/share/nginx/html; 表示网站文件目录，后面的分号不可省略。

ssl_certificate和ssl_certificate_key 指向https证书。

error_page 497 https://hosturi?$args; 这句的作用是，强制http跳转到https。
