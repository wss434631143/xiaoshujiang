---
title: Nginx 访问日志配置
weblog_mt_keywords: "nginx"
---
## 一.Nginx 访问日志介绍

Nginx 软件会把每个用户访问网站的日志信息记录到指定的日志文件里，供网站提供者分析用户的浏览行为等，此功能由 ngx_http_log_module 模块负责。


----------


## 二.语法及默认值

**语法:**	

``` vim
access_log path [format [buffer=size]];
access_log off;
```

**默认值:**	

``` vim
access_log logs/access.log combined;
# “combined”日志格式:
log_format combined '$remote_addr - $remote_user [$time_local] '
                    '"$request" $status $body_bytes_sent '
                    '"$http_referer" "$http_user_agent"';
```

为访问日志设置路径，格式和缓冲区大小（nginx访问日志支持缓存）。 在同一个配置层级里可以指定多个日志。 特定值off会取消当前配置层级里的所有access_log指令。 如果没有指定日志格式则会使用预定义的“combined”格式。


----------


## 三.配置实战

### 1.修改配置文件

``` vim
[root@localhost conf]# vim nginx.conf
worker_processes  1;
error_log  logs/error.log  error;
events {
    worker_connections  1024;
}
http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
    include     vhosts/*.conf;
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '   # 先定义日志格式，main是日志格式的名字
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    access_log  logs/access.log  main;                                          # 使用日志格式，也可以把这一行放到想记录访问日志的虚拟主机配置文件中去
}
```

### 2.日志变量说明

``` vim
$remote_addr ：记录访问网站的客户端地址
$remote_user ：记录远程客户端用户名称
$time_local ：记录访问时间与时区
$request ：记录用户的 http 请求起始行信息
$status ：记录 http 状态码，即请求返回的状态，例如 200 、404 、502 等
$body_bytes_sent ：记录服务器发送给客户端的响应 body 字节数
$http_referer ：记录此次请求是从哪个链接访问过来的，可以根据 referer 进行防盗链设置
$http_user_agent ：记录客户端访问信息，如浏览器、手机客户端等
$http_x_forwarded_for ：当前端有代理服务器时，设置 Web 节点记录客户端地址的配置，此参数生效的前提是代理服务器上也进行了相关的 x_forwarded_for 设置
```

### 3.真实日志分析

**access.log中的真实日志：**

``` vim
192.168.5.1 - - [25/May/2017:18:27:51 +0800] "GET / HTTP/1.1" 200 12 "-" "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.1; WOW64; Trident/7.0; SLCC2; .NET CLR 2.0.50727; .NET CLR 3.5.30729; .NET CLR 3.0.30729; .NET4.0C; .NET4.0E)" "-"
```

**对上面的日志进行分析：**

``` vim
$remote_addr 对应的是 192.168.5.1 ，即客户端的 IP 地址
$remote_user 对应的是 '-' ，没有远程用户，所以用 '-' 填充
$time_local 对应的是 [25/May/2017:18:27:51 +0800]
$request 对应的是 "GET / HTTP/1.1"
$status 对应的是状态码 200 ，表示访问正常
$body_bytes_sent 对应的是 12 字节，即响应 body 的大小
$http_referer 对应的是 "-" ，由于是直接打开域名浏览的，因此 referer 没有值
$http_user_agent 对应的是 "Mozilla/4.0 (compatible; MSIE........)"
$http_x_forwarded_for 对应的是 "-" ，因为 Web 服务没有使用代理，所以用 "-" 填充
```

----------


## 四.参考资料

[http://tengine.taobao.org/nginx_docs/cn/docs/http/ngx_http_log_module.html#access_log](http://tengine.taobao.org/nginx_docs/cn/docs/http/ngx_http_log_module.html#access_log)