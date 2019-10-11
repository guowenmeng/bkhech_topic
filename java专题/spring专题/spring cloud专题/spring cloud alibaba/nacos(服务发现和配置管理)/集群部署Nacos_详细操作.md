# 集群部署Nacos

> 集群 10.1.0.4；10.1.0.13； 10.1.0.14

## 1.创建目录

> 在三台机器上都执行

```shell
sudo mkdir /data/nacos-server
sudo chown -R kingnet:users /data/nacos-server/
mkdir /data/nacos-server/app
```

## 2.上传部署包
```shell
在10.1.0.4上

#系统安装包
cd /data/nacos-server
sz nacos-server-1.1.3.tar.gz

#jdk
cd /data/nacos-server/app
sz openjdk-11+28_linux-x64_bin.tar.gz

#####本地太慢，远程复制，速度更快(或者让管理员上传)#####
#jdk
scp /data/nacos-server/nacos-server-1.1.3.tar.gz kingnet@10.1.0.13:/data/nacos-server
scp /data/nacos-server/nacos-server-1.1.3.tar.gz kingnet@10.1.0.14:/data/nacos-server
#系统安装包
scp /data/nacos-server/app/openjdk-11+28_linux-x64_bin.tar.gz kingnet@10.1.0.13:/data/nacos-server/app
scp /data/nacos-server/app/openjdk-11+28_linux-x64_bin.tar.gz kingnet@10.1.0.14:/data/nacos-server/app
```

## 3.导入数据库脚本

> 若当前机器上没有安装mysql客户端，则先安装mysql客户端。否则，跳过3.1

- 安装mysql客户端

```shell
#1.安装rpm源
rpm -ivh https://repo.mysql.com//mysql57-community-release-el7-11.noarch.rpm

#2.安装客户端
#可以通过yum搜索
yum search mysql | grep mysql-community-client
#若是64位的话直接安装
yum install mysql-community-client.x86_64

#3.测试
mysql -h10.1.0.16 -unacos -p
```

- 导入脚本

```shell
mysql -h10.1.0.16 -unacos -p wufan_nacos < /data/nacos-server/nacos/conf/nacos-mysql.sql
```

## 4.配置JDK

```shell
#1.解压
tar -zxvf openjdk-11+28_linux-x64_bin.tar.gz

#2.修改配置文件
sudo vim /etc/profile
  #添加
export JAVA_HOME=/data/nacos-server/app/jdk-11
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH

#3.刷新配置文件
sudo -s 
source /etc/profile
好像不行。重新登陆，即可

#4.测试
java -version
```

## 5.部署nacos服务

### 修改配置文件

- 解压安装包

  ```shell
  tar -zxvf nacos-server-1.1.3.tar.gz
  ```

- 修改配置文件application.properties

  ```shell
  vim /data/nacos-server/nacos/conf/application.properties
   #添加
  spring.datasource.platform=mysql
  db.num=1
  db.url.0=jdbc:mysql://10.1.0.16:3306/wufan_nacos?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true
  db.user=nacos
  db.password=WQQ9E1iPF4PL
  ```

- 修改集群配置文件cluster.conf

  ```shell
  cp /data/nacos-server/nacos/conf/cluster.conf.example /data/nacos-server/nacos/conf/cluster.conf 
  && cd /data/nacos-server/nacos/conf/cluster.conf
  && vim cluster.conf

  #删除原来内容，新增下列内容
  10.1.0.4:8848
  10.1.0.14:8848
  10.1.0.13:8848
  ```

> 具体参考同目录下**单机nacos服务部署.md**文档

##6.配置另外两台机器

反复轮训步骤4、步骤5