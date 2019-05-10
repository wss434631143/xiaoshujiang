

#                第一章 zabbix监控的意义

## 1.1 为什么要监控

1. 业务安全性的保障
2. 系统的保障 
3. 产品持续性的运行

## 1.2 监控的内容

![](https://raw.githubusercontent.com/wss434631143/picgo/master/img/20190509205638.png)



## 1.3 zabbix的选择性

- [x] 纯命令监控太局限性
- [x] 监控三剑客（Nagios、*zabbix*、Cacti ）

- [x] 可及时发现故障，并在故障恢复的第一时间得到通知
- [x] 灵活运用，包括zabbix的阈值定义，自动发现，API接口，触发动作等功能

## 1.4 zabbix的工作组件及告警流程

![](https://raw.githubusercontent.com/wss434631143/picgo/master/img/20190509205743.png)

![](https://raw.githubusercontent.com/wss434631143/picgo/master/img/20190509212546.png)

1. 数据采集：Zabbix 通过 SNMP、Agent、ICMP、SSH、IPMI 等对系统进行数据采集。
2. 数据存储： Zabbix存储在MySQL上，也可以存储在其他数据库服务。
3. 数据分析：当我们事后需要复盘分析故障时，zabbix能给我们提供图形以及时间等相关信息，方面我们确定故障所在。
4. 数据展示：web界面展示、(移动APP、java_php开发一个web界面也可以)。
5. 监控报警：电话报警、邮件报警、微信报警、短信报警、报警升级机制等（无论什么报警都可以）。
6. 报警处理：当接收到报警，我们需要根据故障的级别进行处理，比如:重要紧急、重要不紧急，等。根据故障的级别，配合相关的人员进行快速处理。

# **第二章 zabbix的安装部署及使用**

> 注意：安装zabbix要求还是比较多的
>
> 硬件方面：以监控主机台数而定。
>
> 软件方面：源码安装，lamp/lnmp的版本要求，php的扩展包等。
>
> https://www.zabbix.com zabbix的官方网站。
>

## 2.1 源码安装zabbix服务端

**部署nginx**

```less
[root@localhost opt]# yum install gcc  openssl openssl-devel pcre pcre-devel  -y	#安装依赖
[root@localhost opt]# rz -y									#rz上传软件包   			
[root@localhost opt]# ls				
nginx-1.12.2.tar.gz
[root@localhost opt]# useradd -r -s /sbin/nologin nginx					#创建nginx用户
[root@localhost opt]# tar xf nginx-1.12.2.tar.gz  && cd nginx-1.12.2		 #解压，进入目录
[root@localhost nginx-1.12.2]# ./configure  --user=nginx --group=nginx --prefix=/usr/local/nginx --with-http_stub_status_module  --with-http_ssl_module  #编译
[root@localhost nginx-1.12.2]# make && make install				#安装
[root@localhost nginx-1.12.2]# /usr/local/nginx/sbin/nginx		 #直接启动nginx
[root@localhost nginx-1.12.2]# ps -ef |grep nginx					#可查看nginx是否启动
```

**部署php**

```less
[root@localhost nginx-1.12.2]# yum install -y gcc gcc-c++ make gd-devel libxml2-devel \
> libcurl-devel libjpeg-devel libpng-devel openssl-devel \
> libxslt-devel														#安装依赖
[root@localhost opt]# rz -y						    		#上传软件包，亦可直接wget
[root@localhost opt]# ls			
php-5.6.36.tar.gz
******************************************************************************************
[root@localhost opt]# wget http://docs.php.net/distributions/php-5.6.36.tar.gz
[root@localhost opt]# tar xf php-5.6.36.tar.gz					#解压
[root@localhost opt]# cd php-5.6.36/								#进入安装目录
[root@localhost php-5.6.36]# ./configure --prefix=/usr/local/php \
> --with-config-file-path=/usr/local/php/etc \
> --enable-fpm --enable-opcache \
> --with-mysql --with-mysqli  \
> --enable-session --with-zlib --with-curl --with-gd \
> --with-jpeg-dir --with-png-dir --with-freetype-dir \
> --enable-mbstring --enable-xmlwriter --enable-xmlreader \
> --enable-xml --enable-sockets --enable-bcmath --with-gettext		#编译
[root@localhost php-5.6.36]# make -j 8 && make install				#安装
[root@localhost php-5.6.36]# cp php.ini-production /usr/local/php/etc/php.ini  #拷贝模块文件
[root@localhost php-5.6.36]# cp sapi/fpm/php-fpm.conf /usr/local/php/etc/php-fpm.conf
[root@localhost php-5.6.36]# cp sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm
[root@localhost php-5.6.36]# cp sapi/fpm/php-fpm.service /usr/lib/systemd/system/
[root@localhost php-5.6.36]# chmod +x /etc/init.d/php-fpm				#启动文件权限
[root@localhost php-5.6.36]# /etc/init.d/php-fpm start				#启动php
```

**部署mysql**

```less
[root@localhost php-5.6.36]# yum install cmake make gcc-c++ cmake bison-devel ncurses-devel perl-Module-Install.noarch -y										#安装依赖
[root@localhost opt]# wget http://cdn.mysql.com/archives/mysql-5.6/mysql-5.6.27.tar.gz													                 #下载mysql软件包
[root@localhost opt]# groupadd mysql								   #创建mysql组
[root@localhost opt]# useradd  mysql  -s /sbin/nologin -M -g mysql	 #创建mysql用户
[root@localhost opt]# mkdir -p /data/mysql && mkdir -p /usr/local/mysql #创建存储数据目录
[root@localhost opt]# tar xf mysql-5.6.27.tar.gz						#解压mysql软件包
[root@localhost opt]# cd mysql-5.6.27/							   #进入mysql目录
[root@localhost mysql-5.6.27]# cmake -DCMAKE_INSTALL_PREFIX=/usr/local/mysql -DMYSQL_DATADIR=/data/mysql -DSYSCONFDIR=/etc -DWITH_MYISAM_STORAGE_ENGINE=1 -DWITH_INNOBASE_STORAGE_ENGINE=1 -DWITH_MEMORY_STORAGE_ENGINE=1 -DWITH_READLINE=1 -DMYSQL_UNIX_ADDR=/var/lib/mysql/mysql.sock -DMYSQL_TCP_PORT=3306 -DENABLED_LOCAL_INFILE=1 -DWITH_PARTITION_STORAGE_ENGINE=1 -DEXTRA_CHARSETS=all -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci									#编译
[root@localhost mysql-5.6.27]# make -j 8 && make install				#安装
[root@localhost mysql-5.6.27]# cp support-files/my-default.cnf /etc/my.cnf	#配置文件
[root@localhost mysql-5.6.27]# vim /etc/my.cnf						##修改配置文件
##在[mysqld]中增加一行
datadir=/data/mysql
log_bin=mysql-bin
[root@localhost mysql-5.6.27]# cp -rf  support-files/mysql.server /etc/init.d/mysql
[root@localhost mysql-5.6.27]# chown -R mysql.mysql /data/mysql
[root@localhost mysql-5.6.27]# chown -R mysql.mysql /usr/local/mysql
[root@localhost mysql-5.6.27]# chmod +x /etc/init.d/mysql
[root@localhost mysql-5.6.27]# /usr/local/mysql/scripts/mysql_install_db --basedir=/usr/local/mysql/ --datadir=/data/mysql --user=mysql			#初始化
[root@localhost mysql-5.6.27]# ln -s /usr/local/mysql/bin/mysql  /usr/bin	#mysql命令链接
[root@localhost mysql-5.6.27]# /etc/init.d/mysql start					#启动mysql
[root@localhost mysql-5.6.27]# mysql -uroot -p						#登陆mysql
mysql> use mysql;			
mysql>  UPDATE user SET Password=PASSWORD('123456.') where USER='root';	#修改密码
mysql> FLUSH PRIVILEGES;
[root@localhost mysql-5.6.27]# mysql -uroot -pMa123456.					#用新密码登陆
mysql> create database zabbix;										#创建zabbix数据库
mysql> grant all on zabbix.* to 'zabbix'@'localhost' identified by '123456.';	#授权用户
mysql> quit															#退出
########################################################################################
以下为导入zabbix数据，schema.sql，images.sql，data.sql文件在zabbix-4.0.0/database/mysql目录
########################################################################################
[root@localhost mysql]# mysql  -uzabbix -p123456. zabbix < schema.sql
[root@localhost mysql]# mysql  -uzabbix -p123456. zabbix < images.sql
[root@localhost mysql]# mysql  -uzabbix -p123456. zabbix < data.sql
							
```

**部署zabbix_server**

```less
[root@localhost opt]# yum install libxml2-devel libcurl-devel libevent-devel net-snmp-devel mysql-community-devel -y
[root@localhost opt]# tar xf zabbix-4.0.0.tar.gz && cd zabbix-4.0.0/       #解压并进入
[root@localhost zabbix-4.0.0]# groupadd zabbix
[root@localhost zabbix-4.0.0]# useradd -g zabbix zabbix -s /sbin/nologin
./configure --prefix=/usr/local/zabbix --enable-server --enable-proxy --enable-agent --with-mysql=/usr/local/mysql/bin/mysql_config --with-net-snmp --with-libcurl --with-libxml2 [root@localhost zabbix-4.0.0]# make && make install
[root@localhost zabbix-4.0.0]# ls /usr/local/zabbix/						#zabbix目录路径
bin  etc  lib  sbin  share
[root@localhost zabbix-4.0.0]# vim /usr/local/zabbix/etc/zabbix_server.conf  #配置文件
DBHost=localhost
DBName=zabbix
DBUser=zabbix
DBPassword=Ma123456.
[root@localhost bin]# vi /usr/lib/systemd/system/zabbix_server.service	#配置systemd文件
[Unit]
Description=Zabbix Server
After=syslog.target
After=network.target

[Service]
Environment="CONFFILE=/usr/local/zabbix/etc/zabbix_server.conf"
EnvironmentFile=-/etc/sysconfig/zabbix-server
Type=forking
Restart=on-failure
PIDFile=/tmp/zabbix_server.pid
KillMode=control-group
ExecStart=/usr/local/zabbix/sbin/zabbix_server -c $CONFFILE
ExecStop=/bin/kill -SIGTERM $MAINPID
RestartSec=10s
TimeoutSec=0

[Install]
WantedBy=multi-user.target
[root@localhost bin]# systemctl  start zabbix_server.service				#直接启动zabbix
[root@localhost bin]# ps -ef |grep zabbix								   #查看zabbix进程
[root@localhost bin]# /usr/local/zabbix/sbin/zabbix_agentd			   #启动zabbix_agent
[root@localhost bin]# ps -ef |grep zabbix_agent						  #查看agent进程
```

**部署web界面安装**

```
[root@localhost zabbix-4.0.0]# cp -rf /opt/zabbix-4.0.0/frontends/php/* /usr/local/nginx/html/							#把zabxix的前端页面拷贝到nginx的发布目录
[root@localhost zabbix-4.0.0]# vim /usr/local/php/etc/php.ini		  #修改php配置文件
max_execution_time = 300
post_max_size = 16M
max_input_time = 300
always_populate_raw_post_data = -1
date.timezone = Asia/Shanghai
mysqli.default_socket = /var/lib/mysql/mysql.sock

[root@localhost zabbix-4.0.0]# /etc/init.d/php-fpm restart	      #重启php-fpm

[root@localhost zabbix-4.0.0]# vim /usr/local/nginx/conf/nginx.conf   #nginx整合php
    server {
        listen       80;
        server_name  localhost;

        access_log  logs/zabbix.access.log  main;

        location / {
            root   html;
            index  index.php index.html index.htm;
        }

        location ~ \.php$ {
            root           html;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include        fastcgi_params;
        }
    }
[root@localhost zabbix-4.0.0]# /usr/local/nginx/sbin/nginx  -t		#检查nginx配置文件
[root@localhost zabbix-4.0.0]# /usr/local/nginx/sbin/nginx  -s reload  #重新加载nginx配置文件

####################################################################################
至此，zabbix已经安装完成，直接IP即可访问。
安装步骤：
1.配置DB，
2.配置zabbix用户
3.下载文件，直接写入/usr/local/nginx/html/conf/zabbix.conf.php文件
<?php
// Zabbix GUI configuration file.
global $DB;

$DB['TYPE']     = 'MYSQL';
$DB['SERVER']   = 'localhost';
$DB['PORT']     = '3306';
$DB['DATABASE'] = 'zabbix';
$DB['USER']     = 'zabbix';
$DB['PASSWORD'] = 'Ma123456.';

// Schema name. Used for IBM DB2 and PostgreSQL.
$DB['SCHEMA'] = '';

$ZBX_SERVER      = 'localhost';
$ZBX_SERVER_PORT = '10051';
$ZBX_SERVER_NAME = '';

$IMAGE_FORMAT_DEFAULT = IMAGE_FORMAT_PNG;
4.直接登陆，（默认用户Admin,密码zabbix）
5.修改页面为中文
点击右上角用户--Language--chinese(zh_CN)--update即可

```



## 2.2 yum安装zabbix服务端

```less
====yum安装mysql====
[root@localhost ~]# yum -y install yum-utils							#安装插件
[root@localhost ~]# rpm -ivh https://dev.mysql.com/get/mysql80-community-release-el7-1.noarch.rpm														#安装mysql源
[root@localhost ~]# yum-config-manager --disable mysql80-community      #安装mysql180
[root@localhost ~]# yum-config-manager --enable mysql57-community		  #安装mysql157
[root@localhost ~]# yum install mysql-community-server				 #下载mysql
[root@localhost ~]# yum install mysql-community-server -y				 #安装mysql-server
[root@localhost ~]# systemctl start mysqld							#启动mysql
[root@localhost ~]# systemctl status mysqld							#查看mysql状态
[root@localhost ~]# grep 'temporary password' /var/log/mysqld.log		  #查看mysql随机密码
[root@localhost ~]# mysql -uroot -pXp.8nzZVFzri						#登陆mysql
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'Ma123456.';		  #重新设置mysql密码
[root@localhost etc]# cp my.cnf my.cnf.bak						#备份一下mysql配置文件
[root@localhost etc]# vim /etc/my.cnf								#重新设置mysql配置文件
[mysql]
socket = /tmp/mysql.sock									#socker路径
[mysqld]		
user = mysql											   #用户
port = 3306												   #端口
datadir = /var/lib/mysql									#数据目录
socket = /tmp/mysql.sock									#socker路径
bind-address = 0.0.0.0										#监听地址
pid-file = /var/run/mysqld/mysqld.pid						 #pid文件路径
character-set-server = utf8									#字符集UTF8
collation-server = utf8_general_ci							 	
log-error = /var/log/mysqld.log								#error日志
#####mysql基本调优####
max_connections = 10240										#最大用户连接数
open_files_limit = 65535									#打开文件数
innodb_buffer_pool_size = 3G								#缓存池大小
innodb_flush_log_at_trx_commit = 2							 #每一次事务提交是否写入硬盘
innodb_log_file_size = 256M									#mysql事务日志文件大小

[root@localhost etc]# systemctl restart  mysqld					#重启mysql

====yum安装zabbix====
[root@localhost ~]# rpm -ivh http://repo.zabbix.com/zabbix/4.0/rhel/7/x86_64/zabbix-release-4.0-1.el7.noarch.rpm								#安装zabbix源
[root@localhost ~]# yum install zabbix-server-mysql zabbix-web-mysql -y	#安装zabbix
[root@localhost ~]# mysql -uroot -pMa123456.					##进入mysql
mysql> create database zabbix;							    ##创建zabbix数据库
mysql> grant all on zabbix.* to 'zabbix'@'localhost' identified by 'Ma123456.';
														##授权zabbix用户访问
[root@localhost ~]# cd /usr/share/doc/zabbix-server-mysql-4.0.7/   ##进入到mysql文档目录
[root@localhost zabbix-server-mysql-4.0.7]#  zcat create.sql.gz | mysql -uroot -pMa123456.  zabbix												###导入到zabbix库里面

[root@localhost ~]# vim /etc/zabbix/zabbix_server.conf			#修改zabbix配置
DBPassword=Ma123456.										 #配置mysql密码地址用户
[root@localhost ~]# systemctl start zabbix-server					#启动zabbix

====修改apache====
[root@localhost ~]# vim /etc/httpd/conf.d/zabbix.conf				#修改apache配置文件
php_value date.timezone Asia/Shanghai						   #修改时区为上海
[root@localhost ~]# systemctl start httpd						   #启动apache


-----------------------------------------------------------------------------------------
###直接访问ip/zabbix即可
例如：192.168.1.100/zabbix（图形安装这里不做教程）
初次登陆可以修改界面为中文，很简单，直接点击右上角用户--Language--chinese(zh_CN)--update即可

```

## 2.3 Docker最小化安装zabbix服务端

使用内置 MySQL 数据库、Zabbix server、基于 Nginx Web 服务器的 Zabbix Web 界面和 Zabbix Java gateway 来运行 Zabbix 应用。

```shell
[root@localhost ~]# docker run --name zabbix-appliance -t \
      					-p 10051:10051 \
      					-p 80:80 \
      					-d zabbix/zabbix-appliance:latest
```

直接通过IP访问，（默认用户Admin,密码zabbix）
修改页面为中文
点击右上角用户--Language--chinese(zh_CN)--update即可

## 2.3 zabbix的专业术语介绍

|     zabbix术语      |             注解             |
| :-----------------: | :--------------------------: |
|    主机（host）     |          被监控主机          |
| 主机组 (host group) |        被监控主机群组        |
|    监控项 (item)    |    agent端的某个被监控值     |
|  触发器 (trigger)   |    与被监控对比及判断的值    |
|    动作 (action)    | 对触发器响应需要做相应的措施 |
|    媒介 (media)     |          动作的方式          |



## 2.4 zabbix的专业术语之间的关系

![](https://raw.githubusercontent.com/wss434631143/picgo/master/img/20190509210125.png)

# **第三章 zabbix_agent部署及监控**

## 3.1 zabbix_agent的部署

```
 rpm -ivh http://repo.zabbix.com/zabbix/4.0/rhel/7/x86_64/zabbix-release-4.0-1.el7.noarch.rpm 
 个别主机需要添加zabbix的yum网络源，云主机不需要。
 [root@localhost ~]# yum  install zabbix-agent -y							#直接yum安装agent
 [root@localhost ~]# vim /etc/zabbix/zabbix_agentd.conf					#agent配置文件
 PidFile=/var/run/zabbix/zabbix_agentd.pid  #pid文件地址
 LogFile=/var/log/zabbix/zabbix_agentd.log  #log文件的地址
 Server=192.168.8.19					  #zabbix_server的IP地址
 ListenIP=192.168.8.22					  #zabbix_agent的监听地址
 ServerActive=192.168.8.19				  #agent主动汇报的一个地址（一般改为server的IP地址）
 Hostname=192.168.8.22					  #agent的用户名（一般可修改为IP）
 Include=/etc/zabbix/zabbix_agentd.d/*.conf #引入有效配置文件
# UnsafeUserParameters=0				  #自定义key
[root@localhost ~]# systemctl  start zabbix-agent							#启动zabbix_agent
[root@localhost ~]# ps -ef |grep zabbix_agent								#查看zabbix_agent

#############################################################################################
可直接在zabbix-server端测试是否能连接zabbix-agent,如下：
[root@localhost ~]# /usr/local/zabbix/bin/zabbix_get  --help				#查看zabbix_get命令
/usr/local/zabbix/bin/zabbix_get -s 192.168.8.22  -p 10050 -k "system.cpu.load[all,avg1]"
/usr/local/zabbix/bin/zabbix_get -s 192.168.8.22  -p 10050 -k "system.hostname"

如有回值，则代表正在监听中
```

> 防火墙及selinux是需要关闭的。

## 3.2 zabbix-web创建主机及基本监控

**创建主机群组**

![](https://raw.githubusercontent.com/wss434631143/picgo/master/img/20190509210207.png)

**创建主机**

![](https://raw.githubusercontent.com/wss434631143/picgo/master/img/20190509210221.png)

**添加主机**

![](https://raw.githubusercontent.com/wss434631143/picgo/master/img/20190509210232.png)

**添加监控项**

![](https://raw.githubusercontent.com/wss434631143/picgo/master/img/20190509210252.png)

**添加图形**

![](https://raw.githubusercontent.com/wss434631143/picgo/master/img/20190509210310.png)

**查看图形**

![](https://raw.githubusercontent.com/wss434631143/picgo/master/img/20190509210322.png)

> =======触发器这里不做图解

## 3.3 自定义监控--UserParameters



> https://www.zabbix.com/documentation/4.0/zh/manual/config/items/userparameters
>
> 官方文档解析

- UserParameters的语法：
- UserParameter=<key>,<command>

key:一个用户参数包含一个key，在添加监控项引用，且它是唯一的。

command：你需要执行的用户参数（此参数是有agent代理执行的命令，最多可以返回512KB数据

## 3.4 UserParameters自定义监控实例

```
在zabbix_agent端操作：
[root@localhost ~]# vim /etc/zabbix/zabbix_agentd.conf 
UserParameter=user-num,cat /etc/passwd |wc -l
[root@localhost ~]# systemctl  restart zabbix-agent


在zabbix_server端测试：
[root@localhost ~]# /usr/local/zabbix/bin/zabbix_get -s 192.168.8.22  -p 10050 -k "user-num"
29

有回值，则代表key是有效的。则可直接去zabbix-web上配置。
```

**配置zabbix-web**



**1.添加监控项**

![](https://raw.githubusercontent.com/wss434631143/picgo/master/img/20190509210409.png)



**2.添加图形**

![](https://raw.githubusercontent.com/wss434631143/picgo/master/img/20190509210422.png)



**3.添加触发器**

![](https://raw.githubusercontent.com/wss434631143/picgo/master/img/20190509210438.png)

![](https://raw.githubusercontent.com/wss434631143/picgo/master/img/20190509210448.png)

**4.最终图形**

![](https://raw.githubusercontent.com/wss434631143/picgo/master/img/20190509210459.png)



**测试：**

> 可通过创建用户超过触发器可告警



```
[root@localhost ~]# useradd ma								#创建用户，使总用户达到触发器值30
[root@localhost ~]# wc -l /etc/passwd |awk '{print $1}'
30
```



**5.触发图形**

![](https://raw.githubusercontent.com/wss434631143/picgo/master/img/20190509210510.png)



# **第四章 Grafana展示zabbix监控数据**



## 4.1 下载安装Grafana

```
 wget https://s3-us-west-2.amazonaws.com/grafana-releases/release/grafana-5.2.4-1.x86_64.rpm 
[root@localhost ~]#yum localinstall grafana-5.2.4-1.x86_64.rpm 
[root@localhost ~]# systemctl start grafana-server
[root@localhost ~]# ps -ef |grep grafana
[root@localhost ~]# netstat -plnt |grep 3000
直接访问IP:3000即可访问
默认账号密码都是admin直接登陆
```

## 4.2 Grafana添加数据源

![](https://raw.githubusercontent.com/wss434631143/picgo/master/img/20190509210534.png)

![](https://raw.githubusercontent.com/wss434631143/picgo/master/img/20190509210552.png)



```
grafana-cli plugins install alexanderzobnin-zabbix-app           #安装zabbix插件
[root@localhost ~]# systemctl restart grafana-server			    #重启Grafana
```

**启用zabbix**

![](https://raw.githubusercontent.com/wss434631143/picgo/master/img/20190509210603.png)

**选择zabbix数据源**

![](https://raw.githubusercontent.com/wss434631143/picgo/master/img/20190509210613.png)

**配置zabbix数据源**

配置数据源选项，要注意Url部分，如果你的Zabbix访问路径为`http://192.168.1.1`，那么Url就填写`http://192.168.1.1/api_jsonrpc.php`；`Zabbbix API details`部分就填写Zabbix的账号密码。 

![](https://raw.githubusercontent.com/wss434631143/picgo/master/img/20190509210640.png)



添加zabbix数据源基础仪表步骤：

 Configuration --Data Sources --zabbix --Dashboards --Import

## 4.3 Grafana自定义出图

![](https://raw.githubusercontent.com/wss434631143/picgo/master/img/20190509210659.png)

