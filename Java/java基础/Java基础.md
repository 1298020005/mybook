# Java基础

>https://github.com/ayhyh/java-se-bixiangdong   毕向东 源码

#### HashMap

>https://www.cnblogs.com/chengxiao/p/6059914.html  HashMap实现原理及源码分析  
>
>https://zhuanlan.zhihu.com/p/127147909  HashMap原理详解，看不懂算我输

其他 

>https://cloud.tencent.com/developer/article/1421770  Java8 hashmap
>
>https://zhuanlan.zhihu.com/p/127147909     HashMap原理详解，看不懂算我输 
>
>https://www.cnblogs.com/chengxiao/p/6059914.html  hash实现原理及源码分析
>
>https://www.cnblogs.com/ludangxin/p/15526375.html java基础hashmap
>
>https://zhuanlan.zhihu.com/p/21673805 Java 8系列之重新认识HashMap(美团的)   
>
>https://www.jianshu.com/p/003256ce41ce 数据结构解析-HashMap

#### Java中的<T>（尖括号）

**问题**


我目前正在学习Java，最近被尖括号(<>)困扰了。他们究竟是什么意思？

```java
public class Pool<T>{
    public interface PoolFactory<T>{
        public T createObject();
    }
    this.freeObjects= new ArrayList<T>(maxsize)
}
```

`<T>`是ageneric，通常可以读作"T型"。它取决于<>左边的类型实际意味着什么。

我不知道a`Pool`或`PoolFactory`是什么，但你也提到`ArrayList<T>`，这是一个标准的Java类，所以我会谈到它。

通常，你不会看到"T"，你会看到另一种类型。因此，如果你看到例如`ArrayList<Integer>`，那就意味着"**An`ArrayList`of`Integer`s**"。例如，**许多类使用泛型来约束容器中元素的类型**。另一个例子是`HashMap<String, Integer>`**，意思是"带有`String`keys和`Integer`值的 Map "**。

你的游泳池示例有点不同，因为你正在定义一个类。因此，在这种情况下，你正在创建一个其他人可以用特定类型代替T进行实例化的类。例如，我可以使用你的类定义创建类型为`Pool<String>`的对象。这意味着两件事：

- 我的池<String>将有一个接口PoolFactory <String>，其中包含一个返回字符串的createObject方法。
- 在内部，Pool <String>将包含字符串的ArrayList。

这是一个好消息，因为在另一个时间，我可以来创建一个使用相同代码的a`Pool<Integer>`，但无论你在源头看到5334114035，都有`Integer`。

#### HashMap 和 Map（类比List 和ArrayList）

>待补充增加  Iterator array  
>
>简洁但是代码可以多看 http://c.biancheng.net/view/1067.html
>
>虽然是 c++ 写的很好 https://www.cnblogs.com/inception6-lxc/p/9263964.html

代码如下

~~~java
//1 正确写法
HashMap<String, String> parameters = new HashMap<>();
//2 可以写的写法 
Map<String, String> parameters = new HashMap<>();
//3 错误写法  错误 Map is abstract,cannot be instantiated
Map<String, String> parameters = new Map<>();
~~~

问题

在HashMap 上接口的实例化。

注意在此处2 接口的定义可以 而3 在实例化却不行。


- Map集合的特点：
  1、Map集合一次存储两个对象，一个键对象，一个值对象
  2、键对象在集合中是唯一的，可以通过键来查找值

