---
title: Nginx 访问认证 
weblog_mt_keywords: "nginx"
---

## 简介

在实际工作中，企业中有些网站，要求使用账号和密码才能访问，如网站后台、phpMyAdmin 、Wiki 平台 等

模块ngx_http_auth_basic_module 允许使用“HTTP基本认证”协议验证用户名和密码来限制对资源的访问

模块ngx_http_auth_basic_module 下有两条指令 auth_basic 和 auth_basic_user_file


----------


## 语法

``` vim
auth_basic string | off;
```
开启使用“HTTP基本认证”协议的用户名密码验证。参数 off 可以取消继承自上一个配置等级 auth_basic 指令的影响，默认参数为 off。

``` vim
auth_basic_user_file file;
```
指定保存用户名和密码的文件，密码是加密的，格式如下：

``` vim
# comment
name1:password1
name2:password2:comment
name3:password3
```
可以用Apache发行包中的htpasswd命令来创建。


----------


## 实战

配置虚拟主机：

``` vim
server {
        listen       80;
        server_name  www.abc.com;
        location / {
            root   html/blog;
            index  index.html index.htm;
            auth_basic            "xxxxxxxxxx";                     # 设置用于认证的提示字符串
            auth_basic_user_file  /usr/local/nginx/conf/htpasswd;   # 设置认证的密码文件
        }
    }
```

生成认证文件：

``` vim
yum install -y httpd                                   # 要用到 http 的工具htpasswd 来产生账号和密码，所以要先安装 http
htpasswd -bc /usr/local/nginx/conf/htpasswd abc 1234   # 设置认证的账号密码，会保存在密码文件中，第一次使用要用 -bc 参数
htpasswd -b /usr/local/nginx/conf/htpasswd def 5678    # 以后使用无需加 -c 参数
chmod 400 /usr/local/nginx/conf/htpasswd
chown nginx /usr/local/nginx/conf/htpasswd
/usr/local/nginx/sbin/nginx -t
/usr/local/nginx/sbin/nginx -s reload
```

访问网站提示输入用户名和密码：
![认证](http://pb4ob7u50.bkt.clouddn.com/xiaoshujiang/2018724/1532427748208.png)


----------


## 参考资料
[http://tengine.taobao.org/nginx_docs/cn/docs/http/ngx_http_auth_basic_module.html](http://tengine.taobao.org/nginx_docs/cn/docs/http/ngx_http_auth_basic_module.html)