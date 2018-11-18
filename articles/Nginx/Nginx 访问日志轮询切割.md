---
title: Nginx 访问日志轮询切割
weblog_mt_keywords: "nginx,日志"
---

## 一.概述

默认情况下 Nginx 会把所有的访问日志生成到一个指定的访问日志文件 access.log 里，但这样一来，时间长了就会导致日志个头很大，不利于日志的分析和处理，因此，有必要对 Nginx 日志，按天或按小时进行切割，使其分成不同的文件保存。这里使用按天切割的方法。

日志切割实现的原理是将正在写入的 access.log 日志改名为带日期格式的文件，然后平滑重新加载 Nginx ，生成新的 access.log 日志文件。


----------


## 二.shell脚本实现

### 1.早期用过的shell脚本（cut_nginx_log.sh）

``` vim
#!/bin/bash
# This script run at 00:00

# The Nginx logs path
logs_path="/usr/local/nginx/logs/"

mkdir -p ${logs_path}$(date -d "yesterday" +"%Y")/$(date -d "yesterday" +"%m")/
mv ${logs_path}access.log ${logs_path}$(date -d "yesterday" +"%Y")/$(date -d "yesterday" +"%m")/access_$(date -d "yesterday" +"%Y%m%d").log
kill -USR1 `cat /usr/local/nginx/logs/nginx.pid`
```

### 2.老男孩shell脚本（cut_nginx_log.sh）

``` vim
#!/bin/bash
Dateformat=`date +%Y%m%d`
Basedir="/usr/local/nginx"
Nginxlogdir="$Basedir/logs"
Logname="access"
[ -d $Nginxlogdir ] && cd $Nginxlogdir || exit 1
[ -f ${Logname}.log ] || exit 1
/bin/mv ${Logname}.log ${Dateformat}_${Logname}.log
$Basedir/sbin/nginx -s reload
```

赋权限

``` vim
chmod +x /usr/local/nginx/cut_nginx_log.sh
```

设置计划任务，每天凌晨00:00切割nginx访问日志

``` vim
cat >> /var/spool/cron/root << eof
#nginx日志切割
00 00 * * * /bin/bash  /usr/local/nginx/cut_nginx_log.sh
eof
```

### 3.更厉害的shell脚本（log_rotate.sh）

``` vim
#!/bin/bash

function rotate() {
logs_path=$1

echo Rotating Log: $1
cp ${logs_path} ${logs_path}.$(date -d "yesterday" +"%Y%m%d")
> ${logs_path}
    rm -f ${logs_path}.$(date -d "7 days ago" +"%Y%m%d")
}
 
for i in $*
do
        rotate $i
done
```

赋权限

``` vim
chmod +x /usr/local/nginx/log_rotate.sh
```

手动执行切割：

``` vim
find /usr/lcoa/nginx/logs/ -size +0 -name '*.log' | xargs /usr/local/nginx/log_rotate.sh
```

设置计划任务每天凌晨00:00对nginx日志进行切割，0K的日志不进行切割：

``` vim
cat >> /var/spool/cron/root << eof
#nginx日志切割
30 0 * * * find /usr/lcoa/nginx/logs/ -size +0 -name '*.log' | xargs /usr/local/nginx/log_rotate.sh
eof
```


----------


## 三.python脚本实现

python脚本如下（log_rotate.py）：

``` python
#!/usr/bin/env python
   
import datetime,os,sys,shutil
   
log_path = '/usr/local/nginx/logs/'
log_file = 'access.log'
   
yesterday = (datetime.datetime.now() - datetime.timedelta(days = 1))
   
try:
    os.makedirs(log_path + yesterday.strftime('%Y') + os.sep + \
                yesterday.strftime('%m'))
   
except OSError,e:
    print
    print e
    sys.exit()
   
   
shutil.move(log_path + log_file,log_path \
            + yesterday.strftime('%Y') + os.sep \
            + yesterday.strftime('%m') + os.sep \
            + log_file + '_' + yesterday.strftime('%Y%m%d') + '.log')
   
   
os.popen("sudo kill -USR1 `cat /usr/local/nginx/logs/nginx.pid`")
```
赋权限

``` vim
chmod +x /usr/local/nginx/log_rotate.py
```

设置计划任务，每天凌晨00:00切割nginx访问日志

``` vim
cat >> /var/spool/cron/root << eof
#nginx日志切割
00 00 * * * /usr/bin/python  /usr/local/nginx/log_rotate.py
eof
```


----------


## 四.使用logrotate实现

[logrotate日志管理工具](https://www.cnblogs.com/wushuaishuai/p/9330952.html)