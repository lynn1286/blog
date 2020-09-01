---
title: 阿里云服务器安装docker，部署gitlab步骤
date: 2020-09-01 11:37:07
categories: 服务器
tags: 阿里云
---

对服务器内存有要求 , 如果内存小于 4g 会卡顿甚至连接不上服务器或者 gitlab 显示 502 访问慢等问题。


# 环境：

- centos 8 亲测
- 本地 mac 10.15.6

# ssh 无密码登录

步骤一：
登录阿里云 ECS 实例 ， 创建密钥对。
在 管理后台选 网络与安全 ， 选密钥对 ，右上角创建密钥对 ， 这里主要 ， 密钥对创建好了之后 ，浏览器会自动下载私钥文件 .pem , 如果丢失 ， 则无法找回。
密钥对创建好了之后 ， 在密钥对列表点击绑定密钥对 ，然后选中要绑定的 ecs 实例（如果实例为 window 实例不支持 ssh 密钥对）。
绑定后一定要重启实例 ，密钥对才会生效， 对了， 安全组记得开发 ssh 端口（SSH 协议默认的 22 端口）

步骤二：
找到创建后下载的 .pem 私钥路径 ，
然后在本地终端运行命令： chmod 400 [.pem 私钥文件在本地机上的存储路径]，
然后使用 ssh 命令连接远程实例： ssh -i [.pem 私钥文件在本地机上的存储路径] root@[公网 IP 地址]，

# ssh 配置文件

每次连接命令输入有点长， 我们可以配置下 config 文件 来达到简短的命令输入：

步骤一：
终端 cd 进入 .ssh 目录，本人是把 .pem 私钥存放在这个目录的，建议你也存放在这里。
如果没有 config 文件 ， 则创建 ， 如果有的话 则 打开 该文件 ， 并输入以下内容：

```bash
  Host ecs    # 输入ECS实例的名称 , 你喜欢 取什么你决定
  HostName 192.*.*.*   # 输入ECS实例的公网IP地址
  Port 22   # 输入端口号，默认为22
  User root   # 输入登录账号
  IdentityFile ~/.ssh/ecs.pem # 输入.pem私钥文件在本机的地址
```

保存这个文件。

步骤二：
打开本地终端 ， 输入 ssh 连接命令即可， 例如： ssh [ECS 实例的名称，也就是你 config 中的 host 的值]

# 远程登录 实例后 ， 我们准备安装 docker

关于 docker 的概念 ， 可以自行查阅 ，这里不细说。

查看当前系统版本：

```bash
  cat /etc/redhat-release
```

添加新 docker 的 yum 源:

```bash
  yum install -y yum-utils device-mapper-persistent-data lvm2
  yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
  yum-config-manager --enable docker-ce-edge
  yum-config-manager --enable docker-ce-test
```

自定义 docker 配置:

为了提升 Docker 镜像服务的安装速度，这里自定义 docker 的配置，让其从国内镜像源拉去镜像。

```bash
  mkdir /etc/docker
  vi /etc/docker/daemon.json
```

然后在 daemon.json 文件中输入如下内容。

```json
{
  "graph":"/home/docker", // docker 的数据存放文件
  "registry-mirrors": [  
    "https://docker.mirrors.ustc.edu.cn", // docker 国内官方镜像
    "https://reg-mirror.qiniu.com", // 七牛云
    "https://hub-mirror.c.163.com"  // 网易云
    ]
}
```

保存后 ， 更新下这个文件(每次修改后都必要更新和重启docker)。
```bash
  systemctl daemon-reload
```

安装 docker-ce：
```bash
  yum install docker-ce # 或者使用 dnf -y  install docker-ce --nobest
```

安装后 ， 如果你不是root用户 ， 那么使用docker都需要 sudo 来使用。
如果需要省略 sudo ，需要将用户添加到 docker 组。
```bash
  sudo usermod -aG docker USER_NAME
```

开机启动 docker 服务：

```bash
  systemctl enable --now docker
```

启动 docker ：

```bash
  systemctl start docker
```

查看是否安装成功：

```bash
  docker info
```

可以看到我们自定义的配置文件生效了。

# docker 安装 gitlab

查看gitlab镜像：

```bash
  docker search gitlab
```

安装 gitlab(这里备注下： 官方的这个镜像安装之后docker启动后无法访问，看了日志报错是: Chef encountered an error attempting to load the node data for , 找不到解决办法 ，最后无奈删除这个镜像 ， 使用了 twang2218/gitlab-ce-zh 汉化版镜像)：
```bahs
  docker pull twang2218/gitlab-ce-zh
```

检查是否下载了这个镜像：
```bash
  docker images
```

docker 运行 gitlab ：

```bash
  docker run --detach \
    --hostname 127.0.0.1 \
    --publish 443:443 --publish 80:80 --publish 22:22 \
    --name gitlab \
    --restart always \
    --volume /home/gitlab/config:/etc/gitlab \
    --volume /home/gitlab/logs:/var/log/gitlab \
    --volume /home/gitlab/data:/var/opt/gitlab \
    gitlab/gitlab-ce:latest
```

 - --detach：后台运行容器
 - --publish：端口映射，容器端口如何映射到宿主机（本文指我们的 Mac 电脑）端口
 - --name：指定容器的名称，这里我们指定容器名称为 gitlab
 - --restart always：设置当宿主机重启后，容器也会重启
 - --volume：这里使用 bind mount 的方式，设置 gitlab 容器的数据保存在目录 /home/gitlab/ 下

config 文件夹保存 gitlab 配置
logs 文件夹保存 gitlab 输出日志
data 文件夹保存 gitlab 应用数据


输入以上命令后 ，返回一串字符串代表 docker 镜像启动成功了。 
访问你需要在阿里云服务器把对应的安全组端口打开。

接下来我们来改一下 gitlab 的配置文件， 我们通过 volume 参数， 把 数据都存在目录 home/gitlab处，所以我们进入这个目录， 找到config下的gitlab.rb 文件,然后修改以下字段：

```bash
  external_url 'http://127.0.0.1' # 不加端口号 默认 80 
```

接着再修改一些配置来优化我们的gitlab：

```bash
  unicorn['enable'] = true
  unicorn['worker_timeout'] = 60
  unicorn['worker_processes'] = 2

  unicorn['worker_memory_limit_min'] = "300 * 1 << 20"
  unicorn['worker_memory_limit_max'] = "500 * 1 << 20"

  sidekiq['concurrency'] = 16

  postgresql['shared_buffers'] = "256MB"
  postgresql['max_worker_processes'] = 8
```