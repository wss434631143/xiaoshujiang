---
title: Nginx rewrite 
weblog_mt_keywords: "nginx"
---

## rewrite 作用

rewrite 指令的作用是实现 URL 地址重写（或跳转）。Nginx 的 rewrite 规则需要 PCRE 软件的支持，即通过 Perl 兼容正则表达式语法进行规则匹配。因此需要提前安装好 PCRE 软件，同时在编译安装 Nginx 的时候应该将 rewrite 模块开启，默认编译时 rewrite 模块是开启的，只要你不特殊指定将它关闭。

----------

## rewrite 语法

rewrite 使用的语法为：

``` vim
rewrite regex replacement [flag];
```
语法各部分含义：
| rewrite | regex| replacement | [flag] |
| --- | --- | --- | --- |
| 指令 | 正则表达式 | 重写（或跳转）的 URL 地址 | 标记符号 |

regex 常用正则表达式字符：
![regex 常用正则表达式字符](https://www.github.com/wss434631143/xiaoshujiang/raw/master/img/20181119/1542620548792.png)

flag 标记符号：
| flag 标记符号| 说明|
| --- | --- |
|last|本条规则匹配完成后，继续向下匹配新的 location URI 规则，超过10次匹配不到报500错误，浏览器地址栏 URL 不变（同一站点下）|
|break|本条规则匹配完成即终止，不再匹配后面的任何规则，浏览器地址栏 URL 不变（同一站点下）|
|redirect|返回 302 临时重定向，浏览器地址栏会显示跳转后的 URL 地址,爬虫不会更新 URL|
|permanent|返回 301 永久重定向，浏览器地址栏会显示跳转后的 URL 地址，爬虫会更新 URL|

注意：
在以上的 flag 标记中，last 和 break 标记功能类似，但二者之间有细微的差别，使用 alias 指令时必须用 last 标记，使用 proxy_pass 指令时要使用 break 标记。last 标记在本条 rewrite 规则 执行完毕后，会对其所在的 server {......} 标签重新发起请求，而 break 标记则会在本条规则匹配完成后，终止匹配，不再匹配后面的规则。

last 和 break 只有在同一个站点下浏览器地址栏 URL 才不变，并且返回 200 状态码，如果非同一站点下会返回 302 临时重定向，并且浏览器地址栏会显示跳转后的 URL 地址。


同一站点下使用 last 和 break，浏览器地址栏 URL 不变：
``` vim
server {
    listen       80;
    server_name  blog.abc.com;
    rewrite /blog.html /index.html last; # 当用户访问 http://blog.abc.com/blog.html 时会显示 http://blog.abc.com/index.html 中的内容，浏览器地址栏 URL 不会变
    location / {
        root   html/www;
        index  index.html index.htm;
    }
}
```
同一站点下使用 redirect 和 permanent，浏览器地址栏 URL 变：

``` vim
server {
    listen       80;
    server_name  blog.abc.com;
    rewrite /blog.html /index.html permanent; # 当用户访问 http://blog.abc.com/blog.html 时会跳转到 http://blog.abc.com/index.html ，浏览器地址栏 URL 变
    location / {
        root   html/www;
        index  index.html index.htm;
    }
}
```

----------


## rewrite 示例

``` vim
rewrite ^/(.*) http://www.abc.com/$1 permanent;
```

rewrite 为固定关键字，表示开启一条 rewrite 匹配规则，regex 部分是 ^/(.*) ，这是一个正则表达式，表示匹配所有，匹配成功后跳转到 http://www.abc.com/$1 。这里的 $1 是取前面 regex 部分括号里的内容，结尾的 permanent 是永久 301 重定向标记，即跳转到后面的 http:www.abc.com/$1 地址上。


----------


## rewrite 实战

### 配置 Nginx rewrite 301 跳转

www.abc.com 虚拟主机配置：

``` vim
server {
        listen       80;
        server_name  www.abc.com;
        location / {
                root   html/www;
                index  index.html index.htm;
        }
}
```

abc.com 虚拟主机配置：

``` vim
server {
    listen       80;
    server_name  abc.com;
    rewrite ^/(.*) http://www.abc.com/$1 permanent;   # 当用户访问 abc.com 及下面的任意内容时跳转到 http://www.abc.com/ 对应的地址
    location / {
        root   html/www;
        index  index.html index.htm;
    }
}
```

实验结果：

访问前的地址

![访问前的地址](https://www.github.com/wss434631143/xiaoshujiang/raw/master/img/20181119/1542620615107.png)

访问后的地址

![访问后的地址](https://www.github.com/wss434631143/xiaoshujiang/raw/master/img/20181119/1542620651923.png)

访问 http://abc.com/index.html 时跳转到 http://www.abc.com/index.html ，浏览器的地址栏变成了 http://www.abc.com/index.html

### 配置不同域名的 URL 跳转

www.abc.com 虚拟主机配置：

``` vim
server {
        listen       80;
        server_name  www.abc.com;
        location / {
                root   html/www;
                index  index.html index.htm;
        }
}
```

在html/www文件夹下面创建blog文件夹，并在blog文件夹下面创建index.html文件，文件内容为：http://www.abc.com/blog/ 

blog.abc.com 虚拟主机配置：

``` vim
server {
        listen       80;
        server_name  blog.abc.com;
        location / {
                root   html/blog;
                index  index.html index.htm;
        }
        if ( $http_host ~* "^(.*)\.abc\.com$") {
           set $domain $1;
           rewrite ^/(.*) http://www.abc.com/$domain/$1 permanent; # 当用户访问 blog.abc.com 及下面的任意内容时跳转到 http://www.abc.com/blog/ 对应的地址
        }
}
```

实验结果：

访问前的地址

![访问前的地址](https://www.github.com/wss434631143/xiaoshujiang/raw/master/img/20181119/1542620700354.png)

访问后的地址

![访问后的地址](https://www.github.com/wss434631143/xiaoshujiang/raw/master/img/20181119/1542620737574.png)

访问 http://blog.abc.com/index.html 时跳转到 http://www.abc.com/blog/index.html ，浏览器的地址栏变成了 http://www.abc.com/blog/index.html


----------


## 总结

使用 alias 指令时必须用 last 标记，使用 proxy_pass 指令时要使用 break 标记。

在根 location（即 location / {......}）中 或者 server{......} 标签中编写 rewrite 规则，使用 last 标记比较好, 因为如果有.php等 fastcgi 请求还要继续处理，而在普通的 location（例 location /oldboy/ {......} 或 if{}）中编写 rewrite 规则，则建议使用 break 标记。


----------


## 参考资料

[http://tengine.taobao.org/nginx_docs/cn/docs/http/ngx_http_rewrite_module.html#rewrite](http://tengine.taobao.org/nginx_docs/cn/docs/http/ngx_http_rewrite_module.html#rewrite)

[https://www.cnblogs.com/fengchi/p/6525021.html](https://www.cnblogs.com/fengchi/p/6525021.html)

[https://www.cnblogs.com/mikeluwen/p/7068279.html](https://www.cnblogs.com/mikeluwen/p/7068279.html)

[https://blog.csdn.net/sunyuhua_keyboard/article/details/78273700](https://blog.csdn.net/sunyuhua_keyboard/article/details/78273700)
