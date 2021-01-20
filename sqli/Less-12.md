# Less-12

## 0x00 注入

admin' or '1'='1 无效

admin"--+ 报错

![image-20210114163451444](D:\WEB安全\笔记\image\image-20210114163451444.png)

将注入点放在passwd上，爆出一个账户

![image-20210114163548380](D:\WEB安全\笔记\image\image-20210114163548380.png)

参数被括号包围了。

![image-20210114163716806](D:\WEB安全\笔记\image\image-20210114163716806.png)

一样可以爆数据

## 0x01 分析



```php

     $uname='"'.$uname.'"';
     $passwd='"'.$passwd.'"';
     @$sql="SELECT username, password FROM users WHERE username=($uname) and password=($passwd) LIMIT 0,1";

```


​    


