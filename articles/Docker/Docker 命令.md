---
title: Docker 命令 
weblog_mt_keywords: "docker"
---

## docker 命令

### 查看 docker 版本

``` shell
docker version
```

### 显示 docker 系统的信息

``` shell
docker info
```

### 搜索 docker 镜像

``` shell
docker search <镜像名>
```


----------


## docker image 命令

### 查看本地镜像

``` shell
docker image ls
```
docker image ls 子命令

-a, --all：显示所有镜像

-q, --quiet：只显示镜像ID

--no-trunc：不截断输出


### 下载镜像

通常情况下，描述一个镜像需要包括“名称+标签”信息，如果不指定标签信息，默认会选择latest标签，这会下载仓库中最新版本的镜像。更严格地讲，镜像的仓库名称中还应该添加仓库地址（即registry,注册服务器）作为前缀，只是我们默认使用的是Docker Hub服务，该前缀可以忽略。

``` shell
docker image pull <镜像名>[:标签(往往用来表示版本信息)]
# 例如：
docker image pull ubuntu:14.04
docker image pull hub.c.163.com/public/ubuntu:14.04
```

docker image pull 子命令： 

-a,--all-tags=true|false：是否获取仓库中所有版本镜像，默认为否。

### 删除一个或多个镜像

``` shell
# 删除一个镜像
docker image rm <镜像 ID>
# 删除多个镜像(中间用空格隔开)
docker image rm <镜像 ID> <镜像 ID> <...>
# 批量删除本机所有镜像
docker image rm $(docker image ls -a -q)
```


----------


## docker container 命令

### 查看容器，默认只显示运行中的容器

``` shell
docker container ls
```

docker container 子命令：
-a, --all：显示所有容器，包括没有在运行的。
--no-trunc：不截断输出

后台启动容器
docker container run -d <镜像名或ID> 

给镜像打上标签
docker tag <镜像名> <标签名>

删除镜像
docker rmi <镜像名>



启动容器
docker run --name <容器名>  <镜像名>

容器后台运行
docker run -d --name <容器名>  <镜像名>

docker run -d -it -p 8080:8080 tomcat

-d:表示后台运行
-i:表示让容器的标准输入打开
-t:表示分配一个伪终端


查看容器运行状态
docker ps
查看所有容器状态（包括已经停止的）
docker ps -a

停止容器
docker stop <CONTAINER ID>

进入容器
docker exec -it bd303844cfb3 /bin/bash
docker  nsenter -t docker inspect --format "{{ .State.Pid }}" <docker_name or docker_CONTAINER ID> -m -u -i -n -p


docker简单使用 - CSDN博客
https://blog.csdn.net/tongzhenggang/article/details/54288351

Docker入门 - 278108678 - 博客园
http://www.cnblogs.com/sunyujun/p/9181069.html