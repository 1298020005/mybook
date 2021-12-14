#### http协议 

客户端发送请求都需要服务器端回送响应.请求结束后，主动释放链接，**因此为短连接**。通常的做法是，不需要任何数据，也要保持每隔一段时间向服务器发送"保持连接"的请求。这样可以保证客户端在服务器端是"上线"状态。

HTTP连接使用的是**"请求-响应"**方式，不仅在请求时建立连接，而且客户端向服务器端请求后，服务器才返回数据。

#### **Socket **

> socket是对TCP/IP协议的封装，**Socket本身并不是协议，而是一个调用接口（API）**，通过Socket，我们才能使用TCP/IP协议。

- socket连接

  socket连接及时所谓的长连接，理论上客户端和服务端一旦建立连接，则不会主动断掉；但是由于各种环境因素可能会是连接断开，比如说：服务器端或客户端主机down了，网络故障，或者两者之间长时间没有数据传输，网络防火墙可能会断开该链接已释放网络资源。所以当一个socket连接中没有数据的传输，那么为了位置连续的连接需要发送心跳消息，具体心跳消息格式是开发者自己定义的。

1. HTTP的长连接一般就只能坚持一分钟而已，而且是浏览器决定的，你的页面很难控制这个行为。
   Socket连接就可以维持很久，几天、数月都有可能，只要网络不断、程序不结束，而且是可以编程灵活控制的。

2. HTTP连接是建立在Socket连接之上。在实际的网络栈中，Socket连接的确是HTTP连接的一部分。但是从HTTP协议看，它的连接一般是指它本身的那部分。

   ![image-20210915183136580](C:\Users\12980\Pictures\typora图片\image-20210915183136580-1638783152066.png)

   >个人理解:
   >
   >①应用层:http协议理解为货物。客户端用web浏览器发送http请求给服务器,服务器发送被请求信息给客户端。
   >
   >②传输层:tcp/udp协议相当于 卡车
   >
   >③socket:相当于码头Socket是应用层与TCP/IP协议族通信的中间软件抽象层，它是一组接口。socket是在应用层和传输层之间的一个抽象层，它把TCP/IP层复杂的操作抽象为几个简单的接口供应用层调用已实现进程在网络中通信。
   >
   >④网络层协议:相当于高速公路

#### websocket 和 socket 的区别

可以把WebSocket想象成HTTP(应用层)，事实上两者是，同一层，HTTP和Socket什么关系，WebSocket和Socket就是什么关系。

HTTP 协议有一个缺陷：**通信只能由客户端发起，做不到服务器主动向客户端推送信息**。

WebSocket 协议 它的最大特点就是，**服务器可以主动向客户端推送信息，客户端也可以主动向服务器发送信息**，是真正的双向平等对话，属于服务器推送技术的一种



| name      | 关系(依赖)                           |        |
| --------- | ------------------------------------ | ------ |
| socket    | 对TCP/IP协议的封装api                | 全双工 |
| http      | 建立在socket之上，但不全是给予socket | 半双工 |
| websocket | 依赖于http                           | 全双工 |

1. WebSocket依赖于HTTP连接，那么它如何从连接的HTTP协议转化为WebSocket协议？

   每个WebSocket连接都始于一个HTTP请求。具体来说，WebSocket协议在第一次握手连接时，通过HTTP协议在传送WebSocket支持的版本号，协议的字版本号，原始地址，主机地址等等一些列字段给服务器端：

2. WebSocket为什么要依赖于HTTP协议的连接？

   ①WebSocket设计上就是天生为HTTP增强通信（全双工通信等），所以在HTTP协议连接的基础上是很自然的一件事，并因此而能获得HTTP的诸多便利。

   ②基于HTTP连接将获得最大的一个兼容支持，比如即使服务器不支持WebSocket也能建立HTTP通信。





#### Socket 

>　　socket起源于Unix，**而Unix/Linux基本哲学之一就是“一切皆文件”**，都可以用“打开open –> 读写write/read –> 关闭close”模式来操作。

>https://www.jianshu.com/p/066d99da7cbd
>
>https://blog.csdn.net/pashanhu6402/article/details/96428887



#### Socket与Websocket关系 

>https://zhuanlan.zhihu.com/p/32052530

![](C:\Users\12980\Pictures\typora图片\image-20210805114729574-1638781495030.png)

>https://www.cnblogs.com/aspirant/p/11334957.html 
>
>文章链接

https://zhuanlan.zhihu.com/p/95622141  理清 WebSocket 和 HTTP 的关系