- HashMap特点：
  1、使用[哈希算法](https://www.baidu.com/s?wd=哈希算法&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)对键去重复，效率高，但无序
  2、HashMap是[Map接口](https://www.baidu.com/s?wd=Map接口&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)的主要实现类

#### NameValuePair的用法

>https://www.cnblogs.com/liuminchao/p/12856214.html
>
>完整的方法传输

~~~
String url="http://www.baidu.com";

HttpPost httppost=new HttpPost(url); //建立HttpPost对象

List<NameValuePair> params=new ArrayList<NameValuePair>();
//建立一个NameValuePair数组，用于存储欲传送的参数

params.add(new BasicNameValuePair("pwd","2544"));
//添加参数

httppost.setEntity(new UrlEncodedFormEntity(params,HTTP.UTF_8));
//设置编码

HttpResponse response=new DefaultHttpClient().execute(httppost);
//发送Post,并返回一个HttpResponse对象
~~~



#### add 和 put 

add（）和put（）方法都是集合框架中的添加元素的方法。
但是put（）方法应用于map集合中，add（）方法应用于collection集合中。
二者的主要区别是：返回值类型不一样。
add（）放回布尔（boolean）类型。因为像Set集合中不允许添加重复的元素。当HashSet调用add（）方法时，如果返回false，表示添加不成功。
put（）的使用是：添加时出现相同的键，那么后添加的值会替换（覆盖）掉此键对应的原来的值。并返回此键对应的原来的值。

#### 泛型

例子

~~~java
public class Pool<T>{
    public interface PoolFactory<T>{
        public T createObject();
    }
    this.freeObjects= new ArrayList<T>(maxsize)
}
~~~

##### 尖括号 < T > 

它与java中的泛型有关。如果我提到`ArrayList<String>`这意味着我只能将String类型对象添加到该ArrayList。

**Java中泛型的两个主要好处是：**

1. 减少程序中的强制转换数量，从而减少程序中潜在错误的数量。
2. 提高代码清晰度

`<T>`是*通用的*，通常可以读作“T型”。它取决于<>左边的类型实际意味着什么。

我不知道是什么，`Pool`或者`PoolFactory`你也提到`ArrayList<T>`，这是一个标准的Java类，所以我会谈到它。

通常，你不会看到“T”，你会看到另一种类型。因此，如果您看到`ArrayList<Integer>`，例如，这意味着“An `ArrayList`of `Integer`s。” 例如，许多类使用泛型来约束容器中元素的类型。另一个例子是`HashMap<String, Integer>`“带有`String`键和`Integer`值的地图”。

您的Pool示例有点不同，因为您正在*定义*一个类。因此，在这种情况下，您正在创建一个其他人可以用特定类型代替T进行实例化的类。例如，我可以`Pool<String>`使用您的类定义创建一个类型的对象。这意味着两件事：

- 我`Pool<String>`将有一个返回s `PoolFactory<String>`的`createObject`方法的接口`String`。
- 在内部，`Pool<String>`它将包含一个`ArrayList`字符串。

这是一个好消息，因为在另一个时间，我可以来创建一个`Pool<Integer>`使用相同代码的东西，但`Integer`无论你`T`在源头看到的地方都有。

#### i++ 和++i

> https://blog.csdn.net/android_cai_niao/article/details/106027313

#### 枚举 

>小册子  枚举的思想层面(为什么有)
>
>https://cloud.tencent.com/developer/article/1595786?from=article.detail.1756253 解决为什么枚举对象只能在枚举类定义的时候进行实例化
>
>https://cloud.tencent.com/developer/article/1745734 枚举的应用 属于多例设计模式
>
>https://blog.csdn.net/weixin_41249041/article/details/96764460 使用枚举类型作为请求参数 
>
>https://developer.aliyun.com/article/313093?spm=a2c6h.14164896.0.0.89887995Sw3GEB 枚举的简单应用
>
>https://www.cnblogs.com/coderzjz/p/13587167.html  必要看的  Java集合 Collection、Set、Map、泛型 简要笔记



####  浅拷贝和深拷贝 BeanUtils.copyProperties 工具类的使用 (实际项目中其)

>https://www.cnblogs.com/aduner/p/14708036.html  这篇博客关于 引用 的代码非常好理解 
>
>如果这些对象都是**基础对象**当然没什么问题，但是如果对象进行操作，相当于**两个对象同属一个实例**。 
>
>对象跟实例的概念
>
>https://blog.csdn.net/qgnczmnmn/article/details/109384632 

#### 泛型类型

>小册子 

变量是对数据的抽取，而泛型是对变量类型的抽取，抽取成类型参数，抽象层次更高

把变量类型抽取成类型参数T构造出模板代码，再通过继承泛型类等方式确定T（实际类型参数赋值），把类型特定化，最后配合编译器在编译期对相关操作的变量类型进行约束，这就是泛型。

抽取变量，我们早就习以为常，但抽取变量类型，却从未听说。这也是初学者觉得泛型抽象的根本原因。

最后，强调一下，**泛型是对引用类型的抽取，基本类型是无法抽取的**
