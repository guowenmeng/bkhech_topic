## Centos7.2下只安装mysql客户端

centos7.2下yum下找不到mysql客户端的rpm包了，需要从官网下载

### 1.安装rpm源

```shell
rpm -ivh https://repo.mysql.com//mysql57-community-release-el7-11.noarch.rpm
```

### 2.安装客户端

```shell
#可以通过yum搜索
yum search mysql | grep mysql-community-client
#若是64位的话直接安装
yum install mysql-community-client.x86_64
```

### 3.测试

```she
mysql -hxxx.xxx.xxx.xx -uuser123 -p
```



> [参看资料] (https://yq.aliyun.com/articles/332325)

