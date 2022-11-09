Structured query language,SQL,结构化查询语言，一种特殊的编程语言，用于数据库中的标准数据查询，是管理关系式数据库的标准语言。各种通行的数据库系统在实践中都对SQL规范进行了某些编改和扩充，实际上不同的数据库系统之间的SQL不完全相互通用。

关系式数据库，有明显的层次结构，常见的有：mysql，access，Oracle；基本结构都是：库名-表名-字段名-字段内容

SQL注入，SQL injection是一种常见的web安全漏洞，攻击者利用这个漏洞，可以访问和修改数据，或者利用潜在的数据库漏洞进行攻击

#### 漏洞原理

针对SQL注入的攻击行为可描述为通过用户可控参数中注入SQL语法，破坏原有SQL结构，达到编写程序时意料之外结果的攻击行为，可以归结为两个原因造成：

1. 开发人员在出理程序和数据库交互时，使用字符串拼接的方式构造SQL语句
2. 未对用户可控参数进行足够的过滤便将参数内容拼接到SQL语句中

#### 注入点可能存在的位置

根据SQL注入漏洞的原理，在用户“可控参数”中注入SQL语法，也就是说web应用在获取用户数据的地方，只要带入数据库查询，都有存在SQL注入的可能，通常包括：

1. GET数据
2. POST数据
3. HTTP头部（HTTP请求报文其他字段）refer字段，cookie字段
4. Cookie数据

#### 漏洞危害

攻击者利用SQL注入漏洞，可以获取数据库中的多种信息（如：管理员后台密码），从而脱取数据库中的内容（脱库）。在特别情况下还可以修改数据库内容或者插入内容到数据库，如果数据库的权限分配存在问题，或者数据库本身存在缺陷，那么攻击者可以通过SQL注入漏洞直接获取webshell或者服务器系统权限

##### 	漏洞分类

1. 数字型注入，注入点的数据，拼接到SQL语句中以数字型出现，即数据两边都没有被单引号、双引号包括
2. 字符型和数字型相反，注入的字符在引号内，根据注入手法可以分为下面几类
   1. 可联合查询注入，UNION query SQL injection，可以看到数据库回显
   2. 报错型注入，Error-based SQL injection，可以看到数据库回显
   3. 布尔型注入，Boolean-based blind SQL injection，只能看到页面状态，盲注
   4. 基于时间延迟注入，Time-based blind SQL injection，只能看到页面状态，盲注
   5. 可多语句查询注入，Stacked queries SQL injection，同时存在多条语句，多用于增删改

#### 数据库基础知识

1. mysql注释大概分几种

   1. #
   2. --  （杠杠空格）
   3. /*…… */
   4. /*!…… */ 内联查询

2. mysql元数据库information_schema，这个数据库存储了库名、表明和字段名

    information_schema(存储了所有的库名表明字段名)

3. mysql常用函数和参数

   | 函数与参数                   | 注释                                                         |
   | ---------------------------- | ------------------------------------------------------------ |
   | =、>、>=、<=、<>             | 比较运算符，等于、大于、大小于等于、不等于                   |
   | and、or                      | 逻辑预算符                                                   |
   | version()                    | 数据库版本：select version();                                |
   | database()                   | 当前数据库名称：select database();                           |
   | user()                       | 用户名：select user();                                       |
   | current_user()               | 当前用户名：select current_user();                           |
   | system_user()                | 系统用户名：select system_user();                            |
   | @@datadir                    | 数据库路径：select @@datadir;                                |
   | @@version_compile_os         | 操作系统版本：select @@version_compile_os;                   |
   | length()                     | 返回字符串的长度：select length('12356');                    |
   | substring()、substr()、mid() | 截取字符串，起始位置从1开始计数，截取长度：select substring(database()1,1)，截取database的长度，从1开始，截取1位 |
   | left()                       | 从左侧开始取指定字符个数的字符串：select left("123456",1);返回1，输入2，返回12 |
   | concat()                     | 没有分隔符的连接字符串：select concat('a','b','c');          |
   | concat_ws()                  | 含有分隔符的连接字符串：select conca_ws("-",'a','b','c');返回a-b-c |
   | group_concat()               | 连接一组字符串，将不同行的内容放入同一行：select group_concat(id) from cms_article; |
   | ord()                        | 返回ASCII码：select ord('2');返回ASCII的对应码：97           |
   | rand()                       | 返回0-1的随机数：select rand();                              |
   | load_file()                  | 读取文件，并返回文件内容作为一个字符串                       |
   | sleep()                      | 失眠时间指定的秒数：select sleep(4);                         |
   | if(true,t,f)                 | if判断，如果为true执行t，如果为false执行f：select if (true,1,2);返回1；select if (false,1,2);返回2 |

4. SQL语句中逻辑运算与（and）比或（or）的优先级高

#### 注入流程

关系型数据库具有明显的库-表-列-内容结构层次，所以通过SQL注入漏洞获取数据库信息的时候，也要依据这样的顺序

首先获取数据库名，其次获取表名，然后获取列名，最后获取数据