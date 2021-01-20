# Less-15

## 0x00 注入

盲注

![image-20210114165259032](D:\WEB安全\笔记\image\image-20210114165259032.png)

单引号+ # 绕过

## 0x01 分析

这一关中，代码里注释了输出错误信息，彻底没有错误信息了。真正的盲注。

 $sql="SELECT username, password FROM users WHERE username='$uname' and password='$passwd' LIMIT 0,1";
 $result=mysql_query($sql);
 $row = mysql_fetch_array($result);