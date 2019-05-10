# 第一章 Ansible概述

## 1.1 Ansible的介绍

1. 简单易用的一款工具
2. 支持多节点发布、远程任务执行 . 
3. 无代理架构，基于ssh通信，无需在agent端安装代理
4. 能够调用特定的模块来完成特定任务 
5. 支持自定义模块功能 
6. 支持playbook剧本，连续任务按先后设置顺序完成

## 1.2 Ansible的必要性

- [x] 解决很多局限性环境问题
- [x] 节省时间，提高工作效率
- [x] 减少出错故障
- [x] 减少重复操作
- [x] 加强运维部署操作规范性

## 1.3 Ansible与同类工具的对比

- ansible:精简和快速,无需安装代理
- saltstack:命令行的工具，不同级别主服务器，但系统兼容较差
- puppet:长久，全面。但使用复杂。



# 第二章 Ansible的安装配置与简单使用

## 2.1 Ansible的安装

Ansible的安装部署非常简单，其仅依赖于 Python 和 SSH，而系统默认均已安装。

Ansible 被 RedHat 红帽官方收购后，其安装源被收录在 epel 中，如已安装 epel 可直接 yum 安装。

通过 pip 和 easy_install 的 Python 第三方包管理器也可以便捷安装 Ansible。

>  推荐使用yum方式安装。

### 2.1.1 yum方式安装

```
[root@localhost]# yum install epel-release -y   # 安装epel源
[root@localhost]# yum install ansible -y        # yum安装ansible即可，非常简单方便
[root@localhostins]# ansible --version          # 验证安装是否成功
[root@localhost]# ansible help               # help可以查看ansible的命令参数
```

### 2.1.2 pip方式安装

```
[root@localhost]# yum install gcc glibc-devel zlib-devel rpm-build openssl-devel -y  # 安装前确保服务器的gcc、glibc开发环境均已安装
[root@localhost]# yum install python-pip python-devel -y # 安装 python-pip 及 python-devel 程序包
[root@localhost]# pip install --upgrade pip # 升级本地 pip 至最新版本 
[root@localhost]# pip install --upgrade ansible # 安装ansible
```

## 2.2 Ansible的配置

|         配置文件         |          作用          |
| :----------------------: | :--------------------: |
| /etc/ansible/ansible.cfg |       主配置文件       |
|    /etc/ansible/hosts    | 机器清单，进行分组管理 |
|   /etc/ansible/roles/    |     存放角色的目录     |



```shell
[root@localhost]# vim /etc/ansible/ansible.cfg   # 修改主配置文件
```

```shell
[defaults]
#inventory      = /etc/ansible/hosts  					# 默认主机文件
#library        = /usr/share/my_modules/                # 库文件存放目录
#forks          = 5                                     # 默认开启的并发数
#remote_user = root                                     # 默认palybooks用户 
#sudo_user      = root                                  # 默认sudo用户
#ask_sudo_pass = True                                   # 是否需要sudo密码
#ask_pass      = True                                   # 是否需要密码
#remote_port    = 22                                    # 默认远程主机的端口号
#host_key_checking = False                              # 是否检查host_key，推荐关闭
#deprecation_warnings=False      # 是否开启“不建议使用”警告，该类型警告大多属于版本更新方法过时提醒
#command_warnings=False      # 当shell和命令行模块被默认模块简化的时,Ansible 将默认发出警告
#timeout = 10											# 连接主机的时间
#log_path=/var/log/ansible.log                          # 开启ansible日志
#private_key_file = /path/to/file（/root/.ssh/id_rsa）   # 基于密钥认证的文件
[privilege_escalation]                                  # 默认以root身份运行ansible
become=True
become_method=sudo
become_user=root
become_ask_pass=False
```

```shell
[root@localhost]# vim /etc/ansible/hosts		   #修改主机清单配置文件（Inventory）
```

```shell
# Ex 1: Ungrouped hosts, specify before any group headers.	#未分组的主机清单
## green.example.com									    #定义主机为主机名
## blue.example.com
## 192.168.100.1										    #定义主机为IP
## 192.168.100.10
```

```shell
## [webservers]											#组名为webservers的主机
## alpha.example.org									#定义主机为主机名
## beta.example.org
## 192.168.1.100										#定义主机为IP
## 192.168.1.110
## www[001:006].example.com								#若主机名有规律，也可以范围性的简写
```

```shell
## [dbservers]											#同上，此组名为dbserver
## db01.intranet.mydomain.net
## db02.intranet.mydomain.net
## 10.25.1.56
## 10.25.1.57
## db-[99:101]-node.example.com
```

```shell
## [all:vars]                           # 为所有主机设置变量（可用于首次批量推送密钥） 
## ansible_ssh_user=wushuaishuai        # 用户名
## ansible_ssh_pass=123456              # 密码    
```

