---
title: CentOS安装OpenResty(Nginx+Lua)开发环境
weblog_mt_keywords: "openresty,nginx,lua,未完待续"
---
## 一.简介

----------
OpenResty® 是一个基于 Nginx 与 Lua 的高性能 Web 平台，其内部集成了大量精良的 Lua 库、第三方模块以及大多数的依赖项。用于方便地搭建能够处理超高并发、扩展性极高的动态 Web 应用、Web 服务和动态网关。

OpenResty® 通过汇聚各种设计精良的 Nginx 模块（主要由 OpenResty 团队自主开发），从而将 Nginx 有效地变成一个强大的通用 Web 应用平台。这样，Web 开发人员和系统工程师可以使用 Lua 脚本语言调动 Nginx 支持的各种 C 以及 Lua 模块，快速构造出足以胜任 10K 乃至 1000K 以上单机并发连接的高性能 Web 应用系统。

OpenResty® 的目标是让你的Web服务直接跑在 Nginx 服务内部，充分利用 Nginx 的非阻塞 I/O 模型，不仅仅对 HTTP 客户端请求,甚至于对远程后端诸如 MySQL、PostgreSQL、Memcached 以及 Redis 等都进行一致的高性能响应。来自[OpenResty®官网](http://openresty.org/cn/)

**总结和拓展：** 
- OpenResty 是 Nginx 与 Lua 的结合；
- OpenResty 是多进程模式，会有一个 master 进程和多个 worker 进程。Master 进程管理 worker 进程,向各 worker 进程发送信号,监控 work 进程状态；
- OpenResty 是异步非阻塞 ；[怎样理解阻塞非阻塞与同步异步的区别？知乎](https://www.zhihu.com/question/19732473)
- 子查询：OpenResty 中有三种方式发起子请求：capture、exec、redirect；
- OpenResty 缓存机制。

**Nginx+Lua架构思维导图：**
![Nginx+Lua架构思维导图](https://www.github.com/wss434631143/xiaoshujiang/raw/master/img/20181118/1542535222267.png)

## 二.关闭SELinux


----------

先临时关闭，然后永久关闭
``` vim
setenforce 0 # 临时关闭
sed -i "s#SELINUX=enforcing#SELINUX=disabled#g" /etc/selinux/config # 永久关闭
```

## 三.防火墙开启80端口


----------

先临时开启80端口，然后再永久开启
``` vim
iptables -I INPUT -p tcp --dport 80 -j ACCEPT # 临时生效
/etc/init.d/iptables save # 保存到配置文件，永久生效
/etc/init.d/iptables status # 查看iptables当前状态
```
## 四.yum安装


----------


对于一些常见的 Linux 发行版本，OpenResty^®^ 提供 [官方预编译包](http://openresty.org/cn/linux-packages.html)。确保你首先用这种方式来安装。

你可以在你的 CentOS 系统中添加 openresty 仓库，这样就可以便于未来安装或更新我们的软件包（通过 yum update 命令）。运行下面的命令就可以添加我们的仓库：

``` vim
yum install yum-utils -y
yum-config-manager --add-repo https://openresty.org/package/centos/openresty.repo
```
然后就可以像下面这样安装软件包，比如 openresty：

``` vim
yum install openresty -y
```
如果你想安装命令行工具 resty，那么可以像下面这样安装 openresty-resty 包：

``` vim
yum install openresty-resty -y
```

命令行工具 opm 在 openresty-opm 包里，而 restydoc 工具在 openresty-doc 包里头。

列出所有 openresty 仓库里头的软件包：

``` vim
yum --disablerepo="*" --enablerepo="openresty" list available
```
参考 [OpenResty RPM 包](http://openresty.org/cn/rpm-packages.html)页面获取这些包更多的细节。

## 五.源码包编译安装


----------


### 下载
从下载页 [Download](http://openresty.org/cn/download.html)下载最新的 OpenResty^®^ 源码包，并且像下面的示例一样将其解压:

```vim
wget https://openresty.org/download/openresty-1.13.6.2.tar.gz
```

### 安装前的准备
必须将这些库 perl 5.6.1+, libpcre, libssl安装在您的电脑之中。 对于 Linux来说, 您需要确认使用 ldconfig 命令，让其在您的系统环境路径中能找到它们。

推荐您使用yum安装以下的开发库:

``` vim
yum install pcre-devel openssl-devel gcc curl -y
```

### 安装
```vim
tar -zxvf openresty-1.13.6.2.tar.gz
cd openresty-1.13.6.2
./configure
make
make install
```

如果您的电脑支持多核 make 工作的特性, 您可以这样编译:

```vim
make -j2
```

默认, --prefix=/usr/local/openresty 程序会被安装到/usr/local/openresty目录。您可以指定各种选项，比如:

``` vim
./configure --prefix=/opt/openresty \
            --with-luajit \
            --without-http_redis2_module \
            --with-http_iconv_module \
            --with-http_postgres_module
```

试着使用 ./configure --help 查看更多的选项。

## 六.配置


----------


### 第一种常规配置方案

修改nginx.conf配置文件

```vim
cd /usr/local/openresty/nginx/conf
mv nginx.conf nginx.conf.$(date +%Y%m%d)
vim nginx.conf
worker_processes  1;
error_log logs/error.log;
events {
    worker_connections 1024;
}
http {
    server {
        listen 8080;
        location / {
            default_type text/html;
            content_by_lua '
                ngx.say("<p>hello, world</p>")
            ';
        }
    }
}
```

添加环境变量

```
echo "export PATH=$PATH:/usr/local/openresty/nginx/sbin" >> /etc/profile
source /etc/profile
```

然后启动openresty，启动命令和nginx一致。

```vim
nginx -c /usr/local/openresty/nginx/conf/nginx.conf
```
启动后查看一下服务

```vim
ps -ef | grep  nginx 
```

访问 Web 服务

```vim
curl http://localhost:8080/
```

如果一切正常，我们应该得到输出

```vim
<p>hello, world</p>
```

重启 Web 服务

```vim
nginx  -s reload
```

### 第二种lua配置方案

添加lua.conf配置文件

```vim
server {
        listen 8080;
        location / {
            default_type text/html;
            content_by_lua '
                ngx.say("<p>hello, world Lua!</p>")
            ';
        }
    }
```

修改nginx.conf配置文件

``` vim
cd /usr/local/openresty/nginx/conf
mv nginx.conf nginx.conf.$(date +%Y%m%d)
vim nginx.conf
```

```vim
worker_processes  1;
error_log logs/error.log;
events {
    worker_connections 1024;
}
http {
    #lua模块路径，多个之间”;”分隔，其中”;;”表示默认搜索路径，默认到/usr/servers/nginx下找  
    lua_package_path "/usr/local/openresty/lualib/?.lua;;";  #lua模块
    lua_package_cpath "/usr/local/openresty/lualib/?.so;;";  #c模块 
    include lua.conf; #lua.conf和nginx.conf在同一目录下
}
```

添加环境变量

```
echo "export PATH=$PATH:/usr/local/openresty/nginx/sbin" >> /etc/profile
source /etc/profile
```

然后启动openresty，启动命令和nginx一致。

```vim
nginx -c /usr/local/openresty/nginx/conf/nginx.conf
```
#启动后查看一下服务

```vim
ps -ef | grep  nginx 
```

访问 Web 服务

```vim
curl http://localhost:8080/
```

如果一切正常，我们应该得到输出

```vim
<p>hello, world Lua!</p>
```

配置lua代码文件

我们把lua代码放在nginx配置中会随着lua的代码的增加导致配置文件太长不好维护，因此我们应该把lua代码移到外部文件中存储。 

在conf文件夹下创建lua文件夹，专门用来存放lua文件

```vim
mkdir /usr/local/openresty/nginx/conf/lua
```


创建test.lua文件

```vim
cd /usr/local/openresty/nginx/conf/lua
vim test.lua
ngx.say("test lua");
```

修改conf/lua.conf文件

```vim
vim /usr/local/openresty/nginx/conf/lua.conf
server {
        listen 8080;
        location / {
            default_type text/html;
            lua_code_cache off; #关闭lua代码缓存,调试时关闭，正式环境开启
            content_by_lua_file conf/lua/test.lua; #相对于nginx安装目录
        }
    }
```

关闭缓存后会看到如下报警（忽略不管）

```vim
nginx: [alert] lua_code_cache is off; this will hurt performance in /usr/local/openresty/nginx/conf/lua.conf:5
```

重启 Web 服务

```vim
nginx  -s reload
```

## 七.测试性能


----------


**安装压力测试工具ab**

```vim
yum -y install httpd-tools
```

**压力测试**
- -c:每次并发数为10个
- -n:共发送50000个请求

```vim
ab -c10 -n50000 http://localhost:8080/ 
```

**测试报详解**

``` vim
Server Software:        Apache          #服务器软件
Server Hostname:        localhost       #域名
Server Port:            80              #请求端口号

Document Path:          /               #文件路径
Document Length:        40888 bytes     #页面字节数

Concurrency Level:      10              #请求的并发数
Time taken for tests:   27.300 seconds  #总访问时间
Complete requests:      1000            #请求成功数量
Failed requests:        0               #请求失败数量
Write errors:           0
Total transferred:      41054242 bytes  #请求总数据大小（包括header头信息）
HTML transferred:       40888000 bytes  #html页面实际总字节数
Requests per second:    36.63 [#/sec] (mean)  #每秒多少请求，这个是非常重要的参数数值，服务器的吞吐量
Time per request:       272.998 [ms] (mean)     #用户平均请求等待时间 
Time per request:       27.300 [ms] (mean, across all concurrent requests)
                                                # 服务器平均处理时间，也就是服务器吞吐量的倒数 
Transfer rate:          1468.58 [Kbytes/sec] received  #每秒获取的数据长度

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:       43   47   2.4     47      53
Processing:   189  224  40.7    215     895
Waiting:      102  128  38.6    118     794
Total:        233  270  41.3    263     945

Percentage of the requests served within a certain time (ms)
  50%    263    #50%用户请求在263ms内返回
  66%    271    #66%用户请求在271ms内返回
  75%    279    #75%用户请求在279ms内返回
  80%    285    #80%用户请求在285ms内返回
  90%    303    #90%用户请求在303ms内返回
  95%    320    #95%用户请求在320ms内返回
  98%    341    #98%用户请求在341ms内返回
  99%    373    #99%用户请求在373ms内返回
 100%    945 (longest request)
```

## 八.启动脚本


----------


添加启动脚本
```vim
vim /etc/init.d/openresty
```

脚本内容如下

```vim
#!/bin/sh
#
# openresty - this script starts and stops the openresty daemin
#
# chkconfig:   - 85 15
# description:  OpenResty is a full-fledged web platform that integrates \ 
#		the standard Nginx core, LuaJIT, many carefully written Lua libraries, \
#		lots of high quality 3rd-party Nginx modules, and most of their external dependencies.
# processname: openresty
# config:      /usr/local/openresty/nginx/conf/nginx.conf
# pidfile:     /usr/local/openresty/nginx/logs/nginx.pid
# Url http://www.cnblogs.com/wushuaishuai/
# Last Updated 2018.07.15

# Source function library.
. /etc/rc.d/init.d/functions

# Source networking configuration.
. /etc/sysconfig/network

# Check that networking is up.
[ "$NETWORKING" = "no" ] && exit 0

nginx="/usr/local/openresty/nginx/sbin/nginx"
prog=$(basename $nginx)

NGINX_CONF_FILE="/usr/local/openresty/nginx/conf/nginx.conf"
NGINX_PID="/usr/local/openresty/nginx/logs/nginx.pid"

[ -f /etc/sysconfig/nginx ] && . /etc/sysconfig/nginx

lockfile=/var/lock/subsys/nginx

start() {
    [ -x $nginx ] || exit 5
    [ -f $NGINX_CONF_FILE ] || exit 6
    echo -n $"Starting $prog: "
    daemon $nginx -c $NGINX_CONF_FILE
    retval=$?
    echo
    #service php-fpm start
    [ $retval -eq 0 ] && touch $lockfile
    return $retval
}
stop() {
    echo -n $"Stopping $prog: "
    $nginx -s stop
    echo_success
    retval=$?
    echo
    #service php-fpm stop
    [ $retval -eq 0 ] && rm -f $lockfile
    return $retval
}

restart() {
    stop
    start
}

reload() {
    configtest || return $?
    echo -n $"Reloading $prog: "
    $nginx -s reload
    RETVAL=$?
    echo
}

force_reload() {
    restart
}

configtest() {
  $nginx -t -c $NGINX_CONF_FILE
}

rh_status() {
    status $prog
}

rh_status_q() {
    rh_status >/dev/null 2>&1
}

case "$1" in
    start)
        rh_status_q && exit 0
        $1
        ;;
    stop)
        rh_status_q || exit 0
        $1
        ;;
    restart|configtest)
        $1
        ;;
    reload)
        rh_status_q || exit 7
        $1
        ;;
    force-reload)
        force_reload
        ;;
    status)
        rh_status
        ;;
    condrestart|try-restart)
        rh_status_q || exit 0
            ;;
    *)
        echo $"Usage: $0 {start|stop|status|restart|condrestart|try-restart|reload|force-reload|configtest}"
        exit 2
esac
```

赋予执行权限

```vim
chmod u+x /etc/init.d/openresty
```

## 九.使用systemctl来管理openresty

``` vim
#查看openresty状态
systemctl status openresty.service

#启动openresty
systemctl start openresty.service

#设置openresty开机自启动
systemctl enable openresty.service

#重启openresty
systemctl restart openresty.service

#停止openresty
systemctl stop openresty.service

#取消openresty开机自启动
systemctl disable openresty.service
```

## 十.参考资料


----------


[跟我学OpenResty(Nginx+Lua)开发目录贴](http://jinnianshilongnian.iteye.com/blog/2190344)