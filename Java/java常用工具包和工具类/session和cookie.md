

#### Session 和 cookie

https://www.cnblogs.com/xdp-gacl/p/3855702.html

##### springboot session

>https://blog.csdn.net/ljk126wy/article/details/93971421 SpringBoot 2 整合 Spring Session 最简操作

WebServer，http，websocket(待解决)

有关session序列化待，钝化补充(待解决) 

cookie的响应地址(待解决) 

token问题的解决



>https://www.cnblogs.com/xichji/p/11793439.html cookie
>
>https://blog.csdn.net/yang731227/article/details/82263125 beego cookie 和 session
>
>https://cloud.tencent.com/developer/article/1780392 token



>https://blog.csdn.net/qq_15096707/article/details/74012116 session 怎么实现的 以及怎么存储

### Cookie 和Session

>因为绝大部分接口都是基于“请求-响应”这种通信模式的，即服务器不会主动搭理客户端，只是被动地响应客户端的请求。
>
>所以我们需要采用会话跟踪技术来①标识用户②标识用户状态

#### 会话机制

>会话机制最主要的目的是**帮助服务器记住客户端状态**（标识用户，跟踪状态）。

- 简短定义

  从打开一个浏览器访问某个站点，到关闭这个浏览器的整个过程，称为一次会话。

  而目前，客户端与服务器的通讯都是通过HTTP协议。

- Http协议

  HTTP是一种无状态协议。无状态指的是服务端的每一个请求在客户端看来都是各自独立的

  体现在 ①客户端第二次来访时，服务器不知道这个客户端以前是否来访过②Web服务器本身不能识别出哪些请求是同一个浏览器发出的。原因就是浏览器的每一次请求都是完全孤立的。

  <img src="C:\Users\12980\Pictures\typora图片\image-20210917111939484-1638783014415.png" alt="image-20210917111939484" style="zoom:67%;" />

  这样一来会产生很多问题，比如我无法记住与区分客户端，可能会导致同一个不同客户端的操作归在一类中。

  举例来说，双十一你的购物车点点点了三件，结果你一刷新页面突然多了一堆，因为服务器无法区分客户端用户，可能把小只的购物内容给了小宋。

  此外，假设我们注册了一个论坛，因为HTTP是无状态的，服务器无法辨认访问者，那么即使是同一个网站的不同页面，服务器都必须强制访问者重新登录一次，以确定你是合法用户

- 定义会话

  >会话是为了唯一标识一个用户并记录其状态

  **一个会话可以帮助服务器断定一个客户端**，那么反向推导得到的结论就是：**当服务器无法断定客户端时，一次会话就结束了**。

  那么服务器何时断定客户端即①服务器挂了(session失效)②客户端挂了(cookie失效)

  进一步得出会话存在的原则就是cookie和session同在

#### Cookie

>Cookie是一份小数据，是服务器响应给客户端，并且**存储在客户端**的一份小数据。**下次客户端访问服务器时，会自动带上这个Cookie。**服务器通过Cookie就可以区分客户端。

##### java 代码

服务器new 一个cookie，通过response的addCookie方法响应给浏览器

~~~java
    @RequestMapping(value = "/setCookies",method = RequestMethod.GET)
    public  String setCookies(HttpServletResponse response){
        //HttpServerletRequest 装请求信息类
        //HttpServerletRespionse 装相应信息的类
        // 创建一个 cookie对象
        Cookie cookie=new Cookie("sessionId","CookieTestInfo");
        //设置存在时间
        cookie.setMaxAge(10);
        //将cookie对象加入response响应
        response.addCookie(cookie);
        return "添加cookies信息成功";
    }
~~~

浏览器接收到这些响应头后，会把它们作为Cookie文件存在客户端。当我们第二次请求**同一个**服务器，浏览器在发送HTTP请求时，会带上这些Cookie。

<img src="C:\Users\12980\Pictures\typora图片\image-20210917123659811-1638783014414.png" alt="image-20210917123659811" style="zoom: 50%;" />



流程如下

<img src="C:\Users\12980\Pictures\typora图片\image-20210917140301933-1638783014415.png" alt="image-20210917140301933" style="zoom:67%;" />

服务端获得客户端cookie

