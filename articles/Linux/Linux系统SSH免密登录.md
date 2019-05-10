## 第一章 生成密钥

### 1.1 生成用户默认文件名的密钥

```shell
[root@localhost ~] ssh-keygen -t rsa # root用户下生成root用户的默认密钥
```

### 1.2 生成用户指定文件名的密钥

```shell
[wushuaishuai@localhost ~] ssh-keygen -N "" -b 4096 -t rsa -C "wushuaishuai@qq.com" -f ~/.ssh/wushuaishuai.rsa # wushuaishuai用户下生成指定文件名的密钥
```

### 1.3 ssh-keygen 参数说明

- -N new_passphrase 提供一个新的密语。
- -b bits 指定密钥长度。对于RSA密钥，最小要求768位，默认是2048位。DSA密钥必须恰好是
- -1024位(FIPS 186-2 标准的要求)。
- -t type 指定要创建的密钥类型。可以使用："rsa1"(SSH-1) "rsa"(SSH-2) "dsa"(SSH-2)
- -C comment 提供一个新注释
- -f filename 指定密钥文件名

## 第二章 发送公钥

```shell
[root@localhost ~] ssh-copy-id -i /root/.ssh/id_rsa.pub root@10.40.58.60 # 将root用户的默认密钥发送到对端服务器
[root@localhost ~] su - wushuaishuai
[wushuaishuai@localhost ~] ssh -i ~/.ssh/wushuaishuai.rsa 10.40.58.60 # 将指定文件名的密钥发送到对端服务器
```

>  对端服务器家目录的.ssh目录中会生成authorized_keys公钥文件

## 第三章 免密测试

```shell
[root@localhost ~] ssh 10.40.58.60
[wushuaishuai@localhost ~] ssh 10.40.58.60
```

> 无需输入密钥即可登录

