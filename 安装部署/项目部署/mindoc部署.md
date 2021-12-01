[toc]

### mindoc 及 Go部署



#### Linux部署mindoc

服务器上没有安装golang程序(采用已经编译好的项目部署)

- 配置环境变量，键名为 ZONEINFO，值为MinDoc根目录下的/lib/time/zoneinfo.zip 绝对路径

  在 `/etc/profile`文件中添加变量

  ~~~shell
  export ZONEINFO=..yourpath/lib/time/zoneinfo.zip
  ~~~

  然后执行

  ~~~shell
  source /etc/profile
  ~~~

- 下载编译好的最新版的可执行文件，一般文件名为 mindoc_linux_amd.tar.gz 或 mindoc_linux_amd64.zip 

  ~~~
  tar -xzvf mindoc_linux_amd64.tar.gz
  或
  unzip mindoc_linux_amd64.zip
  ~~~

- 如果你使用的 mysql 数据库，请创建一个编码为utf8mb4格式的数据库，如果没有GUI管理工具，推荐用下面的脚本创建：

  ~~~
  CREATE DATABASE mindoc_db  DEFAULT CHARSET utf8mb4 COLLATE utf8mb4_general_ci;
  ~~~

- 请将刚才解压目录下 conf/app.conf.example 重名为 app.conf:

  ```bash
  cp conf/app.conf.example conf/app.conf
  ```

  同时配置如下节点：

  ```ini
  #数据库配置
  db_adapter=mysql
  #mysql数据库的IP
  db_host=127.0.0.1
  
  #mysql数据库的端口号一般为3306
  db_port=3306
  
  #刚才创建的数据库的名称
  db_database=mindoc_db
  
  #访问数据库的账号和密码
  db_username=root
  db_password=123456
  ```

  在 MinDoc 根目录下使用命令行执行如下命令，用于初始化数据库：

  ```bash
  ./mindoc_linux_amd64 install
  ```

  此时出现错误

  ![image-20210908144942472](C:\Users\12980\Pictures\typora图片\image-20210908144942472.png)

  需要将GLIBC升级到2.28

- CentOS7下升级GLIBC 到2.28

  1. 当前版本的GLIBC版本信息

     ~~~shell
     cat /etc/redhat-release 
     #CentOS Linux release 7.6.1810 (Core) 
     
     uname -r
     #3.10.0-957.el7.x86_64
     
     strings /lib64/libc.so.6 | grep GLIBC
     
     ll /lib64/libc.so*
     ~~~
  
  2. 源码编译升级gcc9.3.0 
  
     ~~~shell
     wget https://mirrors.aliyun.com/gnu/gcc/gcc-9.3.0/gcc-9.3.0.tar.gz
     cp gcc-9.3.0.tar.gz /opt
     cd /opt
     tar -zxf gcc-9.3.0.tar.gz
     cd gcc-9.3.0/
     
      ./contrib/download_prerequisites
     #建议先手动下载依赖的这四个包，下载地址ftp://gcc.gnu.org/pub/gcc/infrastructure/
     cat /proc/cpuinfo| grep "processor"| wc -l
     
     mkdir build
     cd build
     ../configure --enable-checking=release --enable-language=c,c++ --disable-multilib --prefix=/usr
     
     #以下两步耗时很多
     make -j6
     make install
     ~~~
  
     检查gcc版本 
  
     ~~~shell
     cd /usr/lib64
     ll libstdc++*
     gcc -v
     gcc --version
     ~~~
  
  3. 源码编译升级make
  
     编译升级make
  
     ~~~shell
     wget https://mirrors.aliyun.com/gnu/make/make-4.3.tar.gz
     cp make-4.3.tar.gz /opt
     cd /opt/
     tar -zxf make-4.3.tar.gz 
     cd make-4.3/
     mkdir build
     cd build
     ../configure --prefix=/usr && make && make install
     ~~~
  
     检查升级后的make信息
  
     ~~~shell
     make -v
     ~~~
  
  4. 升级glibc到2.28
  
     ~~~shell
     cd /opt
     wget https://mirrors.aliyun.com/gnu/glibc/glibc-2.28.tar.gz
     tar -zxf glibc-2.28.tar.gz
     cd glibc-2.28/
     cat INSTALL | grep -E "newer|later"
     mkdir build
     cd build
     ../configure  --prefix=/usr --disable-profile --enable-add-ons --with-headers=/usr/include --with-binutils=/usr/bin --disable-sanity-checks --disable-werror
     ~~~
  
     在此处可能会报错
  
     ![image-20210908191443207](C:\Users\12980\Pictures\typora图片\image-20210908191443207.png)
  
     升级python版本，然后继续执行
  
     ~~~shell
      yum install python3
      ../configure  --prefix=/usr --disable-profile --enable-add-ons --with-headers=/usr/include --with-binutils=/usr/bin --disable-sanity-checks --disable-werror
     
      make -j6
     
      make install
     ~~~
  
     注意 由于glibc 更新后 系统不会安装 设置的 字符编码，比如我系统上设置的是字符编码是en_US.UTF-8，而系统并没有安装en_US.UTF-8字符编码.
  
     所以需要继续执行
  
     ~~~shell
      make localedata/install-locales
     ~~~
  
     验证gblic版本
  
     ~~~shell
      strings /lib64/libc.so.6 | grep GLIBC
      ll /lib64/libc.so*
     ~~~
  
     ![image-20210908192030906](C:\Users\12980\Pictures\typora图片\image-20210908192030906.png)