~~~java
 @RequestMapping(value = "/getCookies",method = RequestMethod.GET)
    public  String getCookies(HttpServletRequest request){
        //   获得客户端携带的cookie数据
        Cookie[] cookies =  request.getCookies();
        //通过cookie名称获得想要的cookie
        if(cookies != null){
            for(Cookie cookie : cookies){
                if(cookie.getName().equals("sessionId")){
                    return cookie.getValue();
                }
            }
        }
        return  null;
    }
~~~

完整流程如下

<img src="C:\Users\12980\Pictures\typora图片\image-20210917141328574-1638783014415.png" alt="image-20210917141328574" style="zoom:50%;" />

注意当你不设置MaxAge，默认响应会话Cookie，**会话Cookie被保存在浏览器的内存中**，当浏览器关闭时，内存被释放，内存中的Cookie也就随之消失。

![image-20210917141548120](C:\Users\12980\Pictures\typora图片\image-20210917141548120-1638783014415.png)

此处我设置的cookie时间为10秒。



##### go cookie代码

Get

~~~go
func (this *GetCookieController)Get()  {
	token:=this.Ctx.GetCookie("token")
	this.Ctx.WriteString("token="+token)
}
~~~

Set

~~~go
func (this *CookieController)Set()  {
	if this.Ctx.GetCookie("user") ==""{
		this.Ctx.SetCookie("user","admin",20)
		this.Ctx.WriteString("Cookie设置成功")
	}else{
		this.Ctx.WriteString("没有cookie")
	}
}
~~~



#### Session

>存储在服务端，本质类似一个大Map，底层依赖于cookie。

- 需要session的原因

  ①出于安全性和传输效率的考虑。Cookie是存在客户端的，如果保存了敏感信息，会被其他用户看到。

  ②如果信息太多，可能影响传输效率。但这些都是我由果推因得到的

- 我们不再把"name=yyf，time=9"这样的数据作为Cookie放在请求头/响应头里传来传去了，而是只给客户端传一个JSESSIONID（其实也是一个Cookie）此时，真正的数据存在服务器端的Session中。


##### session java 代码

~~~java
    @RequestMapping(value = "/setSessions",method = RequestMethod.GET)
    public void getSessions(HttpServletResponse response, HttpServletRequest request){
        //HttpServerletRequest 装请求信息类
        //HttpServerletRespionse 装相应信息的类
        // 创建一个 session对象\
        HttpSession session=request.getSession();
        session.setAttribute("zhi", "yyds");
        session.setAttribute("song", "zn");
        String sessionId=session.getId();
        //当你不写如下代码，服务器默认将sessionId作为cookie返回
//        Cookie cookie=new Cookie("sessionId",sessionId);
//        cookie.setMaxAge(10);
//        //将cookie对象加入response响应
//        response.addCookie(cookie);
//        return "添加cookies信息成功";
    }
~~~

  下次访问该网站时，把JSESSIONID带上，即可在服务器端找到对应的Session，也相当于“带去”了用户信息。

  <img src="C:\Users\12980\Pictures\typora图片\image-20210917141932057-1638783014415.png" alt="image-20210917141932057" style="zoom:67%;" />

  同样的，既然返回的JSESSIONID也是一个Cookie，那么也分为会话Cookie和持久性Cookie，可以通过设置MaxAge更改

  

  另外要注意的是，Session有个默认最大不活动时间：30分钟（可在配置文件中修改数值）。也就是说，创建Session并返回JSESSIONID给客户端后，如果30分钟内你没有再次访问，即使你下次再带着JSESSIONID来，服务端也找不到对应ID的Session了，因为它已经被销毁。此时你必须重新登录。

  这里要注意！只要你在服务器端创建了Session，即使不写addCookie("JSESSIONID", id)，JSESSIONID仍会被作为Cookie返回。结果服务器默认new一个Cookie，将刚才创建的Session的JSESSIONID返回。默认是会话Cookie。也就是说，服务端返回给客户端的默认是一个Cookie。

##### go session代码

~~~go
func (this *SessionController) Get() {
	if this.GetSession("user") == nil {
		this.SetSession("user", "admin")
		this.Ctx.WriteString("Session设置成功!")
	}else {
		//获取session
		username := this.GetSession("user")
		this.CruSession=this.StartSession()
		fmt.Println("sessionid = ", this.CruSession.SessionID(nil))
		this.Ctx.WriteString("user = " + username.(string))
	}
}
~~~

##### session的存储

