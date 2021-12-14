#### 验证token过期后beego是否能取到

js代码运行前

![image-20210916175826083](C:\Users\12980\Pictures\typora图片\image-20210916175826083.png)



js运行

![image-20210916175926407](C:\Users\12980\Pictures\typora图片\image-20210916175926407.png)



20s 后刷新

![image-20210916175952844](C:\Users\12980\Pictures\typora图片\image-20210916175952844.png)

go 代码

~~~go
func (this *GetCookieController)Get()  {
	token:=this.Ctx.GetCookie("token")
	this.Ctx.WriteString("token="+token)
}
~~~

