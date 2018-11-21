---
title: 利用Syslog Watcher在windows下部署syslog日志服务器 
weblog_mt_keywords: "syslog"
---

## 概述

syslog协议是各种网络设备、服务器支持的网络日志记录标准。Syslog消息提供有关网络事件和错误的信息。系统管理员使用Syslog进行网络管理和安全审核。

通过专用的syslog服务器和syslog协议将来自整个网络的事件记录整合到一个中央存储库中，对于网络安全具有重大意义，syslog日志服务器可收集，解析，存储，分析和解释系统日志消息给专业网络安全管理员，有助于提高网络的稳定性和可靠性。

通过Syslog Watcher可在windows平台搭建日志集中服务器，便于管理并满足合规需求。

## 安装服务端

可前往[https://syslogwatcher.com](https://syslogwatcher.com)下载软件并安装。

在Syslog Watcher部署完成后需对其配置进行修改，主要修改如下：

 - 将编码格式修改为UTF-8不然会出现日志乱码问题。
![](https://www.github.com/wss434631143/xiaoshujiang/raw/master/img/20181121/1542783995586.png)

- 可自定义监听端口

![](https://www.github.com/wss434631143/xiaoshujiang/raw/master/img/20181121/1542784023061.png)

## 安装客户端

本文使用nxlog作为windows日志手机客户端，可前往[https://nxlog.co/](https://nxlog.co/)下载软件并安装。

在部署完成后需对nxlog.conf配置文件进行修改，主要修改如下：
注意：复制之前把中文注释去掉

``` html
Panic Soft
#NoFreeOnExit TRUE

define ROOT     D:\nxlog  #安装路径
define CERTDIR  %ROOT%\cert
define CONFDIR  %ROOT%\conf
define LOGDIR   %ROOT%\data
define LOGFILE  %ROOT%\data\nxlog.log #日志路径
LogFile %LOGFILE%

Moduledir %ROOT%\modules
CacheDir  %ROOT%\data
Pidfile   %ROOT%\data\nxlog.pid
SpoolDir  %ROOT%\data

<Extension _syslog>
    Module      xm_syslog  #syslog服务
</Extension>

<Input in> 
    Module      im_msvistalog  #对windowsvista及以上适用
    ReadFromLast FALSE
    SavePos     FALSE
#   Query       <QueryList>\
#                   <Query Id="0">\
#                        <Select Path="Application">*</Select>\
#                        <Select Path="System">*</Select>\
#                        <Select Path="Security">*</Select>\
#                    </Query>\
#                </QueryList>
</Input>

<Output out>   #输出到远程日志服务器
    Module      om_udp 
    Host        10.10.0.36 
    Port        514 
    Exec        to_syslog_snare();
</Output>

<Output out2>  #输出到远程日志服务器
    Module      om_udp 
    Host        10.10.0.37 
    Port        666 
    Exec        to_syslog_snare();
</Output>

<Route 1>  #输出规则
    Path        in => out 
</Route>

<Route 2>  #输出规则
    Path        in => out2 
</Route>

#<Extension _charconv>
#    Module      xm_charconv
#   AutodetectCharsets gb2312
#    AutodetectCharsets iso8859-2, utf-8, utf-16, utf-32
#</Extension>

<Extension _exec>
    Module      xm_exec
</Extension>

<Extension _fileop>
    Module      xm_fileop

    # Check the size of our log file hourly, rotate if larger than 5MB
    <Schedule>
        Every   1 hour
        Exec    if (file_exists('%LOGFILE%') and \
                   (file_size('%LOGFILE%') >= 5M)) \
                    file_cycle('%LOGFILE%', 8);
    </Schedule>

    # Rotate our log file every week on Sunday at midnight
    <Schedule>
        When    @weekly
        Exec    if file_exists('%LOGFILE%') file_cycle('%LOGFILE%', 8);
    </Schedule>
</Extension>
```

## 参考资料

[利用Syslog Watcher在windows下部署syslog日志服务器 ](https://www.cnblogs.com/meandme/p/9675941.html)