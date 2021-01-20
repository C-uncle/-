# Less-6

## 0x00 注入

依旧是盲注，双引号闭合

## 0x01 分析

参数被双引号包括

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

        echo '<font size="3"  color= "#FFFF00">';
        print_r(mysql_error());
        echo "</br></font>";
        echo '<font color= "#0000ff" font size= 3>';

        }
}
        else { echo "Please input the ID as parameter with numeric value";}
```
