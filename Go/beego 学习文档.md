### beego 学习文档

----

>http://example.beego.me/document/httpserver.html#route
>
>https://cloud.tencent.com/developer/article/1541826
>
>https://cloud.tencent.com/developer/article/1079566
>
>https://zhuanlan.zhihu.com/p/65869449
>
>https://zhuanlan.zhihu.com/p/66508669
>
>https://www.cnblogs.com/tudaogaoyang/p/7940697.html

#### 环境搭建

~~~
CREATE TABLE IF NOT EXISTS User(
        Id INT(4) PRIMARY KEY AUTO_INCREMENT NOT NULL,
         Name  VARCHAR(64),
         Nickname VARCHAR(64),
         Pwd  VARCHAR(64),
         Email  VARCHAR(64),
         Sex  VARCHAR(64),
         Roleid  VARCHAR(64),
        status INT(4),
        Phone VARCHAR(64)
        );
        
            Id              int64    `orm:"auto"`
    Name            string   `orm:"size(100)"`
    Nickname        string   `orm:"size(100)"`
    Pwd             string   `orm:"size(100)"`
    Email           string   `orm:"size(100)"`
    Sex             string   `orm:"size(2)"`
    Roleid          string   `orm:"size(100)"`
    Status          int64   
    Phone           string   `orm:"size(16)"`
~~~



- go环境搭建

  >[软件下载](https://studygolang.com/dl)

- 环境配置

  >注意 GOPATH 在go mod 后被舍弃了(待解决)

  ~~~shell
  设置环境变量
  GOPATH: 项目路径
  GOROOT：go软件安装路径 
  # 终端运行
  # windows 设置环境变量可以从[计算机]-->[系统属性]-->[高级]-->[环境变量]
  换源
  # go env -w GO111MODULE=on
  # go env -w GOPROXY=https://goproxy.cn,direct
  ~~~

- bee 工具 与beego 安装

  ~~~shell
  # 终端运行
  # go get github.com/beego/bee
  # go get github.com/astaxie/beego
  ~~~

- Ide

  >建议goland



#### 第一个Beego项目 

- 创建项目 

  ~~~
   bee new beego_project
  ~~~

- 启动项目

  ~~~
  cd been_project
  bee run 
  ~~~

  注意因为 编译采用go mod 所以项目没有放在 `$GOPATH/src/` 目录下，所以项目运行可能报如下错误

  <img src="C:\Users\12980\Pictures\typora图片\image-20210907150819904.png" alt="image-20210907150819904" style="zoom:67%;" />

  此时需要运行(待解决 go get mod 之间的关系 以及go get 是什么)

  ~~~shell
  go get beego_project 
  ~~~

   访问项目

  ![image-20210907155327124](C:\Users\12980\Pictures\typora图片\image-20210907155327124.png)

  ​	

#### 项目解析

- 使用goland打开项目

  ![image-20210907155554176](C:\Users\12980\Pictures\typora图片\image-20210907155554176.png)

  ```javascript
  main.go         入口文件 
  conf            配置文件路径
  controllers     业务逻辑
  models          数据库模型
  routers         路由
  static          静态文件
  test            单元测试
  views           前端代码
  ```

  >从上面的目录结构我们可以看出来 M（models 目录）、V（views 目录）和 C（controllers 目录）的结构， main.go 是入口文件。

- 项目运行逻辑介绍

<img src="C:\Users\12980\Pictures\typora图片\image-20210907155646238.png" alt="image-20210907155646238" style="zoom:67%;" />







