---
title: Nginx 优化 
weblog_mt_keywords: "nginx"
---

## Nginx 安全优化

### 隐藏 Nginx 版本号
实现隐藏 Nginx 版本号的方法是：在 Nginx 配置文件 nginx.conf 中的 http 标签段内加入 “server_tokens off;”参数，如下：

``` vim
http{
……
server_tokens off;
……
}
```
此参数放置在 http 标签内，作用是控制 http response header 内的 Nginx 版本号的显示，以及错误信息中 Nginx 版本号的显示。此参数的默认值为 on，表示显示 Nginx 版本号。[点击查看官方中文说明。](http://tengine.taobao.org/nginx_docs/cn/docs/http/ngx_http_core_module.html#server_tokens)



### 隐藏 Nginx 软件名及版本号

需要修改3个 Nginx 源代码文件并重新编译安装 Nginx。

**第1个文件：nginx-1.14.0/src/core/nginx.h（13、14、22行）**
``` vim
13 #define NGINX_VERSION      "1.14.0"    # 将1.14.0修改为想要显示的版本号，如1.6.2
14 #define NGINX_VER          "nginx/" NGINX_VERSION    # 将 nginx 修改为想要修改的软件名称，如 Apache
22 #define NGINX_VAR          "NGINX"       # 将 NGINX 修改为想要修改的软件名称，如 Apache  
```

**第2个文件：nginx-1.14.0/src/http/ngx_http_header_filter_module.c（49行）**

``` vim
49 static u_char ngx_http_server_string[] = "Server: nginx" CRLF;              #把“Server: nginx”替换为“Server: Apache”
```

**第3个文件：nginx-1.14.0/src/http/ngx_http_special_response.c（22、36行）**

``` vim
 22 "<hr><center>" NGINX_VER "</center>" CRLF # 此行需要修改，修改为"<hr><center>" NGINX_VER "(http://www.cnblogs.com/wushuaishuai/)</center>" CRLF
 36 "<hr><center>nginx</center>" CRLF # 此行需要修改,将对外展示的 nginx 改为 Apache
```

**具体操作步骤：**

- 关闭 Nginx 进程

``` vim
/usr/local/nginx/sbin/nginx -s stop
```

- 备份一下之前的 nginx 安装目录

``` vim
cp -Rp /usr/local/nginx-1.14.0/ /usr/local/nginx-1.14.0-$(date +%Y%m%d)
```

- 查看一下配置参数并记录下来

``` vim
/usr/local/nginx/sbin/nginx -V
```

- 使用 sed 命令一键修改（只测试了1.14.0版本）
``` vim
sed -i 's#"1.14.0"#"1.6.2"#g' nginx-1.14.0/src/core/nginx.h
sed -i 's#"nginx/"#"Apache/"#g' nginx-1.14.0/src/core/nginx.h
sed -i 's#"NGINX"#"Apache"#g' nginx-1.14.0/src/core/nginx.h
sed -i 's#Server: nginx#Server: Apache#g' nginx-1.14.0/src/http/ngx_http_header_filter_module.c
sed -i 's#NGINX_VER_BUILD "#NGINX_VER "(http://www.cnblogs.com/wushuaishuai/)#g' nginx-1.14.0/src/http/ngx_http_special_response.c
sed -i 's#nginx</center>#Apache</center>#g' nginx-1.14.0/src/http/ngx_http_special_response.c
```

- 最后重新编译安装，如果之前经过配置的 nginx 源代码删掉了，则需要重新下载 nginx 源码包并使用之前的配置参数重新配置一下再编译安装
``` vim
cd nginx-1.14.0/
make
make install
```

- 启动 Nginx 进程
``` vim
/usr/local/nginx/sbin/nginx
```


----------


## Nginx 性能优化

nginx的优化 - 跪着行走的BoY - 博客园
https://www.cnblogs.com/dazhidacheng/p/7772451.html

企业级Nginx服务基础到架构优化详解--25条-改变从每一天开始-51CTO博客
http://blog.51cto.com/lilongzi/1839751

Nginx配置及linux系统内存高并发多方面优化 - CSDN博客
https://blog.csdn.net/qq_23598037/article/details/79505398

Nginx web服务优化 （一） - 沉心十年 - 博客园
https://www.cnblogs.com/freeblogs/p/7820227.html


278108678 - 博客园
http://www.cnblogs.com/sunyujun/


