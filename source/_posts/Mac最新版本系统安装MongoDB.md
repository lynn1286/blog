---
title: Mac最新版本系统安装MongoDB
date: 2020-09-01 11:45:54
categories: Mac
tags: 安装MongoDB
---

# MongoDB安装（4.2.x）

Mac环境下，使用homebrew安装：
> 现MongoDB不再是开源，官方已经从Homebrew中移除，所以无法通过 brew install mongodb 安装，会提示 No available formula with the name "mongodb"，需使用最新的方法安装社区版。

首先使用 brew tap 命令设定到MongoDB第三方库，执行下方命令：

```
    brew tap mongodb/brew
```

加载完成后，开始安装MongoDB，根据需求选择安装的版本：

```
    brew install mongodb-community   // 安装MongoDB社区服务器的最新可用生产版本
    brew install mongodb-community@4.2   // 安装MongoDB最新4.2.x
    brew install mongodb-community@4.0   // 安装MongoDB最新4.0.x
```

等待安装完成.

验证是否安装成功，可输入查看版本信息：

```
    mongo -version
```

显示版本信息则表示安装成功！！

# 默认配置文件

正常情况下，不需要自己修改配置，直接用默认配置即可:

```
    配置文件：/usr/local/etc/mongod.conf
    日志目录路径：/usr/local/var/log/mongodb
    数据目录路径：/usr/local/var/mongodb
```

# 启动和停止服务器
> 有两种启动方式，使用brew服务启动、手动启动；同时注意的是，用什么方式启动的，就用什么方式停止，否则有可能出现下次无法启动的问题

第一种: 使用brew服务启动，此方式启动，会自动后台运行，关闭终端不影响运行（推荐！）

```
    sudo brew services start mongodb-community  // 启动
    sudo brew services start mongodb-community  // 停止
    sudo brew services restart mongodb-community  // 重启
```

第二种: 手动启动，如果不想或不需要后台MongoDB服务，可手动启动.

```
    sudo mongod --config /usr/local/etc/mongod.conf // 启动
    mongo admin --eval "db.shutdownServer()" // 关闭
```

# 启动异常说明

1.启动时提示‘exception in initAndListen: NonExistentPath: Data directory /data/db not found., terminating’

未加--config启动，使用的dbPath是 /data/db ，不存在或没有创建这个文件件的话或报这个错误。

```
    sudo mkdir -p /data/db // 创建db文件可解决！
```

最新系统中创建会提示: mkdir: /data/db: Read-only file system 
这个问题可以看看[文章](https://xuezenghui.com/posts/update-catalina-bug/), 或者谷歌一下其他的[方法](https://blog.csdn.net/nightwishh/article/details/102535869)

2.启动报‘exception in initAndListen: DBPathInUse: Unable to lock the lock file: (Unknown error). Another mongod instance is already running on the /data/db directory, terminating.’

mongodb非正常关闭，删除mongod.lock文件即可，然后重启

```
    sudo rm /data/db/mongod.lock
```

3.输入启动命令没有任何反应

```
    大部分是Mac系统权限的问题，启动的时候加入 sudo 尝试
```