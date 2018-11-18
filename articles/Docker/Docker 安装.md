---
title: Docker 安装 
weblog_mt_keywords: "docker"
---

## 安装 Docker 要求
Docker 要求 CentOS 系统的内核版本高于 3.10 且是64位操作系统
通过 uname -r 命令查看你当前的内核版本

``` shell
uname -r
```

## 脚本安装 Docker
运行 Docker 安装脚本

``` shell
curl -fsSL https://get.docker.com/ | sh 
# 或者
wget -qO- https://get.docker.com/ | sh
```

**推荐使用阿里云的安装脚本，速度快**

``` shell
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```


## 使用 yum 安装

-使用 CentOS-Extras yum源安装
对于CentOS 7 系统，Docker 软件包和依赖包已经包含在默认的 CentOS-Extras 软件源里，可以直接yum命令安装,但是版本较低,不推荐。

``` shell
# 使用默认CentOS-Extras 软件源安装docker
yum install -y docker
```
- 使用 Docker 官方yum源安装

**此处使用的是阿里云的yum源（和官方保持同步的，碎度快），推荐使用**

``` shell
# step 1: 安装必要的一些系统工具
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
# Step 2: 添加软件源信息
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
# Step 3: 更新并安装 Docker-CE
sudo yum makecache fast
sudo yum -y install docker-ce
# 注意：
# 官方软件源默认启用了最新的软件，您可以通过编辑软件源的方式获取各个版本的软件包。例如官方并没有将测试版本的软件源置为可用，你可以通过以下方式开启。同理可以开启各种测试版本等。
# vim /etc/yum.repos.d/docker-ce.repo
#   将 [docker-ce-test] 下方的 enabled=0 修改为 enabled=1
#
# 安装指定版本的Docker-CE:
# Step 1: 查找Docker-CE的版本:
# yum list docker-ce.x86_64 --showduplicates | sort -r
#   Loading mirror speeds from cached hostfile
#   Loaded plugins: branch, fastestmirror, langpacks
#   docker-ce.x86_64            17.03.1.ce-1.el7.centos            docker-ce-stable
#   docker-ce.x86_64            17.03.1.ce-1.el7.centos            @docker-ce-stable
#   docker-ce.x86_64            17.03.0.ce-1.el7.centos            docker-ce-stable
#   Available Packages
# Step2 : 安装指定版本的Docker-CE: (VERSION 例如上面的 17.03.0.ce.1-1.el7.centos)
# sudo yum -y install docker-ce-[VERSION]
```

## 启动 Docker 服务

启动Docker服务

``` shell
systemctl start docker.service 
# 或者 
service docker start
```


设置 Docker 服务开机自启

``` shell
systemctl enable docker.service
```


## 参考资料

阿里巴巴开源镜像站
https://opsx.alibaba.com/mirror?lang=zh-CN

Docker CE 镜像源站-博客-云栖社区-阿里云
https://yq.aliyun.com/articles/110806

Centos7安装Docker Engine - CarsonFan - 博客园
https://www.cnblogs.com/xmzncc/p/6059554.html

阿里云centos安装docker-engine实践 - andrew-xie - 博客园
https://www.cnblogs.com/andrew-xie/p/5304104.html

tee命令_Linux tee 命令用法详解：把数据重定向到给定文件和屏幕上
http://man.linuxde.net/tee
