## docker	

>https://cloud.tencent.com/developer/article/1386100 docker 安装 
>
>https://cloud.tencent.com/developer/article/1622955 docker 及其常用命令 *
>
>https://www.cnblogs.com/Bourbon-tian/p/6867796.html  docker 入门镜像构建
>
>https://blog.csdn.net/bean_business/article/details/108974600 查看修改docker 镜像源
>
>https://cloud.tencent.com/document/product/457/9883 docker 参数适配
>
>https://www.cnblogs.com/25miao/p/11607228.html  将 docker 更新到最新版本
>
>https://blog.csdn.net/hicoldcat/article/details/80802447 docker如何删除none镜像
>
>https://cloud.tencent.com/developer/article/1762661 DockerFIle 文件
>
>https://cloud.tencent.com/developer/article/1540703  无效的<none>Docker镜像

#### docker介绍及相关术语

##### 为什么出现docker

- 以前我们开发项目有专门的开发环境，做测试时有测试环境，而产品上线就会有生产环境，这个过程经常要迁移项目，不同的环境配置可能导致不可预估的错误，要经常性的改动

- 世界陷入了错误，于是上帝说，让Docker来吧，于是一切光明。Dokcer把原始的环境一模一样地复制过来，那么就消除了协作编码时，我的机器能运行，而其他机器不能运行的困境

##### docker 相关术语

Docker主机：安装了Docker程序的主机

客户端：连接docker主机进行操作（与守护进程通信）

仓库：保存各种打包好的软件镜像（笔者理解为软件管家可以下载很多软件包）

镜像：软件打包好的镜像，放在仓库中（笔者理解为安装包）

容器：镜像启动后的实例成为容器（笔者理解安装好的在运行的软件）

##### docker 特点

