---
title: 关于 Docker Hub 上不能注册 Docker ID 的问题 
weblog_mt_keywords: "docker"
---

## 引言

我们中国大陆访问dockerhub的时候，想要注册一个dockerID，发现sign up按钮是灰色的，不能点击进行注册。这个时候通过点击右键“查看网页源代码”和“检查”看看前端代码，就会发现几个联网错误。有谷歌的文件和连接facebook的文件，这时候我们需要翻墙才能注册。有一个可以不借助翻墙软件翻墙注册的办法。


----------


## 方法

1、用chrome浏览器访https://hub.docker.com/，发现注册按钮是灰色的
![](https://www.github.com/wss434631143/xiaoshujiang/raw/master/img/20181122/1542877689762.png)

2、到http://www.ggfwzs.com/下载安装谷歌访问助手
![](https://www.github.com/wss434631143/xiaoshujiang/raw/master/img/20181122/1542877701455.png)

3、解压下载的压缩包
![](https://www.github.com/wss434631143/xiaoshujiang/raw/master/img/20181122/1542877723190.png)

4、按照文件夹下的安装方法页面引导进行安装谷歌访问助手
![](https://www.github.com/wss434631143/xiaoshujiang/raw/master/img/20181122/1542877732445.png)


 注意：要在开发者模式下。

![](https://www.github.com/wss434631143/xiaoshujiang/raw/master/img/20181122/1542877760304.png)

![](https://www.github.com/wss434631143/xiaoshujiang/raw/master/img/20181122/1542877775754.png)

5、安装完访问助手后再次访问https://hub.docker.com/填入注册信息发现出现了人机验证模块，可以成功注册

![](https://www.github.com/wss434631143/xiaoshujiang/raw/master/img/20181122/1542877789195.png)

---------------------

## 参考资料

[https://blog.csdn.net/qq_41684957/article/details/81428971 ](https://blog.csdn.net/qq_41684957/article/details/81428971 )
