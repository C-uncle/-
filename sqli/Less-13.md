# Less-13

## 0x00  注入

布尔型盲注

回显是图片

![image-20210114164226956](D:\WEB安全\笔记\image\image-20210114164226956.png)

![image-20210114164236517](D:\WEB安全\笔记\image\image-20210114164236517.png)

用bp爆破就行了，和前面一样

## 0x01  分析

值得一提的是，代码里返回的失败页面里包含mysql_error()，但是实际里，页面没有显示错误信息。

知道原因了，因为没有报错，只是单纯的布尔值为false

```php
  @$sql="SELECT username, password FROM users WHERE username=('$uname') and password=('$passwd') LIMIT 0,1";
        $result=mysql_query($sql);
        $row = mysql_fetch_array($result);

        if($row)
        {
                //echo '<font color= "#0000ff">';

                echo "<br>";
                echo '<font color= "#FFFF00" font size = 4>';
                //echo " You Have successfully logged in " ;
                echo '<font size="3" color="#0000ff">';
                echo "<br>";
                //echo 'Your Login name:'. $row['username'];
                //echo "<br>";
                //echo 'Your Password:' .$row['password'];
                //echo "<br>";
                echo "</font>";
                echo "<br>";
                echo "<br>";
                echo '<img src="../images/flag.jpg"   />';

                echo "</font>";
        }
        else
        {
                echo '<font color= "#0000ff" font size="3">';
                //echo "Try again looser";
                print_r(mysql_error());
                echo "</br>";
                echo "</br>";
                echo "</br>";
                echo '<img src="../images/slap.jpg"   />';
                echo "</font>";
        }
```