---
title: Nginx 状态信息功能配置 
weblog_mt_keywords: "nginx"
---

## Nginx 状态信息功能介绍

- Nginx 有一个 ngx_http_stub_status_module 模块，主要功能是记录 Nginx 的基本访问状态信息，让使用者了解 Nginx 的工作状态
- 要使用该模块，必须在编译安装 Nginx 的时候添加 --with-http_stub_status_module 参数，可以用 /usr/local/nginx/sbin/nginx -V 来查看是否添加


----------


## 配置 Nginx 状态信息功能

可以单独创建一个虚拟主机来配置 Nginx 状态信息功能

``` vim
[root@localhost conf]# vim vhosts/status.abc.conf 
server {
    listen       80;
    server_name  status.abc.com;        
    location / {
        stub_status  on;                # 打开状态信息开关
        access_log   off;
        allow 192.168.153.0/24          # 设置允许访问的 IP 段
        deny all;                       # 设置禁止访问的 IP 段，all 代表除了允许的 IP 以外的所有 IP 都拒绝访问
    }
}
```

使用 ab 压力测试工具模拟用户并发访问网站：

``` vim
yum -y install httpd-tools
ab -c5000 -n50000 http://www.abc.com/ 
```

修改客户端 hosts解析文件，在模拟访问网站的同时访问 http://status.abc.com/

``` html
Active connections: 1005 
server accepts handled requests
 152111 152111 149869 
Reading: 0 Writing: 1 Waiting: 1004 
```

数值说明：

- Active connections ：表示 Nginx 正在处理的活动连接数有多少个
- server ：表示 Nginx 启动到现在共处理了多少个连接
- accepts ：表示 Nginx 启动到现在共成功创建了多少次握手
- handled requests ： 表示总共处理了多少次请求
- Reading ：表示 Nginx 读取到客户端的 Header 信息数
- Writing ：表示 Nginx 返回给客户端的 Header 信息数
- Waiting ：表示 Nginx 已经处理完正在等候下一次请求指令的驻留连接数

请求丢失数 = accepts - server，可以看出本次状态显示没有丢失请求
在开启 keep-alive 的情况下，Waiting = Active connections - (Reading + Writing)

