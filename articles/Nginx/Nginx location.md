---
title: Nginx location 
weblog_mt_keywords: "nginx"
---

## location 作用

location 指令的作用是根据用户请求的 URI（路径） 来匹配不同的配置。其实就是根据用户请求的网站地址 URL 进行匹配，匹配成功即进行相关的操作。


----------


## location 语法

location 使用的语法为：

``` vim
location [ = | ~ | ~* | ^~ ] uri { ... }
```
语法各部分含义：
| location | `[ = | ~ | ~* | ^~ | @ ]` | URI | { ... } |
| --- | --- | --- | --- |
| 指令 | 匹配标识 | 匹配的网站网址 | 匹配URI后要执行的配置段 |

每个匹配标识的作用：
|匹配标识|作用|
| --- | --- |
|~* |正则匹配，不区分大小写|
|~ |正则匹配，区分大小写|
|^~ |优先匹配常规字符串，不使用正则匹配|
|= |精确匹配，比如，如果请求“/”出现频繁，定义“location = /”可以提高这些请求的处理速度|
|@|定义命名路径，用在请求重定向中|

----------


## location 示例

``` vim
location = / {                         
    [ configuration A ]                # 当用户请求 http://www.abc.com/ 时执行配置A     
}
location / {
    [ configuration B ]                # 当用户请求 http://www.abc.com/ 时执行配置B
}
location /documents {
    [ configuration C ]                # 当用户请求 http://www.abc.com/documents/document.html 时执行配置C
}
location ^~ /images/ {
    [ configuration D ]                # 当用户请求 http://www.abc.com/images/1.gif 时执行配置D
}
location ~* \.(gif|jpg|jpeg)$ {
    [ configuration E ]                # 当用户请求 http://www.abc.com/documents/1.jpg 时执行配置E
}
```


----------
## location 匹配实战


虚拟主机配置：
``` vim
server {
    listen       80;
    server_name  www.abc.com abc.com;
    root    html/www;

    location / {                           # " / " 表示所有 location 都不能匹配后才匹配这个
        return 401;                        # 当用户请求 http://www.abc.com/index.html 时返回 401
    }

    location = / {                         # " = / " 表示精确匹配
        return 402;                        # 当用户请求 http://www.abc.com/ 时返回 402
    }

    location /documents/ {                 # " /documents/ " 表示匹配常规字符，如果有正则，则优先匹配正则
        return 403;                        # 当用户请求 http://www.abc.com/documents/document.html 时返回 403
    }

    location ^~ /images/ {                 # " ^~ /images/ " 表示匹配常规字符串，不做正则匹配检查
        return 404;                        # 当用户请求 http://www.abc.com/images/1.gif 时返回 404
    }

    location ~* \.(gif|jpg|jpeg)$ {        # " ~* \.(gif|jpg|jpeg)$ " 表示正则匹配
        return 500;                        # 当用户请求 http://www.abc.com/documents/1.jpg 时返回 500
    }
}
```

实验结果：

``` vim
curl -s -o /dev/null -I -w "%{http_code}\n" http://www.abc.com/index.html
401
curl -s -o /dev/null -I -w "%{http_code}\n" http://www.abc.com/
402
curl -s -o /dev/null -I -w "%{http_code}\n" http://www.abc.com/documents/document.html
403
curl -s -o /dev/null -I -w "%{http_code}\n" http://www.abc.com/images/1.gif
404
curl -s -o /dev/null -I -w "%{http_code}\n" http://www.abc.com/documents/1.jpg
500
```

通过以上多个 location 的匹配可以看出匹配的优先顺序。


----------


## 参考资料

[http://tengine.taobao.org/nginx_docs/cn/docs/http/ngx_http_core_module.html#location](http://tengine.taobao.org/nginx_docs/cn/docs/http/ngx_http_core_module.html#location)