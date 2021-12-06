### mybatis 

>https://blog.csdn.net/qq_40298902/article/details/106243726
>
>https://www.w3cschool.cn/mybatis/f4uw1ilx.html
>
>https://www.jianshu.com/p/ceb1df475021 
>
>https://blog.csdn.net/qq_39766167/article/details/105726064

Spring Boot 会自动加载 spring.datasource.* 相关配置，数据源就会自动注入到 sqlSessionFactory 中，sqlSessionFactory 会自动注入到 Mapper 中，对了，你一切都不用管了，直接拿起来使用就行了。



#### ORM  Mapper

>http://www.mybatis.cn/archives/862.html 重要
>
>https://blog.csdn.net/weixin_39580715/article/details/110527615
>
>https://blog.csdn.net/z828849eser/article/details/87561407



#### MyBatis-Plus 新增插入成功并获取自增Id

>https://blog.csdn.net/qq_18108159/article/details/106573593 

#### Mybatis中resultMap使用

>https://www.jianshu.com/p/df86cb39d672        跟下边是一个人   
>
>https://www.jianshu.com/u/a26fc7859dff          nice！ 看此人相关文章就足够了。
>
>https://www.jianshu.com/p/281eb3d9feea   	数据库xml配置信息 

实际上是resultType将查询到结果映射封装成pojo类型中，前提是该pojo类的**属性名**和查询到的数据库表的**字段名一致**。这种映射封装mybatis帮我们自动做好了，不需要我们自己考虑。所以看起来我们是我们查询一些数据，然后返回类型是pojo。实际上内部映射封装不需要我们实现。

很适合单表查询。



#### mybatis-plus 代码生成器

>https://www.cnblogs.com/chenyanbin/p/13702283.html
>
>https://blog.csdn.net/liu649983697/article/details/113930493

#### 教育平台期间带整理的文档 

##### Mybatis-plus

https://blog.csdn.net/weixin_43691058/article/details/115368094     查询数据库中所有记录

https://blog.csdn.net/FurtherSkyQ/article/details/109381257     Mybatis-plus实现只查询指定列数据以及如何联表查询

https://www.cnblogs.com/pingguo-softwaretesting/p/14204509.html        Mybatis-plus 条件查询  相等 小于 大于 
