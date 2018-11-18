---
title: Docker 安装 
weblog_mt_keywords: "docker"
---

## 安装Docker 要求
Docker 要求 CentOS 系统的内核版本高于 3.10 且是64位操作系统
通过 uname -r 命令查看你当前的内核版本
uname -r

## 使用 yum 安装

添加Docker的yum源
tee /etc/yum.repos.d/docker.repo <<-'EOF'
[dockerrepo]
name=Docker Repository
baseurl=https://yum.dockerproject.org/repo/main/centos/$releasever/
enabled=1
gpgcheck=1
gpgkey=https://yum.dockerproject.org/gpg
EOF

对于CentOS 7 系统，Docker 软件包和依赖包已经包含在默认的 CentOS-Extras 软件源里，可以直接yum命令安装。

更新yum源
yum update

yum安装
yum install -y docker-engine
    

## 脚本安装Docker（此方法安装的版本比较新）
运行Docker安装脚本
curl -fsSL https://get.docker.com/ | sh 或者
wget -qO- https://get.docker.com/ | sh


## 启动Docker服务

启动Docker服务
systemctl start docker.service

设置Docker服务开机自启
systemctl enable docker.service


Centos7安装Docker Engine - CarsonFan - 博客园
https://www.cnblogs.com/xmzncc/p/6059554.html

阿里云centos安装docker-engine实践 - andrew-xie - 博客园
https://www.cnblogs.com/andrew-xie/p/5304104.html

tee命令_Linux tee 命令用法详解：把数据重定向到给定文件和屏幕上
http://man.linuxde.net/tee
