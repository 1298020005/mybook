# Redis

>https://cloud.tencent.com/developer/article/1686990	redis学习心得
>
>https://cloud.tencent.com/developer/article/1745094	springboot 整合redis
>
>https://blog.csdn.net/weixin_46091684/article/details/104011970  Redis集成和ES的认识
>
>https://cloud.tencent.com/developer/article/1594109  StringRedisTemplate 和RedisTemlate有什么不同
>
>https://blog.csdn.net/lydms/article/details/105512006 同上
>
>https://zhuanlan.zhihu.com/p/47692277  Redis 常用操作命令，非常详细！
>
>https://blog.csdn.net/zhuimeng_by/article/details/90515456     Linux上Redis的启动、使用和停止
>
>https://blog.csdn.net/qq_17132757/article/details/114234315    Springboot 启动异常Unable to read meta-data for class 待解决 
>
>http://www.coozhi.com/youxishuma/g4/89226.html  JedisConnectionFactory如何获取Redis
>
>https://blog.csdn.net/lydms/article/details/105224210 RedisTemplate操作redis api

#### Redis常用命令 

##### Linux 

启动

~~~
cd src
./redis-server
~~~

停止

~~~
~~~



#### SpringBoot 整合redis

##### 导入依赖

在我们创建 Spring Boot 项目时，选择相关的 Starter (这是个啥)时，**在 “NoSQL” 中把 “Spring Data Redis” 项勾选**。

这样就会在 pom 文件中添加相关的依赖，依赖如下：

~~~xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
~~~

##### RedisRedisConfigurtion 配置 

在未配置时

<img src="C:\Users\12980\Pictures\typora图片\image-20211202151720838.png" alt="image-20211202151720838" style="zoom:80%;" />

定义一个全局的 Redis 的配置类，以后就无需每次使用都进行序列化器(见代码RedisConfigurtion.java)

![image-20211202154559701](C:\Users\12980\Pictures\typora图片\image-20211202154559701.png)



#### Redis API 

##### 介绍

Spring Boot 提供的 Redis API 分为 **高阶 API** 和 **低阶 API**，**高阶 API 是经过一定封装后的 API**，而**低阶 API 的使用和直接使用 Redis 的命令差不多**。

 **高阶 API 提供了两个类可以供我们使用，分别是 RedisTemplate 和 StringRedisTemplate**。**StringRedisTemplate 继承自 RedisTemplate，StringRedisTemplate 的序列化方式与 RedisTemplate 的序列化的方式不同**。具体**在使用上的差别不是特别明显，但是数据在存储到 Redis 中的时候，因为序列化方式的不同，会有一定的差别**。

 **低阶 API 其实也是通过 RedisTemplate 或 StringRedisTemplate 来进行获取。低阶 API 的方法和 Redis 的命令差不多**。

##### Redis 两个 高阶 API的不同

>https://cloud.tencent.com/developer/article/1594109  StringRedisTemplate 和RedisTemlate有什么不同 *** 待整理
>
>https://blog.csdn.net/lydms/article/details/105512006  两者区别



##### RedisTemplate 

>https://blog.csdn.net/lydms/article/details/105224210 列举了 基础的函数
>
>https://blog.csdn.net/xinhuashudiao/category_8513755.html  详细描述了每一个函数的使用

##### String类型操作

1. 添加缓存

   ~~~java
   //1、通过redisTemplate设置值
   redisTemplate.boundValueOps("StringKey").set("StringValue");
   redisTemplate.boundValueOps("StringKey").set("StringValue",1, TimeUnit.MINUTES);
   
   //2、通过BoundValueOperations设置值
   BoundValueOperations stringKey = redisTemplate.boundValueOps("StringKey");
   stringKey.set("StringVaule");
   stringKey.set("StringValue",1, TimeUnit.MINUTES);
   
   //3、通过ValueOperations设置值
   ValueOperations ops = redisTemplate.opsForValue();
   ops.set("StringKey", "StringVaule");
   ops.set("StringValue","StringVaule",1, TimeUnit.MINUTES);
   ~~~

   boundValueOps类型的操作是获取到一个Opercation,但是没有指定操作的key,可以在一个连接(事务)内操作多个key以及对应的value;opsForValue类型的会获取到一个指定了key的operation,在一个连接内只操作这个key对应的value。

   人话就是前者未明确key-value的对应关系，后者唯一对应了关系。

2. 

##### Redis 低阶API 

序列化

##### SpringDataRedis (教学不涉及，暂且搁置)

>https://blog.csdn.net/lydms/article/details/96733376   SpringDataRedis

#### 代码例子

##### 初始值的赋值

~~~java
    //直接测试，赋值为字符串
    public void testRedisTemplate()
    {
        ValueOperations valueOperations = redisTemplate.opsForValue();
        valueOperations.set("key1", "日照香炉生紫烟");
        valueOperations.set("key2", "老板就是我的天");
        System.out.println(valueOperations.get("key1"));
        System.out.println(valueOperations.get("key2"));
    }
~~~

我们通过 redisTemplate (上文的API介绍里提到)的 opsForValue 得到一个 ValueOperations 的实例，然后通过它来操作 Redis 的字符串类型。使用它的 set 方法添加了 key1 和 key2 两个键值对。然后，通过它的 get 方法又读取了这两个键对应的值。

之后可以将其封装为，redisUtil类

#### Redis为什么有16 个数据库？

>https://zhuanlan.zhihu.com/p/349776401

在同一redis实例中使用redis数据库有一个明显的优势，那就是管理。如果您为每个应用程序启动一个单独的实例，并且假设您有3个应用程序，那么这就是3个独立的redis实例，每个实例在生产中可能都需要一个HA从属实例，因此总共有6个实例。从管理的角度来看，这很快就会变得混乱不堪，因为您需要监视所有这些，进行升级/修补等。如果您不打算通过高I / O重载Redis，那么具有从属的单个实例会更简单，并且只要满足您的SLA，就更易于管理。

Redis的多个实例使您可以利用多个核心，在监视和管理多个实例其实也不困难，可以根据你不同的需求用不同的指标管理不同的数据库。

实际上，通过基于实例的隔离，您将在每个数据库上获得更好的指标。每个实例将具有反映该数据段的统计信息，这可以允许进行更好的调整以及更敏感和更准确的监视。

正如大多数开发者的想法一样，不要轻易使用keys命令。如果仅创建一个键索引，就会发现更好的性能。每当添加密钥时，请将密钥名称添加到集合中。一旦扩大规模，keys命令就不会非常有用，因为返回将花费大量时间。

让访问模式确定如何构造数据，而不是按照您认为的工作方式存储数据，然后解决如何访问数据并将其切碎的问题。您会拥有更好的性能，并且发现使用数据的代码通常更干净，更简单。

#### Redis Desktop Manager

>https://www.w3cschool.cn/redis/redis-evz12p08.html   Redis可视化工具Redis Desktop Manager使用教程
>
>https://www.cnblogs.com/qingmuchuanqi48/p/11966568.html Redis DeskTop Manager 使用教程 

