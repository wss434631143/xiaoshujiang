---
title: Nginx 编译安装 
weblog_mt_keywords: "nginx,lnmp,未完待续"
---

## 一.关闭SELinux

``` vim
setenforce 0 # 临时关闭
sed -i "s#SELINUX=enforcing#SELINUX=disabled#g" /etc/selinux/config # 永久关闭
```


----------


## 二.防火墙开启80端口

``` vim
iptables -I INPUT -p tcp --dport 80 -j ACCEPT # 临时生效
/etc/init.d/iptables save # 保存到配置文件，永久生效
/etc/init.d/iptables status # 查看iptables当前状态
```


----------


## 三.安装nginx依赖包

``` vim
yum -y groupinstall Development tools
yum -y install pcre pcre-devel zlib zlib-devel openssl openssl-devel wget
```
>pcre pcre-devel：使nginx支持正则表达式
>zlib zlib-devel：使nginx支持gzip压缩
>openssl openssl-devel：使nginx支持https


----------


## 四.添加nginx用户

``` vim
useradd nginx -s /sbin/nologin -M 
```


----------


## 五.编译安装nginx

``` vim
wget http://nginx.org/download/nginx-1.14.0.tar.gz
tar zxvf nginx-1.14.0.tar.gz 
cd nginx-1.14.0/
./configure --prefix=/usr/local/nginx-1.14.0 \
--user=nginx \
--group=nginx \
--with-http_ssl_module \
--with-http_stub_status_module
make
make install
ln -s /usr/local/nginx-1.14.0 /usr/local/nginx # 创建软链接
```

**本次用到的配置项：**
> --prefix=PATH 					   set installation prefix # 设置安装目录
> --user=USER                        set non-privileged user for worker processes # worker进程用户权限
> --group=GROUP					   set non-privileged group for worker processes # worker进程用户组权限
> --with-http_ssl_module             enable ngx_http_ssl_module # 激活 ssl 功能
> --with-http_stub_status_module     enable ngx_http_stub_status_module # 激活状态信息功能

**配置后的基本信息：**
> nginx path prefix: "/usr/local/nginx-1.14.0" nginx binary file:
> "/usr/local/nginx-1.14.0/sbin/nginx" nginx modules path:
> "/usr/local/nginx-1.14.0/modules" nginx configuration prefix:
> "/usr/local/nginx-1.14.0/conf" nginx configuration file:
> "/usr/local/nginx-1.14.0/conf/nginx.conf" nginx pid file:
> "/usr/local/nginx-1.14.0/logs/nginx.pid" nginx error log file:
> "/usr/local/nginx-1.14.0/logs/error.log" nginx http access log file:
> "/usr/local/nginx-1.14.0/logs/access.log" nginx http client request
> body temporary files: "client_body_temp" nginx http proxy temporary
> files: "proxy_temp" nginx http fastcgi temporary files: "fastcgi_temp"
> nginx http uwsgi temporary files: "uwsgi_temp" nginx http scgi
> temporary files: "scgi_temp"

**查看nginx编译参数**

``` vim
/usr/local/nginx/sbin/nginx -V
```


----------


## 六.检查配置文件并启动nginx进程

``` vim
/usr/local/nginx/sbin/nginx -t # 检查配置文件
/usr/local/nginx/sbin/nginx # 启动nginx进程
```


----------


## 七.查看nginx进程

``` vim
ps -ef|grep nginx
```


----------


## 八.查看nginx进程对应的端口是否成功启动

``` vim
lsof -i:80
```


----------


## 九.测试是否能成功访问

``` vim
curl http://localhost
http://192.168.164.134/
```


----------


## 十一.启动脚本

**添加启动脚本**
``` vim
vim /etc/init.d/nginx
```

**脚本内容如下**
``` vim
#!/bin/sh
#
# nginx - this script starts and stops the nginx daemin
#
# chkconfig:   - 85 15
# description:  Nginx is an HTTP(S) server, HTTP(S) reverse \
#               proxy and IMAP/POP3 proxy server
# processname: nginx
# config:      /usr/local/nginx/nginx.conf
# pidfile:     /usr/local/nginx/logs/nginx.pid
# Url http://www.cnblogs.com/wushuaishuai/
# Last Updated 2018.07.17

# Source function library.
. /etc/rc.d/init.d/functions

# Source networking configuration.
. /etc/sysconfig/network

# Check that networking is up.
[ "$NETWORKING" = "no" ] && exit 0

nginx="/usr/local/nginx/sbin/nginx"
prog=$(basename $nginx)

NGINX_CONF_FILE="/usr/local/nginx/conf/nginx.conf"
NGINX_PID="/usr/local/nginx/logs/nginx.pid"

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

``` vim
chmod u+x /etc/init.d/nginx
```

脚本下载

``` vim
wget https://files.cnblogs.com/files/wushuaishuai/nginx.sh -O nginx
```

----------


## 十二.使用systemctl来管理nginx

``` vim
#查看nginx状态
systemctl status nginx.service

#启动nginx
systemctl start nginx.service

#设置nginx开机自启动
systemctl enable nginx.service

#重启nginx
systemctl restart nginx.service

#停止nginx
systemctl stop nginx.service

#取消nginx开机自启动
systemctl disable nginx.service
```



