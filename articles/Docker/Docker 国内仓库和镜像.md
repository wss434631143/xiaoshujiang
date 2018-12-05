---
title: Docker 国内仓库和镜像
weblog_mt_keywords: "docker"
---

> 由于网络原因，我们在pull Image 的时候，从Docker Hub上下载会很慢。。。所以，国内的Docker爱好者们就添加了一些国内的镜像（mirror）,方便大家使用。

## 国内 Docker 仓库

[阿里云](https://dev.aliyun.com/search.html)

[网易云](https://c.163yun.com/hub#/m/home/)

[时速云](https://hub.tenxcloud.com/)

[DaoCloud](https://hub.daocloud.io/)

## 国外 Docker 仓库

[Docker Hub](https://hub.docker.com/)

[Quay](https://quay.io/)


## 配置 Docker 镜像加速

### 国内加速站点

https://registry.docker-cn.com

http://hub-mirror.c.163.com

https://3laho3y3.mirror.aliyuncs.com

http://f1361db2.m.daocloud.io

https://mirror.ccs.tencentyun.com


### 使用命令来配置加速站点

``` shell
mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["<your accelerate address>"]
}
```

### 使用脚本来配置加速站点

该脚本可以将 --registry-mirror 加入到你的 Docker 配置文件 /etc/docker/daemon.json 中。适用于 Ubuntu14.04、Debian、CentOS6 、CentOS7、Fedora、Arch Linux、openSUSE Leap 42.1，其他版本可能有细微不同。更多详情请访问文档。

``` shell
curl -sSL https://raw.githubusercontent.com/wss434631143/xiaoshujiang/master/articles/Docker/shell/set_mirror.sh | sh -s <your accelerate address>
```

### 通过修改启动脚本配置加速站点

``` shell
# 直接修改 /usr/lib/systemd/system/docker.service 启动脚本
vim /usr/lib/systemd/system/docker.service 
# 在dockerd后面加参数
ExecStart=/usr/bin/dockerd --registry-mirror=<your accelerate address>
```

**以上操作后重启一下 Docker**

``` shell
sudo systemctl daemon-reload
sudo systemctl restart docker
```


## 参考资料

国内 docker 仓库镜像对比
https://ieevee.com/tech/2016/09/28/docker-mirror.html

查找最快的docker镜像
https://github.com/silenceshell/docker_mirror

Docker - 配置国内加速器加速镜像下载。 - TonyZhang24 - 博客园
https://www.cnblogs.com/atuotuo/p/6264800.html

DaoCloud – 企业级云计算领域的创新领导者
https://www.daocloud.io/mirror#accelerator-doc

tee命令_Linux tee 命令用法详解：把数据重定向到给定文件和屏幕上
http://man.linuxde.net/tee
