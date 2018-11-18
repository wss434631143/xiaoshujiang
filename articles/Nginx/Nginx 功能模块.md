---
title: Nginx 功能模块
weblog_mt_keywords: "nginx"
---

## 一.Nginx 核心功能模块
- Nginx 核心功能模块负责 Nginx 的全局应用，主要对应主配置文件的 Main 区块和 Events 区块，这里有很多  Nginx 必须的全局参数配置。

- Nginx 核心功能模块详情：
		[英文链接](http://nginx.org/en/docs/ngx_core_module.html)
		[中文链接](http://tengine.taobao.org/nginx_docs/cn/docs/ngx_core_module.html)

- 实例：

``` vim
user nginx nginx;
worker_processes  auto;
error_log logs/error.log error;
events {
    worker_connections  1024;
}
```


----------


## 二.Nginx http 功能模块

| Nginx http 功能模块| 模块说明|
| --- | --- |
| ngx_http_core_module | 包括一些核心的 http 参数配置，对应 Nginx 的配置为 HTTP 区块部分 |
| ngx_http_access_module | 访问控制模块，用来控制网站用户对 Nginx 的访问 |
| ngx_http_gzip_module | 压缩模块，对 Nginx 返回的数据压缩，属于性能优化模块 |
| ngx_http_fastcgi_module | FastCGI 模块，和动态应用相关的模块，如 PHP |
| ngx_http_proxy_module | proxy 代理模块 |
| ngx_http_upstream_module | 负载均衡模块，可实现网站的负载均衡和节点的健康检查 |
| ngx_http_rewrite_module | URL 地址重写模块 |
| ngx_http_limit_conn_module | 限制用户并发连接数以及请求数的模块 |
| ngx_http_limit_req_module | 根据定义的 key 限制 Nginx 请求过程的速率 |
| ngx_http_log_module | 访问日志模块，以指定的格式记录 Nginx 客户访问日志等信息 |
| ngx_http_auth_basic_module | Web 认证模块，设置 Web 用户通过账号密码访问 Nginx |
| ngx_http_ssl_module | ssl 模块，用于加密的 http 连接，如 https |
| ngx_http_stub_status_module | 记录 Nginx 基本访问状态信息等的模块 |

Nginx http 功能模块详情请看下面的官方文档



----------


## 三.Nginx 官方文档

[Ningx英文文档](http://nginx.org/en/docs/)
[Ningx中文文档](http://tengine.taobao.org/nginx_docs/cn/docs/)