### Centos7搭建RabbitMQ



#### 一系统环境 

1、JDK1.8(卸载Open JDK)
2、Centos7-64位
3、Erlang-OTP 23
4、RabbitMQ-3.8.5



#### 二 更换镜像源

1. 备份镜像源

   ~~~
   mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
   ~~~

2. 下载重命名为阿里源

   ~~~
   wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
   wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
   ~~~

3. 清理并生成新缓存

   ~~~
   yum clean all
   yum makecache
   ~~~



#### 三 安装Erlang

1. erlang和rabbitmq需要对应 

   [对应关系](https://www.rabbitmq.com/which-erlang.html)

2. 通过rpm安装erlang

   1.完成erlang的前置条件配置

   ```
   curl -s https://packagecloud.io/install/repositories/rabbitmq/erlang/script.rpm.sh | sudo bash 
   ```

   2.安装erlang

   ```
   yum install -y erlang
   ```

   3）检查erlang的版本号

   ```
   erl
   ```



#### 四 安装RabbitMQ

1. 先导入两个key

   ~~~
   rpm --import https://packagecloud.io/rabbitmq/rabbitmq-server/gpgkey
   rpm --import https://packagecloud.io/gpg.key
   ~~~

2. 完成RabbitMQ的前置条件配置

   ~~~
   curl -s https://packagecloud.io/install/repositories/rabbitmq/rabbitmq-server/script.rpm.sh | sudo bash
   ~~~

3. 下载RabbitMQ安装包

   注意看CentOS的版本，6,7,8都有。我这里是7.4。有时候直接点击浏览器下载可能会很慢，可以F12，找到链接，在centos里面去使用wget下载，可能会很快。这里给出Centos7和Centos8的下载链接。

   [CentOS7](https://github.com/rabbitmq/rabbitmq-server/releases/download/v3.8.5/rabbitmq-server-3.8.5-1.el7.noarch.rpm)

   [CentOS8](https://github.com/rabbitmq/rabbitmq-server/releases/download/v3.8.5/rabbitmq-server-3.8.5-1.el8.noarch.rpm)

4. 下载成功后，上传到服务器，然后使用命名安装。

   ```
   rpm -ivh rabbitmq-server-3.8.5-1.el7.noarch.rpm
   ```

   仔细看有一个警告和一个错误。警告是缺少key，而错误是socat，只需要导入key和安装socat即可。
   1.导入key

   ```
   rpm --import https://www.rabbitmq.com/rabbitmq-release-signing-key.asc
   ```
   2.安装socat

   ```
   yum -y install epel-release
   yum -y install socat
   ```

5. 再次安装RabbitMQ

   ```
   rpm -ivh rabbitmq-server-3.8.5-1.el7.noarch.rpm
   ```

6. 启用管理平台插件，启用插件后，可以可视化管理RabbitMQ。

   ```
   rabbitmq-plugins enable rabbitmq_management
   ```

7. 启动RabbitMQ

   ```
   systemctl start rabbitmq-server
   ```



#### 四、访问控制台界面

1. 访问地址
   http://127.0.0.1:15672

2. 用户登录
   默认账号密码都是guest，但是如果使用guest登录

   至此安装完成



#### 五 CentOS7卸载 OpenJDK 安装Sun的JDK8

>注意权限问题 

##### 一  卸载 openjdk

 1. 查看 所有包含java字符串文件

    ~~~
    rpm -qa | grep java
    ~~~

    将会得到以下文件(加入你是一个干净的环境)

    ~~~
    java-1.7.0-openjdk-1.7.0.111-2.6.7.8.el7.x86_64
    tzdata-java-2016g-2.el7.noarch
    python-javapackages-3.4.1-11.el7.noarch
    java-1.8.0-openjdk-1.8.0.102-4.b14.el7.x86_64
    java-1.8.0-openjdk-headless-1.8.0.102-4.b14.el7.x86_64
    javapackages-tools-3.4.1-11.el7.noarch
    java-1.7.0-openjdk-headless-1.7.0.111-2.6.7.8.el7.x86_64
    ~~~

    我们删除

    ~~~
    java-1.7.0-openjdk-1.7.0.111-2.6.7.8.el7.x86_64
    java-1.8.0-openjdk-1.8.0.102-4.b14.el7.x86_64
    java-1.8.0-openjdk-headless-1.8.0.102-4.b14.el7.x86_64
    java-1.7.0-openjdk-headless-1.7.0.111-2.6.7.8.el7.x86_64
    ~~~

    删除命令

    ~~~
    rpm -e --nodeps java-1.7.0-openjdk-1.7.0.111-2.6.7.8.el7.x86_64
    rpm -e --nodeps java-1.8.0-openjdk-1.8.0.102-4.b14.el7.x86_64
    rpm -e --nodeps java-1.8.0-openjdk-headless-1.8.0.102-4.b14.el7.x86_64
    rpm -e --nodeps java-1.7.0-openjdk-headless-1.7.0.111-2.6.7.8.el7.x86_64
    ~~~

    检查是否删除成功

    ~~~
    java -version
    ~~~

    若还没有删除，使用

    ~~~
    yum -y remove
    ~~~

##### 二 下载  jdk

1. 历史版本下载(注意系统位数)

   ~~~
   地址: http://www.oracle.com/technetwork/java/javase/archive-139210.html   
   ~~~

2. 拷贝下载好的文件到/usr/java

   >注意此处是否有 java文件夹 
   >
   >注意使用管理员权限

   ~~~
   cp jdk-8u291-linux-x64.tar.gz /usr/java
   ~~~

3. 解压

   ~~~
   tar -zxvf jdk-8u291-linux-x64.tar.gz 
   ~~~

4. 删除jdk压缩包

   ~~~
   rm -f jdk-8u291-linux-x64.tar.gz 
   ~~~



#####  三 配置JDK环境变量 

1. 编辑环境变量(还是注意权限问题)

   ~~~
   vim /etc/profile
   ~~~

2. 进入插入状态：在文本的最后一行粘贴如下

   > 注意JAVA_HOME=/usr/java/jdk1.8.0_144  就是你自己的目录

   ~~~
   #java environment
   export JAVA_HOME=/usr/java/jdk1.8.0_291
   export CLASSPATH=.:${JAVA_HOME}/jre/lib/rt.jar:${JAVA_HOME}/lib/dt.jar:${JAVA_HOME}/lib/tools.jar
   export PATH=$PATH:${JAVA_HOME}/bin
   ~~~

   保存后退出

   此处需要熟悉一些常用的vim操作

3. 环境变量生效

   输入

   ~~~
   source /etc/profile
   ~~~

   检查是否配置成功

   ~~~
   java -version
   ~~~

over~