存储的方式有 ①本地缓存(redis)②数据库③文件④缓存服务器

  1.本地机器或者本地缓存。优点：速度快 缺点：服务宕机后重启用户信息丢失，用户不优好

  2.数据库。优点：技术栈简单 缺点：速度慢

  3.文件。优点：技术栈简单，速度适中 缺点：无灾备或者灾备方案成本高

  4.缓存服务器。一般是内存服务器，优点：速度快 可以和原有技术栈契合，有现成的解决方案。缺点：不明显

###### spring boot 整合 session 

redis

>https://blog.csdn.net/ljk126wy/article/details/93971421  redis 存储



jdbc 

>https://www.jdon.com/54392
>
>https://jingyan.baidu.com/article/495ba841aaf2ee38b30ede05.html
>
>https://developer.aliyun.com/article/652994



### 跨域问题

>https://segmentfault.com/a/1190000017867312
>
>https://zhuanlan.zhihu.com/p/66484450
>
>https://www.cnblogs.com/yanggb/p/10735763.html
>
>https://segmentfault.com/a/1190000015597029
>
>https://cloud.tencent.com/developer/article/1333800
>
>https://www.cnblogs.com/lovesong/p/10269793.html
>
>https://cloud.tencent.com/developer/article/1648860s
>
>https://segmentfault.com/a/1190000019227927

#### 什么是跨域问题

>浏览器不能执行其他网站的脚本，浏览器的同源策略导致的，是浏览器对javaScript施加的安全限制。

- 同源策略

  浏览器在执行脚本的时候，都会检查这个脚本属于哪个页面，即检查是否同源，只有同源的脚本才会被执行；而非同源的脚本在请求数据的时候，浏览器会报一个异常，提示拒绝访问。

  >所谓同源是指**协议、域名、端口号都相同，只要有一个不相同，那么都是非同源。**

  ![image-20210915103631319](C:\Users\12980\Pictures\typora图片\image-20210915103631319-1638783014415.png)	　

  

- 跨域问题例子



​		①、http://www.123.com/index.html 调用 http://www.123.com/welcome.jsp   协议、域名、端口号都相同，同源。

　　②、https://www.123.com/index.html 调用 http://www.123.com/welcome.jsp   协议不同，非同源。

　　③、http://www.123.com:8080/index.html 调用 http://www.123.com:8081/welcome.jsp  端口不同，非同源。

　　④、http://www.123.com/index.html 调用 http://www.456.com/welcome.jsp    域名不同，非同源。

　　⑤、http://localhost:8080/index.html 调用 http://127.0.0.1:8080/welcom.jsp    虽然localhost等同于 127.0.0.1 但是也是非同源的。

- 跨域问题本质

  跨域问题的本质，是浏览器不允许跨域，具体来说是浏览器如果发现一个网址在向另一个网址发送Ajax请求，或者向一个网址相同、端口号不同的地址发送Ajax请求，那么浏览器会禁止这种请求。

  沿用Steam的比喻，有一个家长，规定小孩最多只能玩一款游戏，如果家长发现小孩玩了两款不同的游戏，视为“跨域玩游戏”，会进行禁止。

  然而浏览器和家长，都是很容易糊弄的。我只要让家长以为两款游戏是同一款游戏。我可以告诉家长，我正在玩一款叫做Steam的游戏，我只玩这一款游戏，别的都不玩。家长以为Steam只是一款游戏，于是允许我玩。

  浏览器也是同理，**我只要让浏览器误以为我跨域的两个网址/端口号，并不是两个，而是同一个**，它就不会认为我跨域了。我告诉浏览器，我现在的端口号是Nginx所监听的端口号8080，而我要Ajax请求的端口号也是Nginx所监听的端口号8080，两个网址和端口号是一样的，浏览器认为我两个服务都是同一个服务器，于是允许我发送Ajax请求。

#### 实例演示

![image-20210915142206291](C:\Users\12980\Pictures\typora图片\image-20210915142206291-1638783014415.png)

我们创建了两个 项目，服务端用spring-boot/go穿件restful接口，客户端采用vue.js调用接口，这两个项目的 的端口号设置是不一样的，一个是 8080，一个是8888，所以这两个项目构成了非同源。那么我们从浏览器输入访问部署在客户端 上的服务端项目。无法访问到。



#### 跨域解决方法

>侧重服务端解决方案

##### spring-boot 处理