-  直接使用系统的硬件资源，而不需要虚拟化硬件资源 
-  使用[宿主机](https://cloud.tencent.com/product/cdh?from=10680)的内核而不需要GuestOS，所以新建时无需重新加载内核，因此是秒级 
-  是Client-Server结构的系统，其守护进程运行在主机上，然后通过Socket连接访问，守护进程从客户端接收命令并管理运行在主机上的容器。 

流程：下载镜像--->运行镜像--->产生容器（正在运行的软件）

#### docker 安装部署

#### 部署docker 

注意如果centos默认源的docker 版本在使用dockerfile 构建镜像时会报错`Error parsing reference: "microsoft/dotnet:2.2-aspnetcore-runtime AS base" is not a valid repository/tag: invalid reference format`

~~~shell
# step 1: 安装必要的一些系统工具
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
# Step 2: 添加软件源信息
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
# Step 3: 更新并安装Docker-CE
sudo yum makecache fast
sudo yum -y install docker-ce
# Step 4: 开启Docker服务
sudo service docker start
~~~

#### docker 常用命令

##### Linux

1. 启动docker 服务 

   ~~~
   systemctl daemon-reload
   systemctl start docker
   systemctl enable docker # 开机自启
   ~~~

2. 查询相关

   ~~~shell
   # 列出本地镜像
   docker images 
   #  查找镜像
   docker search xx
   # 列出容器(正在运行) 
   docker ps
   # 列出容器(所有)
   docker ps -a
   # 查看容器日志
   docker logs xx
   ~~~

3. 删除镜像

   ~~~shell
   # 要先停止 
   docker stop XX
   # 删除容器 
   docker rm -f XX
   # 删镜像
   docker rmi -f xx
   ~~~

4. 构建命令

   ~~~shell
   #注意下面有个 . 
   docker build -t edu-admin . 
   ~~~

5. 启动命令

   ~~~shell
    docker run -p 6379:6379 --name redis -v /usr/local/docker/redis.conf:/etc/redis/redis.conf -v /usr/local/docker/data:/data -d redis redis-server /etc/redis/redis.conf --appendonly yes    
    # 不挂载配置文件： 
   docker run --name redis -p 6379:6379 -d --restart=always redis redis-server --appendonly yes --requirepass "这是密码"
   ~~~

   - -p 6379:6379 端口映射：前表示主机部分，：后表示容器部分。
   - --name redis  指定该容器名称，查看和进行操作都比较方便。
   - -v 挂载目录，规则与端口映射相同。
   - -d redis 表示启动的容器是
   - redis-server /etc/redis/redis.conf  以配置文件启动redis，加载容器内的conf文件，最终找到的是挂载的目录/usr/local/docker/redis.conf
   - appendonly yes 开启redis 持久化

6. 其他 

   删除none 镜像 

   ~~~
   docker rmi `docker image ls -f dangling=true -q`
   ~~~

   刷新环境变量

   ~~~
   source /etc/profile
   ~~~

   停止所有容器 

   ~~~
    docker stop $(docker ps -a -q) 
   ~~~

   删除所有停止容器

   ~~~
   docker rm $(sudo docker ps -aq)
   ~~~

   手动拉取镜像

   ~~~
   docker pull xxx
   ~~~

   

##### 删除无效的< none >Docker镜像


~~~
# 直接删除带none的镜像，直接报错了。
docker rmi $(docker images | grep "none" | awk '{print $3}')
~~~



#### Dockerfile 

Dockerfile是用来构建Docker镜像文件的，由一系列的命令和参数构成的脚本。（笔者理解为构建自己的软件包）

构建步骤：

1. 编写Dockerfile文件
2. docker build
3. docker run

centos为例

```javascript
FROM scratch
ADD centos-7-x86_64-docker.tar.xz /

LABEL org.label-schema.schema-version="1.0" \
    org.label-schema.name="CentOS Base Image" \
    org.label-schema.vendor="CentOS" \
    org.label-schema.license="GPLv2" \
    org.label-schema.build-date="20191001"

CMD ["/bin/bash"]
```

- 每条指令都会创建一个新的镜像层，并对镜像进行提交
- 从基础镜像运行一个容器
- 执行一条容器并对容器作出修改
- 执行类型commit的操作提交一个新的镜像层
- docker再基于刚提交的镜像运行一个新容器
- 执行dockerfile中的下一条指令直到所有指令都执行完成docker

**这里只是简单说一下：我们可以通过编写Dockerfile文件来自定义自己需要的镜像**

#### 构建例子

#### 编写配置文件

~~~dockerfile
# VERSION 0.0.1
# Author: eangulee
# 基础镜像使用java
FROM java:8
# VOLUME 指定了临时文件目录为/tmp。
# 其效果是在主机 /var/lib/docker 目录下创建了一个临时文件，并链接到容器的/tmp
VOLUME /tmp 
# 将jar包添加到容器中并更名为app.jar
ADD edu-admin.jar edu-admin.jar
# 运行jar包
RUN bash -c 'touch /app.jar'
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/edu-admin.jar"]
~~~

参数解释

VOLUME 指定了临时文件目录为/tmp。其效果是在主机 /var/lib/docker 目录下创建了一个临时文件，并链接到容器的/tmp。该步骤是可选的，如果涉及到文件系统的应用就很有必要了。/tmp目录用来持久化到 Docker 数据文件夹，因为 Spring Boot 使用的内嵌 Tomcat 容器默认使用/tmp作为工作目录
 项目的 jar 文件作为 “app.jar” 添加到容器的
 ENTRYPOINT 执行项目 app.jar。为了缩短 Tomcat 启动时间，添加一个系统属性指向 “/dev/./urandom” 作为 Entropy Source

如果是第一次打包，它会自动下载java 8的镜像作为基础镜像，以后再制作镜像的时候就不会再下载了。



#### docker 网络:host模式

>https://www.cnblogs.com/freeaihub/p/13197292.html  docker 网络:host模式

#### docker 无法关闭

>https://blog.csdn.net/qq_38837032/article/details/119863238 


