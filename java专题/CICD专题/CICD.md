# CICD

> Centos7

## Install Docker 

1. ## 使用官方安装脚本自动安装

```shell
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

2. 启动

```shell
systemctl start docker
```

3. docker配置阿里云加速器镜像

   > 前提：注册一个阿里云开发者账号

   来到阿里云容器镜像服务，点镜像加速器，会得到一个自己专属的镜像加速器地址，然后再根据你的系统按照操作文档进行配置就行了

   ![image-20201019151420794](C:\Users\guowm\Desktop\CICD\aliyun_mirror.png)
   
   ```shell
sudo mkdir -p /etc/docker
   sudo tee /etc/docker/daemon.json <<-'EOF'
   {
     "registry-mirrors": ["https://feyh14y0.mirror.aliyuncs.com"]
   }
   EOF
   sudo systemctl daemon-reload
   sudo systemctl restart docker
   ```
   
   4. 使用docker info命令查看一下，确认镜像仓库是不是阿里云镜像
   
      ![image-20201019151801623](C:\Users\guowm\Desktop\CICD\docker_info.png)
   
   5. 通过运行 hello-world 映像来验证是否正确安装了 Docker Engine-Community 
   
      ```shell
      docker run hello-world
      ```
   
      ![image-20201019151920377](C:\Users\guowm\Desktop\CICD\docker_run_hello_world.png)

## Install gitlab

> See： https://docs.gitlab.com/omnibus/docker/

1. 安装

```shell
# 搜索
sudo docker search gitlab
# 下载
sudo docker pull gitlab/gitlab-ce:latest
# 启动
sudo docker run --detach \
  --hostname 192.168.71.190 \
  --publish 8443:443 --publish 80:80 --publish 8022:22 \
  --name gitlab \
  --restart always \
  --volume /srv/gitlab/config:/etc/gitlab \
  --volume /srv/gitlab/logs:/var/log/gitlab \
  --volume /srv/gitlab/data:/var/opt/gitlab \
  gitlab/gitlab-ce:latest

# 查看启动情况 
docker logs -f gitlab
```

2. 配置Gitlab

   第一步：进入容器

   ```shell
   sudo docker exec -it gitlab bash
   ```

   第二步：修改gitlab.rb文件	

   ```shell
   sudo cd /etc/gitlab
   sudo vim gitlab.rb
   ```

   第三步：修改IP和端口

   **该部分内容的修改是为了解决，我们再gitlab创建项目的时候，项目访问地址是容器id的问题**

   ```shell
   // 可以使用/ 来查找关键字，找到指定的内容，然后通过n来下一个查找
   
   // 在gitlab创建项目时候http地址的host(不用添加端口)
   external_url 'http://xx.xx.xx.xx'
   
   // 在gitlab创建项目时候ssh地址的host
   gitlab_rails['gitlab_ssh_host'] = 'xx.xx.xx.xx'(不用添加端口)
   
   # docker run 的时候我们把22端口映射为外部的8022了，这里修改下
   gitlab_rails['gitlab_shell_ssh_port'] = 8022
   ```

   

3. 登陆

   http://192.168.71.190:8090

   重设密码： gitlab123456

   username:  root

   > 默认admin用户是 root





> GitLab 配置 Shared Runners
>
> 条件：**必须使用`root`登录**gitlab 在Admin area Runnner 中 找到需要的token和链接，即可

## Install gitlab runner

> See: https://docs.gitlab.com/runner/install/docker.html



### Use Docker

1. Install the Docker image and start the container

   ```shell
   docker run -d --name gitlab-runner --restart always \
        -v /srv/gitlab-runner/config:/etc/gitlab-runner \
        -v /var/run/docker.sock:/var/run/docker.sock \
        gitlab/gitlab-runner:latest
   ```

   

2. Register the Runner

   ```shell
   gitlab-runner register -n \
     --url http://192.168.71.190/ \
     --registration-token PYyVYCYxsxPWPuGG9vKJ \
     --tag-list socket_binding \
     --executor docker \
     --description "My Docker Runner" \
     --docker-image "docker:19.03.12" \
     --docker-volumes /var/run/docker.sock:/var/run/docker.sock
   ```

   

### Use Rpm

> https://docs.gitlab.com/runner/install/linux-manually.html

1. Download

   ```shell
   curl -LJO https://gitlab-runner-downloads.s3.amazonaws.com/latest/rpm/gitlab-runner_arm64.rpm
   ```

2. Install

   ```shell
   rpm -i gitlab-runner_arm64.rpm
   ```

   

### User binary file

### Install

**Important:** With GitLab Runner 10, the executable was renamed to `gitlab-runner`. If you want to install a version prior to GitLab Runner 10, [visit the old docs](https://docs.gitlab.com/runner/install/old.html).

1. Simply download one of the binaries for your system:

   ```
   # Linux x86-64
   sudo curl -L --output /usr/local/bin/gitlab-runner https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64
   ```

   You can download a binary for every available version as described in [Bleeding Edge - download any other tagged release](https://docs.gitlab.com/runner/install/bleeding-edge.html#download-any-other-tagged-release).

2. Give it permissions to execute:

   ```
   sudo chmod +x /usr/local/bin/gitlab-runner
   ```

3. Create a GitLab CI user:

   ```
   sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash
   ```

4. Install and run as service:

   ```
   sudo gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
   sudo gitlab-runner start
   ```

5. [Register the Runner](https://docs.gitlab.com/runner/register/index.html)

   ```shell
   gitlab-runner register -n \
     --url http://192.168.71.190/ \
     --registration-token PYyVYCYxsxPWPuGG9vKJ \
     --tag-list shell \
     --executor shell \
     --description "My Shell Runner"
   ```

   ```shell
   gitlab-runner register \
     --non-interactive \
     --url http://192.168.71.190/ \
     --registration-token "PYyVYCYxsxPWPuGG9vKJ" \
     --tag-list "docker1" \
     --executor "docker" \
     --description "My Docker Runner" \
     --docker-image "docker:19.03.12" \
     --docker-volumes /var/run/docker.sock:/var/run/docker.sock \
     --run-untagged="true" \
     --locked="false" \
     --access-level="not_protected"
   ```

   



