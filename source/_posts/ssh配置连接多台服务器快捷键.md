---
title: ssh配置连接多台服务器快捷键
date: 2020-09-01 11:46:53
categories: SSH
tags: 配置 config
---

创建 SSH 密钥对：

```bash
    # 生成密钥对 我们可以使用 ssh-keygen 命令来生成密钥对：
    ssh-keygen -t ecdsa -b 521 -C "$(whoami)@$(hostname)-$(date -I)"
```

其中可使用 -t 指定加密算法，使用 -b 自定生成密钥长度，使用 -C 添加密钥对的说明comment。生成的密钥对默认存储在用户目录下的 .ssh 目录中，私钥默认名称为 id_*** (即 id_ + 加密算法名称)。还可以使用 -f 指定生成的私钥存储的文件全路径名称；也可以不使用 -f 指定密钥文件路径，在密钥的创建过程中还会提示用户输入密钥文件全路径名称。私钥对应的公钥文件为私钥文件全名称 + .pub。

上面例子中创建了一对长度为512位的椭圆加密算法(ECDSA)加密的密钥对。创建 SSH 密钥对可选择多种加密算法，例如 RSA 、 DSA 、 ECDSA 等。

正常你的.ssh目录下会有以下文件：

```bash
    config      id_rsa     id_rsa.pub  known_hosts
```

即使没有config文件的话，你可以touch config文件。

内容如下：

```bash
    Host        dev
    HostName        111.111.111.111
    Port            22
    User            xx
    IdentityFile    ~/.ssh/id_rsa


    Host        pro1
    HostName        111.111.111.112
    Port            22
    User            xx
    IdentityFile    ~/.ssh/id_rsa
```

配置完事之后，你就可以在命令窗口，直接ssh dev，就可以直接连到服务器111.111.111.111上了，不用像以前那样去写
ssh [-l login_name] [-p port] [user@]hostname。

前提是你要打通本机与服务器的ssh。

另外再提一下：

使用Windows下的putty客户端可以直接使用.ppk密钥文件进行认证登录远程服务器。

Mac系统虽然自带了Terminal，但是Mac下的Terminal使用.pem文件，而不是.ppk文件，而.pem文件可以从.ppk文件转换而来。

一种方法是使用Windows下的puttygen进行转换。

另一种方法是在Mac系统下安装putty并进行转换，步骤如下：

1. 使用Homebrew安装putty(homebrew是Mac下的包管理工具)：

    ```bash
        brew install putty
    ```

    同时会安装puttygen。

2. 使用puttygen从.ppk文件产生.pem文件：

    ```bash
        puttygen privatekey.ppk -O private-openssh -o privatekey.pem
    ```

    注意：前面一个是大写O，后面一个小写o。

3. 连接的话就把 config 内的 IdentityFile 字段指向这个 .pem 文件 ，然后终端 ssh 快捷连接即可
