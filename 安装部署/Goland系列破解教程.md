#### Goland**系列破解教程**

>适用于所有Jetbrains

- 安装goland后，先选择免费试用 
- ide-eval-resetter-2.1.13.zip  
- 通常可以直接把ide-eval-resetter-2.1.13.zip补丁包拖进IDE的窗口来进行插件的安装(如下图)。如果无法拖动安装，你可以在`Settings/Preferences...` -> `Plugins` 里手动安装插件（`Install Plugin From Disk...`）

<img src="C:\Users\12980\Pictures\typora图片\image-20210906185659090.png" alt="image-20210906185659090" style="zoom: 50%;" />

- 插件会提示安装成功。
- 一般来说，在IDE窗口切出去或切回来时（窗口失去/得到焦点）会触发事件，检测是否长时间（`25`天）没有重置，给通知让你选择。（初次安装因为无法获取上次重置时间，会直接给予提示）
- 也可以手动唤出插件的主界面：
  - 如果IDE没有打开项目，在`Welcome`界面点击菜单：`Get Help` -> `Eval Reset `
  - 如果IDE打开了项目，点击菜单：`Help` -> `Eval Reset`

<img src="C:\Users\12980\Pictures\typora图片\image-20210906185759393.png" alt="image-20210906185759393" style="zoom:50%;" />

- 唤出的插件主界面中包含了一些显示信息， 2个按钮，1个勾选项：

    - 按钮：`Reload` 用来刷新界面上的显示信息。
    - 按钮：`Reset` 点击会询问是否重置试用信息并**重启IDE**。选择`Yes`则执行重置操作并**重启IDE生效**，选择`No`则什么也不做。（此为手动重置方式）
    - 勾选项：`Auto reset before per restart` 如果勾选了，则自勾选后**每次重启/退出IDE时会自动重置试用信息**，你无需做额外的事情。（此为自动重置方式）



#### Go 配置

1. [golang下载链接](https://golang.google.cn/dl/)

   

2. 下载慢 换源 

   >$ go env -w GO111MODULE=on
   >$ go env -w GOPROXY=https://goproxy.cn,direct

3. build 失败 

   缺少 编译器 gcc  

   下载符合自己系统版本的压缩包

   >https://sourceforge.net/projects/mingw-w64/files/mingw-w64/

   ![image-20210907101435504](C:\Users\12980\Pictures\typora图片\image-20210907101435504.png)

   直接解压以后 , 把bin目录放入 PATH环境变量就行了

   在这里有个坑，下载exe包后会发现，exe包解压下载需要的资源有时候需要翻墙。

   所以此连接直接下载的是解压后的文件。