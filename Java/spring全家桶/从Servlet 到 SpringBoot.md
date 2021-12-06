2021-7-21

spring   springmvc  bean  

tomcat web 容器  servlet  

https://www.zhihu.com/people/jessenqiang/posts

https://blog.csdn.net/qfikh

https://www.cnblogs.com/jieerma666/p/10805966.html

 什么是ssm框架？  https://www.zhihu.com/question/328810338/answer/720393487 该作者比喻及其生动形象，建议多看。

https://blog.csdn.net/qq_36972826/article/details/118936530?spm=1001.2014.3001.5501

https://blog.csdn.net/qq_36972826/article/details/118894333

https://blog.csdn.net/qq_42046105/article/details/83793787

https://blog.csdn.net/mu_wind/article/details/113431657

https://blog.csdn.net/weixin_40753536/article/details/81285046



servlet tomcat 

>https://zhuanlan.zhihu.com/p/40249834#:~:text=Tomcat%E6%98%AF%E4%B8%80,rvlet%E5%AE%B9%E5%99%A8%E3%80%82
>
>https://zhuanlan.zhihu.com/p/34147010
>
>https://zhuanlan.zhihu.com/p/112659168
>
>https://zhuanlan.zhihu.com/p/33564233
>
>https://www.cnblogs.com/jiangzhaowei/p/9372884.html



**重要**

>https://blog.csdn.net/weixin_45393094/article/details/104714798



### 从Servlet 到 SpringBoot

springboot 参数必须要 RequestParam和 PathVariable吗

关于 servelt-tomcat   bean-spring(boot，mvc)

#### 重要概念

1. tomcat

   Tomcat可以实例化Serlvet并调用执行Servlet来处理业务逻辑。并且它也具有HTTP服务器的功能。所以，**Tomcat就是一个 HTTP服务器 + Servlet容器**。Tomcat也叫Web容器

   容器，Tomcat是一个Web服务器（同时也是Servlet容器），通过它我们可以很方便地**接收和返回**到请求（如果不用Tomcat，那我们需要自己写Socket来接收和返回请求）。

   Tomcat是Servlet/JSP（= in-out servlet)的容器, 那么Servlet是什么？Servlet实质上是用Java语言对HTTP协议的封装，而Tomcat是HTTP服务器的一种实现。

3. http(协议及其服务器)

4. servlet

5. spring mvc 

5. spring boot 

6. 容器的概念



####  @RequestParam详解以及加与不加的区别

1. 加入value 为必须传入，不加则为可传可不传入。
2.  在接受的时候影响不大，但是你像server传入是(类比rpc)，会因为识别不到参数而报错。
3. 在异常处理判断时，不加的话也不会报错，加入的话由value可以进行异常处理的判断。



#### Servlet的前世今生

##### CGU----当时生成Web动态内容的主流技术

CGI的问题是每一个WEB请求都需要重新启动一个进程来处理。创建进程需要消耗不少CPU周期，导致难以编写刻苦鏖战的CGI程序。

#### Servlet

Servlet在创建后（处理第一个请求时）就一直保存在内存中,这就比CGI有着更好的性能。

Servlet是一个Java程序，一个servlet应用有一个或多个Servlet程序。JSP页面会被转换和编译成servlet程序。

Servlet程序无法独立运行，必须运行在Servlet容器中。Servlet容器将用户的请求床底给servlet应用，并将结果返回给用户。由于大部分Servlet用用都包含多个JSP页面，因此更准确地说是“Servlet/JSP应用”。

Servlet/JSP容器是一个可以同时处理Servlet和静态内容的Web容器。过去，由于通常认为HTTP服务器比Servlet/JSP 容器更加可靠，因此人们习惯将servlet容器当做HTTP服务器的一个模块，这种模式下，HTTP服务器用来处理静态资源，Servlet容器则负责生成动态内容。

#### Tomcat

![image-20210728140109740](C:\Users\12980\Pictures\typora图片\image-20210728140109740.png)

我们可以看到Tomcat的核心就是Servlet/Jsp容器

Tomcat的执行过程一般可以认为是：

*— 获取连接 — Servlet来分析请求（HttpServletRequest）— 调用其service方法，进行业务处理 — 产生相应的响应（HttpServletResponse）— 关闭连接*

