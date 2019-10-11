# nacos部署

## 一 单机

### 1.预备环境准备

Nacos 依赖 [Java](https://docs.oracle.com/cd/E19182-01/820-7851/inst_cli_jdk_javahome_t/) 环境来运行。如果您是从代码开始构建并运行Nacos，还需要为此配置 [Maven](https://maven.apache.org/index.html)环境，请确保是在以下版本环境中安装使用:

1. 64 bit OS，支持 Linux/Unix/Mac/Windows，推荐选用 Linux/Unix/Mac。
2. 64 bit JDK 1.8+；[下载](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) & [配置](https://docs.oracle.com/cd/E19182-01/820-7851/inst_cli_jdk_javahome_t/)。
3. Maven 3.2.x+；[下载](https://maven.apache.org/download.cgi) & [配置](https://maven.apache.org/settings.html)。

### 2.下载源码或者安装包

你可以通过源码和发行包两种方式来获取 Nacos。

#### 从 Github 上下载源码方式

```shell
git clone https://github.com/alibaba/nacos.git
cd nacos/
mvn -Prelease-nacos clean install -U  
ls -al distribution/target/

// change the $version to your actual path
cd distribution/target/nacos-server-$version/nacos/bin
```

#### 下载编译后压缩包方式

您可以从 [最新稳定版本](https://github.com/alibaba/nacos/releases) 下载 `nacos-server-$version.zip` 包。

```shell
  unzip nacos-server-$version.zip 或者 tar -xvf nacos-server-$version.tar.gz
  cd nacos/bin
```

### 3.启动服务器

#### Linux/Unix/Mac

启动命令(standalone代表着单机模式运行，非集群模式):

`sh startup.sh -m standalone`

#### Windows

启动命令：

`cmd startup.cmd`

或者双击startup.cmd运行文件。

#### 单机模式支持mysql

在0.7版本之前，在单机模式时nacos使用嵌入式数据库(derby)实现数据的存储，不方便观察数据存储的基本情况。0.7版本增加了支持mysql数据源能力，具体的操作步骤：

- 1.安装数据库，版本要求：5.6.5+
- 2.初始化mysql数据库，数据库初始化文件：nacos-mysql.sql
- 3.修改conf/application.properties文件，增加支持mysql数据源配置（目前只支持mysql），添加mysql数据源的url、用户名和密码。

### 6.服务注册&发现和配置管理

### 

### 5.关闭服务器

#### Linux/Unix/Mac

`sh shutdown.sh`

#### Windows

`cmd shutdown.cmd`

或者双击shutdown.cmd运行文件。

## 二 集群

### 1. 预备环境准备

请确保是在环境中安装使用:

1. 64 bit OS Linux/Unix/Mac，推荐使用Linux系统。
2. 64 bit JDK 1.8+；[下载](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).[配置](https://docs.oracle.com/cd/E19182-01/820-7851/inst_cli_jdk_javahome_t/)。
3. Maven 3.2.x+；[下载](https://maven.apache.org/download.cgi).[配置](https://maven.apache.org/settings.html)。
4. 3个或3个以上Nacos节点才能构成集群。

### 2.下载源码或者安装包

你可以通过两种方式来获取 Nacos。

#### 从 Github 上下载源码方式

```shell
unzip nacos-source.zip
cd nacos/
mvn -Prelease-nacos clean install -U  
cd nacos/distribution/target/nacos-server-0.8.0/nacos/bin
```

#### 下载编译后压缩包方式

下载地址

[zip包](https://github.com/alibaba/nacos/releases/download/0.8.0/nacos-server-0.8.0.zip)

[tar.gz包](https://github.com/alibaba/nacos/releases/download/0.8.0/nacos-server-0.8.0.tar.gz)

```shell
  unzip nacos-server-0.8.0.zip 或者 tar -xvf nacos-server-0.8.0.tar.gz
  cd nacos/bin
```

### 3. 配置集群配置文件

在nacos的解压目录nacos/的conf目录下，有配置文件cluster.conf，请每行配置成ip:port。（请配置3个或3个以上节点）

```she
# ip:port
200.8.9.16:8848
200.8.9.17:8848
200.8.9.18:8848
```

### 4. 配置 MySQL 数据库

生产使用建议至少主备模式，或者采用高可用数据库。

#### 初始化 MySQL 数据库

[sql语句源文件](https://github.com/alibaba/nacos/blob/master/distribution/conf/nacos-mysql.sql)

#### application.properties 配置

[application.properties配置文件](https://github.com/alibaba/nacos/blob/master/distribution/conf/application.properties)

### 5. 启动服务器

#### Linux/Unix/Mac

启动命令(在没有参数模式，是集群模式):

`sh startup.sh`

### 6. 服务注册&发现和配置管理

### 7. 关闭服务器

`sh shutdown.sh`





> 详情前往：[官方文档](https://nacos.io/zh-cn/docs/quick-start.html)

