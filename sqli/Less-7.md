# Less-7

## 0x00 注入

![image-20210114103112652](D:\WEB安全\笔记\image\image-20210114103112652.png)

单引号报错，但是没有详细的错误信息

![image-20210114103211021](D:\WEB安全\笔记\image\image-20210114103211021.png)

![image-20210114103222806](D:\WEB安全\笔记\image\image-20210114103222806.png)

无法通过--+和#过滤单引号。

![image-20210114103414560](D:\WEB安全\笔记\image\image-20210114103414560.png)

![image-20210114103431687](D:\WEB安全\笔记\image\image-20210114103431687.png)



这里在结尾添加一个and '1'='1 来闭合单引号

![image-20210114103530258](D:\WEB安全\笔记\image\image-20210114103530258.png)



![image-20210114104927759](D:\WEB安全\笔记\image\image-20210114104927759.png)

Use outfile的存在就是在筹字数的，加了后面的字符，这样错误信息和正常信息的长度就一样了。

使用bp爆破，需要设置Grep-match，这样才能分辨错误信息

use outfile是提示。。。。。

select 1 into outfile "路径"； 可以将查询结果导出到文件中。

但是mysql.ini里的secure-file-priv限制了可选路径。

@@datadir 爆数据存放位置

@@basedir 爆安装位置

@@version_compile_OS 爆操作系统

## 0x01 分析

```php
  $sql="SELECT * FROM users WHERE id=(('$id')) LIMIT 0,1";
$result=mysql_query($sql);
$row = mysql_fetch_array($result);

        if($row)
        {
        echo '<font color= "#FFFF00">';
        echo 'You are in.... Use outfile......';
        echo "<br>";
        echo "</font>";
        }
        else
        {
        echo '<font color= "#FFFF00">';
        echo 'You have an error in your SQL syntax';
        //print_r(mysql_error());
        echo "</font>";
        }
}
        else { echo "Please input the ID as parameter with numeric value";}
```
加了括号，难怪注释符号没有效果