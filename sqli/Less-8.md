# Less-8

## 0x00 注入

![image-20210114105618094](D:\WEB安全\笔记\image\image-20210114105618094.png)

![image-20210114105629138](D:\WEB安全\笔记\image\image-20210114105629138.png)

![image-20210114105746622](D:\WEB安全\笔记\image\image-20210114105746622.png)

![image-20210114105804454](D:\WEB安全\笔记\image\image-20210114105804454.png)

参数是字符型的，并且应该使用单引号包围的。

错误信息不回显

## 0x01 分析



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
        //echo 'You are in...........';
        //print_r(mysql_error());
        //echo "You have an error in your SQL syntax";
        echo "</br></font>";
        echo '<font color= "#0000ff" font size= 3>';

        }
}
        else { echo "Please input the ID as parameter with numeric value";}
```
