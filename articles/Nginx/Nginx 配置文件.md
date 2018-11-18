---
title: Nginx 配置文件 
weblog_mt_keywords: "nginx"
---

## Nginx 主配置文件 

Nginx 主配置文件 nginx.conf 是以区块的形式组织的。一般，每个区块以一个大括号“{}”来表示，区块可以分为几个层次，整个配置文件中，main 区位于最上层，在 main 区下面可以有 events 区、http 区等层级，在 http 区中又包含有一个或多个 server 区，每个 server 区中又可有一个或多个 location 区，Nginx 整个配置文件 nginx.conf 的主体框架为：

``` vim
#user  nobody;                              # main 区块，Nginx 核心功能模块 
worker_processes  1;                    
#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;
#pid        logs/nginx.pid;

events {                                    # events 区块，Nginx 核心功能模块
    worker_connections  1024;
}

http {                                      # http 区块，Nginx http 核心模块
    include       mime.types;
    default_type  application/octet-stream;
    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';
    #access_log  logs/access.log  main;
    sendfile        on;
    #tcp_nopush     on;
    #keepalive_timeout  0;
    keepalive_timeout  65;
    #gzip  on;

    server {                                # server 区块，一个 server 代表一个虚拟主机
        listen       80;
        server_name  localhost;
        #charset koi8-r;
        #access_log  logs/host.access.log  main;
        location / {                        # location 区块，属于 server 区块的内容
            root   html;
            index  index.html index.htm;
        }
        #error_page  404              /404.html;
        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}
        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}
        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }
    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;
    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}
    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;
    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;
    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;
    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;
    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}
}
```

下面针对默认的主配置文件 nginx.conf 的核心配置参数做详细解释。


----------


## Nginx 核心配置参数

首先去掉所有的注释行和空行
``` vim
egrep -v "#|^$" /usr/local/nginx/nginx.conf.default
```
形式如下：

``` vim
worker_processes  1; 							# worker 进程的数量，一般设置为与CPU核心数一致，最好设置为 auto，默认为1
events {								# events(事件)区块开始
    worker_connections  1024;					        # 每个 worker 进程支持的最大连接数，默认值为512
}									# events(事件)区块结束
http { 									# http 区块开始
    include       mime.types; 					        # Nginx 支持的媒体类型库文件
    default_type  application/octet-stream; 	                        # 定义默认的媒体类型，默认值为 text/plain
    sendfile        on;							# 开启高效传输模式，默认值为 off
    keepalive_timeout  65; 						# 设置客户端的长连接在服务器端保持的最长时间（在此时间客户端未发起新请求，则长连接关闭），默认值为75s
    server {								# 第一个 server 区块开始，表示一个独立的虚拟主机站点
        listen       80;  						# 提供服务的端口，默认值为 *:80
        server_name  localhost; 				        # 提供服务的域名
        location / {    						# 第一个 location 区块开始
            root   html;						# 为请求设置根目录，默认为 Nginx 安装目录下的 html 目录(相对路径)，可以使用绝对路径
            index  index.html index.htm;                                # 默认的首页文件，多个用空格分开，默认值为 index.html
        }								# 第一个 location 区块结束
        error_page   500 502 503 504  /50x.html;                        # 出现对应的 http 状态码时，使用50x.html回应客户
        location = /50x.html { 					        # location 区块开始，访问50x.html
            root   html;						# 指定对应的站点目录为html
        }
    }
} 									# http 区块结束
```
