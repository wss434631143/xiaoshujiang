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

### 获取容器/镜像的元数据

``` shell
 docker inspect <容器名或ID>
```

----------


## docker image 命令

### 查看本地镜像

``` shell
docker image ls
```
docker image ls 子命令：

-a, --all：显示所有镜像

-q, --quiet：只显示镜像ID

--no-trunc：不截断输出


### 下载镜像

通常情况下，描述一个镜像需要包括“名称+标签”信息，如果不指定标签信息，默认会选择latest标签，这会下载仓库中最新版本的镜像。更严格地讲，镜像的仓库名称中还应该添加仓库地址（即registry,注册服务器）作为前缀，只是我们默认使用的是Docker Hub服务，该前缀可以忽略。

``` shell
docker image pull <镜像名>[:标签(往往用来表示版本信息)]
```


``` shell
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

# 批量删除本地所有镜像
docker image rm $(docker image ls -a -q)
```

docker image rm 子命令：
-f, --force：强制删除

### 给镜像打上标签

``` shell
docker image tag <镜像名> <标签名>
```

### 显示一个镜像的历史

``` shell
docker image history <镜像名>

### 保存镜像

``` shell
docker image save <镜像名> -o <文件名>
```

### 加载镜像

``` shell
docker image load -i <文件名>
# 或者
docker image load < <文件名>
```

----------


## docker container 命令

### 查看本地容器

``` shell
docker container ls
```

docker container ls 子命令：
-a, --all：显示所有容器，包括没有在运行的
-q, --quiet：只显示容器ID
--no-trunc：不截断输出

### 创建并启动一个新的容器

``` shell
docker container run <镜像名或ID> <命令>
```
docker container run 子命令：
--name <容器名>：给容器起个名字

-d, --detach：在后台运行容器

-i, --interactive：表示让容器的标准输入打开

-t, --tty：表示分配一个伪终端

-p, --publish list：将容器的端口发布到主机

--rm：容器退出时自动删除

``` shell
# 示例
docker container run --name mytomcat -d -it -p 8080:8080 tomcat
```

### 进入运行中的容器

``` shell
docker container exec -it <容器名或ID> /bin/bash
```

### 停止一个或多个运行中的容器

``` shell
# 停止一个运行中的容器
docker container stop <容器名或ID>

# 停止多个运行中的容器(中间用空格隔开)
docker container stop <容器名或ID> <容器名或ID> <...>
```

### 杀死一个或多个运行中的容器

``` shell
# 停止一个运行中的容器
docker container kill <容器名或ID>

# 停止多个运行中的容器(中间用空格隔开)
docker container kill <容器名或ID> <容器名或ID> <...>
```

### 暂停一个或多个容器内的所有进程

``` shell
# 停止一个运行中的容器
docker container pause <容器名或ID>

# 停止多个运行中的容器(中间用空格隔开)
docker container pause <容器名或ID> <容器名或ID> <...>
```

### 取消暂停一个或多个容器内的所有进程

``` shell
# 停止一个运行中的容器
docker container unpause <容器名或ID>

# 停止多个运行中的容器(中间用空格隔开)
docker container unpause <容器名或ID> <容器名或ID> <...>
```

### 启动一个或多个停止的容器

``` shell
# 启动一个停止的容器
docker container start <容器名或ID>

# 启动多个停止的容器(中间用空格隔开)
docker container start <容器名或ID> <容器名或ID> <...>
```
docker container start 子命令：
-a, --attach：Attach STDOUT/STDERR and forward signals
-i, --interactive：Attach container's STDIN

### 重启一个或多个容器

``` shell
# 重启一个的容器
docker container restart <容器名或ID>

# 重启多个容器(中间用空格隔开)
docker container restart <容器名或ID> <容器名或ID> <...>
```

### 删除一个或多个容器

``` shell
# 删除一个容器
docker container rm <容器名或ID>

# 删除多个容器(中间用空格隔开)
docker container rm <容器名或ID> <容器名或ID> <...>

# 批量删除本地所有容器
docker container rm $(docker container ls -a -q)
```
docker container rm 子命令：
-f, --force：强制删除

### 从一个容器中取日志

``` shell
docker container logs <容器名或ID>
```

### 列出一个容器里面被改变的文件或者目录，list列表会显示出三种事件，A 增加的，D 删除的，C 被改变的

``` shell
docker container diff <容器名或ID>
```

### 显示一个运行的容器里面的进程信息

``` shell
docker container top <容器名或ID>
```

### 从容器里面拷贝文件/目录到本地一个路径

``` shell
docker container cp <容器名或ID>:/container_path to_path
```

## 参考资料

docker简单使用 - CSDN博客
https://blog.csdn.net/tongzhenggang/article/details/54288351

Docker入门 - 278108678 - 博客园
http://www.cnblogs.com/sunyujun/p/9181069.html

Docker学习笔记(2)--Docker常用命令 - Go2Shell - CSDN博客
https://blog.csdn.net/we_shell/article/details/38368137