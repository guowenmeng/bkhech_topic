# ��Ⱥ����Nacos

> ��Ⱥ 10.1.0.4��10.1.0.13�� 10.1.0.14

## 1.����Ŀ¼

> ����̨�����϶�ִ��

```shell
sudo mkdir /data/nacos-server
sudo chown -R kingnet:users /data/nacos-server/
mkdir /data/nacos-server/app
```

## 2.�ϴ������
```shell
��10.1.0.4��

#ϵͳ��װ��
cd /data/nacos-server
sz nacos-server-1.1.3.tar.gz

#jdk
cd /data/nacos-server/app
sz openjdk-11+28_linux-x64_bin.tar.gz

#####����̫����Զ�̸��ƣ��ٶȸ���(�����ù���Ա�ϴ�)#####
#jdk
scp /data/nacos-server/nacos-server-1.1.3.tar.gz kingnet@10.1.0.13:/data/nacos-server
scp /data/nacos-server/nacos-server-1.1.3.tar.gz kingnet@10.1.0.14:/data/nacos-server
#ϵͳ��װ��
scp /data/nacos-server/app/openjdk-11+28_linux-x64_bin.tar.gz kingnet@10.1.0.13:/data/nacos-server/app
scp /data/nacos-server/app/openjdk-11+28_linux-x64_bin.tar.gz kingnet@10.1.0.14:/data/nacos-server/app
```

## 3.�������ݿ�ű�

> ����ǰ������û�а�װmysql�ͻ��ˣ����Ȱ�װmysql�ͻ��ˡ���������3.1

- ��װmysql�ͻ���

```shell
#1.��װrpmԴ
rpm -ivh https://repo.mysql.com//mysql57-community-release-el7-11.noarch.rpm

#2.��װ�ͻ���
#����ͨ��yum����
yum search mysql | grep mysql-community-client
#����64λ�Ļ�ֱ�Ӱ�װ
yum install mysql-community-client.x86_64

#3.����
mysql -h10.1.0.16 -unacos -p
```

- ����ű�

```shell
mysql -h10.1.0.16 -unacos -p wufan_nacos < /data/nacos-server/nacos/conf/nacos-mysql.sql
```

## 4.����JDK

```shell
#1.��ѹ
tar -zxvf openjdk-11+28_linux-x64_bin.tar.gz

#2.�޸������ļ�
sudo vim /etc/profile
  #���
export JAVA_HOME=/data/nacos-server/app/jdk-11
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH

#3.ˢ�������ļ�
sudo -s 
source /etc/profile
�����С����µ�½������

#4.����
java -version
```

## 5.����nacos����

### �޸������ļ�

- ��ѹ��װ��

  ```shell
  tar -zxvf nacos-server-1.1.3.tar.gz
  ```

- �޸������ļ�application.properties

  ```shell
  vim /data/nacos-server/nacos/conf/application.properties
   #���
  spring.datasource.platform=mysql
  db.num=1
  db.url.0=jdbc:mysql://10.1.0.16:3306/wufan_nacos?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true
  db.user=nacos
  db.password=WQQ9E1iPF4PL
  ```

- �޸ļ�Ⱥ�����ļ�cluster.conf

  ```shell
  cp /data/nacos-server/nacos/conf/cluster.conf.example /data/nacos-server/nacos/conf/cluster.conf 
  && cd /data/nacos-server/nacos/conf/cluster.conf
  && vim cluster.conf

  #ɾ��ԭ�����ݣ�������������
  10.1.0.4:8848
  10.1.0.14:8848
  10.1.0.13:8848
  ```

> ����ο�ͬĿ¼��**����nacos������.md**�ĵ�

##6.����������̨����

������ѵ����4������5