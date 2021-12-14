#### with recursive递归

>https://blog.csdn.net/lv_xiaohu/article/details/90691634  SQL根据指定节点ID获取所有父级节点和子级节点

~~~sql
# 注意
WITH TEMP AS
# 改成 
WITH recursive temp AS
~~~

>https://www.cnblogs.com/shenjiangwei/p/14068045.html



#### Like 和 where

LIKE允许部分匹配/使用通配符，而=检查是否完全匹配。

例如

~~~sql
SELECT * FROM test WHERE FIELD LIKE '%oom';
~~~

将返回其中字段值为以下任意值的行：

~~~sql
Zoom, Boom, Loom, Groom
~~~

