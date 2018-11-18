---
title: Docker 国内仓库
weblog_mt_keywords: "Docker"
---

> 由于网络原因，我们在pull Image 的时候，从Docker Hub上下载会很慢。。。所以，国内的Docker爱好者们就添加了一些国内的镜像（mirror）,方便大家使用。

## 国内仓库

[阿里云](https://dev.aliyun.com/search.html)

[网易云](https://c.163yun.com/hub#/m/home/)

[时速云](https://hub.tenxcloud.com/)

[DaoCloud](https://www.daocloud.io/mirror#accelerator-doc)


curl -sSL https://raw.githubusercontent.com/wss434631143/xiaoshujiang/master/articles/Docker/shell/set_mirror.sh | sh -s http://hub-mirror.c.163.com
curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://c45029c5.m.daocloud.io

该脚本可以将 --registry-mirror 加入到你的 Docker 配置文件 /etc/docker/daemon.json 中。适用于 Ubuntu14.04、Debian、CentOS6 、CentOS7、Fedora、Arch Linux、openSUSE Leap 42.1，其他版本可能有细微不同。更多详情请访问文档。


直接修改 vim /usr/lib/systemd/system/docker.service 
在dockerd后面加参数
ExecStart=/usr/bin/dockerd --registry-mirror=http://hub-mirror.c.163.com

以上操作后重启一下docker
 systemctl restart docker 
 
 
国内 docker 仓库镜像对比
https://ieevee.com/tech/2016/09/28/docker-mirror.html

查找最快的docker镜像
https://github.com/silenceshell/docker_mirror
