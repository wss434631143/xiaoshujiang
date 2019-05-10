## 一、通过EPEL源 yum 安装
```
yum -y install epel-release
yum -y install docker-compose
docker-compose --version
```
## 二、下载二进制包安装
```
curl -L https://get.daocloud.io/docker/compose/releases/download/1.23.2/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
docker-compose --version
```
你可以通过修改URL中的版本，可以自定义您的需要的版本。

## 三、参考资料
>DaoCloud | 安装 Docker Compos
>[http://get.daocloud.io/#install-compose](http://get.daocloud.io/#install-compose)
>
>Install Docker Compose | Docker Documentation
>[https://docs.docker.com/compose/install/#install-compose](https://docs.docker.com/compose/install/#install-compose)

