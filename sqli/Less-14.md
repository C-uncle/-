# Less-14

## 0x00 注入

![image-20210114164803259](D:\WEB安全\笔记\image\image-20210114164803259.png)

![image-20210114164731156](D:\WEB安全\笔记\image\image-20210114164731156.png)

确认参数类型

和less-13一样。



## 0x01 分析

$uname='"'.$uname.'"'; // 在参数两边加上了双引号
$passwd='"'.$passwd.'"';
@$sql="SELECT username, password FROM users WHERE username=$uname and password=$passwd LIMIT 0,1";
$result=mysql_query($sql);
$row = mysql_fetch_array($result);