> https://docs.ansible.com ansible官方首页可针对性看一些模板和实例

## 2.3 Ansible的简单使用

```
[root@localhost]# vim /etc/ansible/ansible.cfg    # 修改主配置文件
host_key_checking = False								  # 关闭host_key检查
[root@localhost]# vim /etc/ansible/hosts		  # 修改主机清单
[test]
10.0.1.7 ansible_ssh_user=root ansible_ssh_pass=123456 ansible_ssh_port=22
10.0.1.8 ansible_ssh_user=root ansible_ssh_pass=123456 ansible_ssh_port=22
#10.0.1.7 						 # 主机IP
#ansible_ssh_user=root			 # 主机用户
#ansible_ssh_pass=123456	 	 # 主机密码
#ansible_ssh_port=22			 # 主机ssh端口

```

```
[root@localhost]# ansible test -a  "df -h"		    # 批量查看主机的磁盘分区
10.0.1.8 | SUCCESS | rc=0 >>							    # test主机组名称
Filesystem      Size  Used Avail Use% Mounted on		    # -a指定模块参数
/dev/sda2        30G  2.4G   28G   9% /					    # "df -h" 输入的命令
devtmpfs        3.9G     0  3.9G   0% /dev
tmpfs           3.9G     0  3.9G   0% /dev/shm
tmpfs           3.9G  106M  3.8G   3% /run
tmpfs           3.9G     0  3.9G   0% /sys/fs/cgroup
/dev/sda1       497M   81M  417M  17% /boot
/dev/sdb1        40G   49M   38G   1% /mnt/resource
tmpfs           797M     0  797M   0% /run/user/0

10.0.1.7 | SUCCESS | rc=0 >>
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda2        30G  3.4G   27G  12% /
devtmpfs        3.4G     0  3.4G   0% /dev
tmpfs           3.4G     0  3.4G   0% /dev/shm
tmpfs           3.4G  330M  3.1G  10% /run
tmpfs           3.4G     0  3.4G   0% /sys/fs/cgroup
/dev/sda1       497M   81M  417M  17% /boot
/dev/sdb1       281G   65M  267G   1% /mnt/resource
tmpfs           696M     0  696M   0% /run/user/0
```

# 第三章 Ansible常用命令及模块

## 3.1 Ansible的命令参数

- -m：要执行的模块，默认为command
- -a：模块的参数
- **-u：ssh连接的用户名，默认用root，ansible.cfg中可以配置**
- **-b(-become):  变成某个用户来执行操作，默认变的方式是sudo，默认变成root**
- **--become-user： 指定变成某个用户**
- -k(--ask-pass)：提示输入ssh登录密码，当使用密码验证的时候用
- -s(--sudo)：sudo运行
- -U(--sudo-user)：sudo到哪个用户，默认为root
- -K：提示输入sudo密码，当不是NOPASSWD模式时使用
- -C(--check)：只是测试一下会改变什么内容，不会真正去执行
- -c：连接类型(default=smart)
- -f(--forks)：fork多少进程并发处理，默认为5个
- -i：指定hosts文件路径，默认default=/etc/ansible/hosts
- -I：指定pattern，对已匹配的主机中再过滤一次
- --list-host：只打印有哪些主机会执行这个命令，不会实际执行
- -M：要执行的模块路径，默认为/usr/share/ansible
- -o：压缩输出，摘要输出
- --private-key：私钥路径
- -T：ssh连接超时时间，默认是10秒
- -t：日志输出到该目录，日志文件名以主机命名
- -v：显示详细日志

## 3.2 Ansible命令实例

**以wushuaishuai sudo至root用户执行ping存活检测**

```shell
[root@localhost]# ansible all -m ping -u wushuaishuai -b
```

**以wushuaishuai sudo至junpserver用户执行ping存活检测**

```shell
[root@localhost]# ansible all -m ping -u wushuaishuai -b --become-user jumpserver
```

## 3.2 Ansible的常用模块

1. command模块:使用一种自由格式命令 (为系统默认，可省略，不支持管道命令和变量等)
2. shell模块：调用远程主机的指令 （执行脚本，管道命令等）
3. yum模块：管理程序包 
4. service模块：管理服务
5. ping模块：测试指定主机是否能连接 
6. copy、file模块：文件管理（创建，复制等）
7. setup模块：收集远程主机的信息

# 第四章 Playbook的介绍

## 4.1 Playbook介绍

Playbooks （剧本）与 ad-hoc（临时任务） 相比,是一种完全不同的运用 ansible 的方式,是非常之强大的. 

Playbooks：是一种简单的配置管理系统 ，适合管理配置应用及部署复杂的工作。基于易读的YAML编程语言。