- 配置过滤器

  ~~~java
  @Configuration
  public class NetWorkConfig {
  
      // 过滤器跨域配置
      @Bean
      public CorsFilter corsFilter() {
          UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
  
          CorsConfiguration config = new CorsConfiguration();
  
          // 允许跨域的头部信息
          config.addAllowedHeader("*");
          // 允许跨域的方法
          config.addAllowedMethod("*");
          // 可访问的外部域
          config.addAllowedOrigin("*");
          // 需要跨域用户凭证（cookie、HTTP认证及客户端SSL证明等）
          //config.setAllowCredentials(true);
          //config.addAllowedOriginPattern("*");
  
          // 跨域路径配置
          source.registerCorsConfiguration("/**", config);
          return new CorsFilter(source);
      }
  }
  ~~~

  



- 实现 WebMvcConfigurer，重写 addCorsMappings 方法

  ~~~java
  @Configuration
  public class WebConfig implements WebMvcConfigurer {
  
      // 拦截器跨域配置
      @Override
      public void addCorsMappings(CorsRegistry registry) {
          // 跨域路径
          CorsRegistration cors = registry.addMapping("/**");
  
          // 可访问的外部域
          cors.allowedOrigins("*");
          // 支持跨域用户凭证
          //cors.allowCredentials(true);
          //cors.allowedOriginPatterns("*");
          // 设置 header 能携带的信息
          cors.allowedHeaders("*");
          // 支持跨域的请求方法
          cors.allowedMethods("GET", "POST", "PUT", "DELETE", "OPTIONS");
          // 设置跨域过期时间，单位为秒
          cors.maxAge(3600);
      }
  }
  ~~~



##### beego 处理

- 过滤

  ~~~go
  package main
  
  import (
  	beego "github.com/beego/beego/v2/server/web"
  	"github.com/beego/beego/v2/server/web/filter/cors"
  )
  
  func init() {
  	//InsertFilter是提供一个过滤函数
  	beego.InsertFilter("*", beego.BeforeRouter, cors.Allow(&cors.Options{
  		// 允许访问所有源
  		AllowAllOrigins: true,
  		// 可选参数"GET", "POST", "PUT", "DELETE", "OPTIONS" (*为所有)
  		AllowMethods: []string{"GET", "POST", "PUT", "DELETE", "OPTIONS"},
  		// 指的是允许的Header的种类
  		AllowHeaders: []string{"Origin", "Authorization", "Access-Control-Allow-Origin", "Access-Control-Allow-Headers", "Content-Type"},
  		// 公开的HTTP标头列表
  		ExposeHeaders: []string{"Content-Length", "Access-Control-Allow-Origin", "Access-Control-Allow-Headers", "Content-Type"},
  		// 如果设置，则允许共享身份验证凭据，例如cookie
  		AllowOrigins: []string{"http://192.*.*.*:*","http://localhost:*","http://127.0.0.1:*"},
  		AllowCredentials: true,
  	}))
  }
  ~~~

  

##### Jsonp

>未尝试

- 原理

  ①在同源策略下，在某个服务器下的页面是无法获取到该服务器以外的数据的，即一般的ajax是不能进行跨域请求的。但 img、iframe 、script等标签是个例外，这些标签可以通过src属性请求到其他服务器上的数据。利用 script标签的开放策略，我们可以实现跨域请求数据，当然这需要服务器端的配合。 Jquery中ajax 的核心是通过 XmlHttpRequest获取非本页内容，而jsonp的核心则是动态添加 <script>标签来调用服务器提供的 js脚本。

  ②当我们正常地请求一个JSON数据的时候，服务端返回的是一串 JSON类型的数据，而我们使用 JSONP模式来请求数据的时候服务端返回的是一段可执行的 JavaScript代码。因为jsonp 跨域的原理就是用的动态加载 script的src ，所以我们只能把参数通过 url的方式传递,所以jsonp的 type类型只能是get 

##### nginx

>暂时未尝试

- 原理

  利用nginx反向代理，将请求分发到部署到相应项目的tomcat服务器，所以不存在跨域问题

  ![image-20210915151147545](C:\Users\12980\Pictures\typora图片\image-20210915151147545-1638783014415.png)

#### 什么是 Restful 接口 



#### beego api版本更新问题

>https://blog.csdn.net/qq_33762440/article/details/117261251?spm=1001.2014.3001.5501
>
>https://blog.csdn.net/qq_22328661/article/details/115057770
>
>https://www.jishuchi.com/read/beego2/12808





