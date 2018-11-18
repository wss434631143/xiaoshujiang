---
title: Nginx 错误日志配置 
weblog_mt_keywords: "nginx"
---

## 简介

Nginx 软件会把自身的故障信息及用户的日志信息记录到指定的日志文件里。配置记录 Nginx 的错误日志是调试 Nginx 服务的重要手段，属于核心功能模块（ngx_core_module）的参数，该参数的名字是 error_log


----------


## 语法

``` vim
error_log  file  level; 
```
error_log 是关键字，file 是保存错误日志的文件路径，level 是错误日志级别

----------


## 位置

错误日志可以配置在 main 区块，也可以配置在虚拟主机配置文件中


----------


## 日志级别(level)

``` vim
debug | info | notice | warn | error | crit | alert | emerg
```
日志级别在上面已经按严重性由轻到重的顺序列出。 设置为某个日志级别将会使指定级别和更高级别的日志都被记录下来。 比如，默认级别error会使nginx记录所有error、crit、alert和emerg级别的消息。 如果省略这个参数，nginx会使用error。


----------

## 配置

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
    include      vhosts/*.conf;
}
```

注：Nginx 缺省配置就是 error_log  logs/error.log  error;


----------


## 参考资料

[http://tengine.taobao.org/nginx_docs/cn/docs/ngx_core_module.html#error_log](http://tengine.taobao.org/nginx_docs/cn/docs/ngx_core_module.html#error_log)