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



#### 从一个表中获取两次数据 （其关联字段在此表中）

>https://leetcode-cn.com/problems/employees-earning-more-than-their-managers/      

~~~sql
SELECT *
FROM Employee AS a, Employee AS b;
~~~



#### 重复数据

##### 查询一张表中的重复数据

>https://leetcode-cn.com/problems/duplicate-emails/solution/cha-zhao-zhong-fu-de-dian-zi-you-xiang-by-leetcode/

1. 使用临时表和group by 
2. 使用group by 和having in

##### 删除一张表的重复数据 

>https://leetcode-cn.com/problems/delete-duplicate-emails/solution/shan-chu-zhong-fu-de-dian-zi-you-xiang-by-leetcode/

1. 注意group by 似乎不行 

#### 一张表在另一张不存在的数据 

>https://leetcode-cn.com/problems/customers-who-never-order/

1. 使用两层 select  子查询 和 not in 

   ~~~sql
   select customers.name as 'Customers'
   from customers
   where customers.id not in
   (
       select customerid from orders
   );
   ~~~

2. 左连接 

   ~~~
   select c.Name as Customers 
   from Customers as c
   left join Orders as o on c.Id = o.CustomerId
   where o.Id is null
   ~~~




#### 时间类

例如相差一天

时间类不可以直接用 a - b =1

应该用DATEDIFF(a,b)=1
