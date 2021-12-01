### Springboot 内嵌 tomcat  

#### 一. 查看当前使用的Tomcat版本号

##### 1. 查看本机tomcat 

如果未配置环境变量,需要进入到tomcat\bin目录中,在bin目录下打开命令行输入version

![image-20210727185845453](C:\Users\12980\Pictures\typora图片\image-20210727185845453.png)

##### 2. 查看当前maven 仓库

当我们要查Spring Boot-XXX-RELEASE的内嵌Tomcat版本, 可以打开链接

~~~
https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-tomcat
~~~

如下图，选择指定的Spring Boott版本
![image-20210726153549549](C:\Users\12980\Pictures\typora图片\image-20210726153549549.png)

进入所选链接后， 红框标记的就是tomcat的版本。

![image-20210726153746886](C:\Users\12980\Pictures\typora图片\image-20210726153746886.png)

![image-20210726153843981](C:\Users\12980\Pictures\typora图片\image-20210726153843981.png)

##### 3. 通过idea 查看

idea右侧打开

![image-20210727184522793](C:\Users\12980\Pictures\typora图片\image-20210727184522793.png)



#### 二 指定Spring boot项目内嵌的Tomcat版本

##### 1.Springboot升级tomcat

因为SpringBoot内嵌的Tomcat会伴随SpringBoot的升级而升级，所以可以根据需要选择合适的Tomcat版本，这种特别需要升级Tomcat版本时使用，当然还是要根据情况，因为升级SpringBoot的版本也是有成本的。

##### 2. 通过配置文件指定 Tomcat版本

>基于maven

在 pom.xml文件里面添加一个标签`<properties>`，添加期望的版本。

~~~
<tomcat.version>9.0.41</tomcat.version>
~~~

添加jar包

~~~
<dependency> 
   <groupId>org.apache.tomcat</groupId> 
   <artifactId>tomcat-juli</artifactId> 
   <version>${tomcat.version}</version> 
 </dependency>
<dependency>
    <groupId>org.apache.tomcat.embed</groupId>
    <artifactId>tomcat-embed-logging-juli</artifactId>
    <version>${tomcat.version}</version>
</dependency>
~~~

