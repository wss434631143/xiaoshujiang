## 一、安装 Python3
通过下面的链接下载你需要的版本，这里下载3.6.5版本
[https://www.python.org/ftp/python/](https://www.python.org/ftp/python/)
```
wget https://www.python.org/ftp/python/3.6.5/Python-3.6.5.tgz
tar zxvf Python-3.6.5.tgz
cd Python-3.6.5
yum -y install gcc openssl-devel
./configure --with-ssl
make && make install
```

## 二、使用pip3安装 Docker Compose
```
pip3 install --upgrade pip
pip3 install docker-compose
docker-compose -v
```
## 三、参考资料
>centos6安装docker、docker-compose - boosir的博客 - CSDN博客
>[https://blog.csdn.net/boosir/article/details/82732500](https://blog.csdn.net/boosir/article/details/82732500)