![image-20210908192135060](C:\Users\12980\Pictures\typora图片\image-20210908192135060.png)

- 再回到MinDoc目录下使用命令 

  ~~~shell
   #MinDoc 根目录下使用命令行执行如下命令，用于初始化数据库：
   ./mindoc_linux_amd64 install
  ~~~

  稍等一分钟，程序会自动初始化数据库，并创建一个超级管理员账号：`admin` 密码：`123456`

- 启动程序

  执行如下命令启动程序：

  ```shell
  #修改可执行权限
  chmod +x mindoc_linux_amd64
  
  #启动程序
  ./mindoc_linux_amd64
  ```

  此时访问 [http://localhost:8181](http://localhost:8181/) 就能访问 MinDoc 了。





#### windows 配置midnoc

服务器上没有安装golang程序(采用已经编译好的项目部署)

- 配置环境变量，键名为 ZONEINFO，值为MinDoc根目录下的/lib/time/zoneinfo.zip 绝对路径

  在 `/etc/profile`文件中添加变量

  ①我的电脑->鼠标右键->属性->高级系统设置->环境变量

  ![image-20210909102727337](C:\Users\12980\Pictures\typora图片\image-20210909102727337.png)

  ②Windows平台，有时候设置了这个环节变量，还是会启动失败，此时只需要将这个文件放到此位置`C:\go\lib\time\zoneinfo.zip`

- 下载编译好的最新版的可执行文件，一般文件名为 一般文件名为 mindoc_windows_amd.zip .

  之后将刚才下载的文件解压，可解压到任意目录。建议不用用中文目录名称。

- 如果你使用的 mysql 数据库，请创建一个编码为utf8mb4格式的数据库，如果没有GUI管理工具，推荐用下面的脚本创建：

  ~~~shell
  CREATE DATABASE mindoc_db  DEFAULT CHARSET utf8mb4 COLLATE utf8mb4_general_ci;
  ~~~

- 配置数据库

  请将刚才解压目录下 conf/app.conf.example 重名为 app.conf。同时配置如下节点：

  ~~~shell
  #数据库配置
  db_adapter=mysql
  #mysql数据库的IP
  db_host=127.0.0.1
  
  #mysql数据库的端口号一般为3306
  db_port=3306
  
  #刚才创建的数据库的名称
  db_database=mindoc_db
  
  #访问数据库的账号和密码
  db_username=root
  db_password=123456
  ~~~

  在 MinDoc 根目录下使用命令行执行如下命令，用于初始化数据库：

  ```bash
  mindoc_windows_amd64.exe install
  ```

  此时 会爆出错误 `exec:"gcc" executable file not found in %PATH%`，缺少gcc编译器

- 下载gcc编译器

  1. 下载符合系统版本的压缩包--->[传送门](https://sourceforge.net/projects/mingw-w64/files/mingw-w64/)

     > 我本机下载第一次下载是exe包，发现发现安装时加载所需要依来需要翻墙。
     >
     > 所以此连接直接下载的是解压后的文件。 
     
     我的是64位，下载此版本。

  

  ![image-20210909103635861](C:\Users\12980\Pictures\typora图片\image-20210909103635861.png)
  
  2.  直接解压以后 , 把bin目录放入 PATH环境变量就行了
  
     ![image-20210909104108989](C:\Users\12980\Pictures\typora图片\image-20210909104108989.png)

- 再次安装后 显示时区错误

  <img src="C:\Users\12980\Pictures\typora图片\image-20210909104439440.png" alt="image-20210909104439440" style="zoom:67%;" />

  问题出现在代买模块，`LoadLocation` 它依赖于 IANA Time Zone Database (此处简称 tzdata ) 这个数据库，一般linux系统都带了，但是windows系统就没带。所以如果windows系统没有安装go环境，调用LoadLocation就会报错。高版本go会自带。

  1. 我们可以自己把tzdata文件放到自己的程序目录中，然后让 time 包能够从我们自己的程序目录中加载时区文件就可以了。

     此时即可正常运行

     [下载tzdata](https://www.helay.net/public/source/tzdata.zip)

- 加载好数据后，显示如下界面即为成功

  <img src="C:\Users\12980\Pictures\typora图片\image-20210909105154471.png" alt="image-20210909105154471" style="zoom:67%;" />

- 启动程序

  ①设置了环境变量，但是没有重启电脑，请在cmd命令行启动 mindoc_windows_amd64.exe 程序。

  ②如果你设置了环境变量，并且重启了电脑，双击 mindoc_windows_amd64.exe 即可。

  ③此时访问 [http://localhost:8181](http://localhost:8181/)即可访问mindoc(假设你没有改端口)

#### centos安装go环境

- 下载安装包 

  ~~~shell
  wget -c https://storage.googleapis.com/golang/go1.11.5.linux-amd64.tar.gz
  ~~~

- 解压

  >将源码包解压后直接放到 /usr/local 目录下,可根据自己需要更改

  ~~~shell
  tar -C /usr/local/ -zxvf go1.11.5.linux-amd64.tar.gz 
  ~~~

- 添加系统环境变量

  1. 创建文件

     ~~~shell
     vim /etc/profile.d/go.sh
     #在打开文件里写入
     export PATH=$PATH:/usr/local/go/bin
     ~~~

  2. 使刚刚创建文件生效

     ```shell
     source /etc/profile.d/go.sh
     ```


-  设置 GPOPATH 目录（可以不设置）

  >GOPATH是早期的设置方式，

  GOPATH这个环境变量它指定了一个目录, 这个目录包含了我们所有的源码 ，是工作目录

  我们写的代码可以放到这个目录下面。

  1. 创建工作目录

     ```shell
     mkdir -p ~/home/user/go
     ```

  2. 将这个目录添加到GOPATH中，跟上面一样需要先创建一个文件。

     ~~~shell
     vim /etc/profile.d/gopath.sh
     ~~~


     之后在在文件里面输入GOPATH具体指向位置，然后保存退出。

  3. 使刚刚创建文件生效

     ```shell
     source /etc/profile.d/gopath.sh
     ```

  4. 验证GOPATH环境变量是否添加成功

     ```shell
     echo $GOPATH
     ```

- 配置go mod

  1. 开启go mod

     ~~~shell
     go env -w GO111MODULE=on
     ~~~

  2. 切换为国内源

     ~~~shell
     $ export GO111MODULE=on
     $ export GOPROXY=https://goproxy.cn
     ~~~

  3. 进入到myproject 执行 

     ~~~shell
     go mod init myproject
     ~~~

  4. 就会创建出一个go.mod文件。在该目录下执行

     ~~~shell
     go mod install myproject
     ~~~

     即可生成bin文件。

#### windos安装go环境

- 下载安装包-->[传送门](https://golang.google.cn/dl/)

  <img src="C:\Users\12980\Pictures\typora图片\image-20210909105930863.png" alt="image-20210909105930863" style="zoom:80%;" />

- 完成之后开始安装，一路next直到完成，需要注意的就是安装路径，默认是C盘

  ①我的电脑->鼠标右键->属性->高级系统设置->环境变量

  ②显示用户变量，第一个地方只要出现GOPATH就行，我的截图是之后新建的文件夹，更改之后的。(可设可不设)

  <img src="C:\Users\12980\Pictures\typora图片\image-20210909110428396.png" alt="image-20210909110428396" style="zoom:80%;" />

  ③系统变量，还是一样选中“Path”点击“编辑”按钮就行查看，最下方又出现安装目录的地址。

  <img src="C:\Users\12980\Pictures\typora图片\image-20210909110727385.png" alt="image-20210909110727385" style="zoom:67%;" />

  ④手动新建环境变量。

  - 第一个是系统变量，新建一个GOROOT（Go语言安装目录显示“新建系统变量”，变量值就浏览目录选择安装的目录。
  - 下一个GOPATH，同用户环境变量

  ![image-20210909110904915](C:\Users\12980\Pictures\typora图片\image-20210909110904915.png)

  ​	

- Go切换国内源 

  ~~~shell
  $ go env -w GO111MODULE=on
  $ go env -w GOPROXY=https://goproxy.cn,direct
  ~~~






#### docker部署 

- 部署docker 

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

- 修改Dockerfile文件

  >所需要的依赖包calibre下载过于缓慢，建议下载到本地再从本地拉入

  注意所添加依赖尽量放在Dockerfile同级目录下，修改对应位置内容为以下(大致在第100行)

  ~~~shell
  COPY calibre-5.26.0-x86_64.txz  /tmp
  RUN mkdir -p /opt/calibre
  RUN tar xJof /tmp/calibre-5.26.0-x86_64.txz -C /opt/calibre
  ~~~

- 进行build 

  >①这里仓库为test，镜像名为mindoc。建议为自己的镜像设置合适的名字方便以后追踪和管理
  >
  >②上面命令中最后的“.”告诉Docker到当前目录中去找Dockerfile文件，也可以采用git仓等方法

  ~~~
  docker build  -t="f" .
  ~~~

  此时 运行 `docker images` ，可以列出我们刚刚生成的镜像。

- 容器运行

  ~~~
  docker run -p 8181:8181 --name mindoc   -e httpport=8181 -d 镜像ID/名字 
  ~~~

  ~~~
  docker run -p 8002:8002 --name ferry   -e httpport=8002 -d  ferry
  ~~~
  
  

  
  
  如果遇到问题，可以尝试
  
  ~~~
  docker logs 容器ID/名字
  ~~~
  
  来查看详细信息

over~



~~~
docker build  -t="test/ferry" .
~~~

