#### 页面注入点判断

1. 联合查询

   数据库中的内容会回显到页面中来，可以使用联合查询进行注入。联合查询就是SQL语法中的union select语句，该语句会同时执行两条select语句，生产两张虚拟表，然后把查询到的结果进行拼接。可以跨库跨表进行查询

   select ~~~ union select ·····

   虚拟表是二维结构，联合查询会“纵向”拼接两张虚拟的表。有两个必要条件

   1. 两张虚拟的表具有相同的列数
   2. 虚拟表对应的列的数据类型相同

2. 报错注入

   在注入点的判断过程中，发现数据库中SQL语句的报错信息，会显示在页面中，因此可以进行报错注入。报错注入的原理，就是在错误信息中执行SQL语句。出发报错的方式 有很多，具体细节不同，最好背公式

   1. group by 重复键冲突

      [?id=33 and (select 1 from (select count(*),concat((select version() from information_schema.tables limit 0,1floor(rand()*2))x from information_schema.tables group by x)a --+]

3. 布尔盲注

4. 延时注入