## 4.2 YAML语法介绍

- [x] 大小写敏感 
- [x] 使用缩进表示层级关系 
- [x] 缩进时不允许使用Tab键，只允许使用空格
- [x] yaml文件以"---"作为文档的开始
- [x] "#"表示注释 

# 第五章 playbook应用之ssh密钥分发

## 5.1 确认关闭检查host_key

```shell
[root@localhost]# vim /etc/ansible/ansible.cfg
host_key_checking = False    
```

 ## 5.2 生成管理主机的私钥和公钥

```
[root@localhost]# ssh-keygen -t rsa -b 2048 -P '' -f /root/.ssh/id_rsa # root用户
[root@localhost]# ssh-keygen -t rsa -b 2048 -P '' -f /home/wushuaishuai/.ssh/id_rsa # 也可是其他用户
```

### 5.3 添加主机信息到主机清单中

```shell
[root@localhost ansible]# vim /etc/ansible/hosts
# hosts
[web]
192.168.77.129 ansible_ssh_pass=1234567
192.168.77.130 ansible_ssh_pass=123456
```

### 5.4 编写并运行playbook

```shell
[root@localhost]# vim ssh-addkey.yml
```

```yaml
# ssh-addkey.yml 
---
- hosts: all
  gather_facts: no

  tasks:

  - name: install ssh key
    authorized_key: user=root 
                    key="{{ lookup('file', '/home/wushuaishuai/.ssh/id_rsa.pub') }}" 
                    state=present
```

```shell
[root@localhost]# ansible-playbook -i hosts ssh-addkey.yml
```

这样，管理节点的公钥就会添加到节点的authorized_keys文件中，再把主机清单里的ansible_ssh_pass去掉，执行ansible all -m ping 就不需要密码了。

> 注意：需要切换到你生成密钥的用户下才能免密去执行playbook



# 第六章 playbook应用之部署nginx

## 6.1 编写playbook

```shell
[root@localhost]# vim nginx.yml
```

```yaml
# nginx.yml
---
- hosts: test
  vars:
    hello: Ansible
     
  tasks:
  - name: yum repo 
    yum_repository:
      name: nginx
      description: nginx repo
      baseurl: http://nginx.org/packages/centos/7/$basearch/
      gpgcheck: no
      enabled: 1
  - name: Install nginx
    yum:
      name: nginx
      state: latest
  - name: Copy nginx configuration file
    copy:
      src: /etc/ansible/ansible-playbook/ansible.conf
      dest: /etc/nginx/conf.d/site.conf
  - name: Start nginx
    service:
      name: nginx
      state: started
  - name: Create wwwroot directory
    file:
      dest: /var/www/html
      state: directory
  - name: Create test page index.html
    shell: echo "hello {{hello}}" > /usr/share/nginx/html/index.html
```

> 附加：需要创建一个nginx的新配置文件，如下：

```
[root@localhost]# vim ansible.conf        #在当前目录下创建nginx新配置文件
server {
    listen 81;
    server_name localhost;
    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }
}
```

##  6.2 添加主机信息到主机清单中

```shell
[root@localhost]# vim /etc/ansible/hosts
[test]
10.0.1.7 ansible_ssh_user=root ansible_ssh_pass=123456 ansible_ssh_port=22
10.0.1.8 ansible_ssh_user=root ansible_ssh_pass=123456 ansible_ssh_port=22
```

## 6.3 运行playbook批量安装nginx

```shell
[root@localhost ansible-playbook]# ansible-playbook nginx.yml 

PLAY [test] ***************************************************************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************************************************
ok: [10.0.1.8]
ok: [10.0.1.7]

TASK [yum repo] ***********************************************************************************************************************************************
ok: [10.0.1.8]
ok: [10.0.1.7]

TASK [Install nginx] ******************************************************************************************************************************************
changed: [10.0.1.8]
changed: [10.0.1.7]

TASK [Copy nginx configuration file] **************************************************************************************************************************
ok: [10.0.1.8]
ok: [10.0.1.7]

TASK [Start nginx] ********************************************************************************************************************************************
changed: [10.0.1.8]
changed: [10.0.1.7]

TASK [Create wwwroot directory] *******************************************************************************************************************************
ok: [10.0.1.8]
ok: [10.0.1.7]

TASK [Create test page index.html] ****************************************************************************************************************************
changed: [10.0.1.8]
changed: [10.0.1.7]

PLAY RECAP ****************************************************************************************************************************************************
10.0.1.7                   : ok=7    changed=3    unreachable=0    failed=0   
10.0.1.8                   : ok=7    changed=3    unreachable=0    failed=0
```

到agent端查看，nginx安装完成，访问nginx  ip:81，页面显示hello Ansible ！！！

