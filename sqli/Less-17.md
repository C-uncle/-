# Less-17

## 0x00 注入

这关不是登录，而是更新密码。

所以不是select语句，而是update语句。

一般upload语句是这样的：update user set passwd='xxxx' where name='xxxx'

![image-20210115101223224](D:\WEB安全\笔记\image\image-20210115101223224.png)

在passwd输入单引号，确定参数被单引号包围。

passwd=123'#  将所有用户密码更新为123

这里用盲注来猜数据。

![image-20210115101423111](D:\WEB安全\笔记\image\image-20210115101423111.png)

只有where后面的条件成立，update才会成功。

![image-20210115101522701](D:\WEB安全\笔记\image\image-20210115101522701.png)

虽然显示内容一样，但是后台数据并没有被修改。

布尔盲注应该是不行了，没有可区分的回显。

尝试时间盲注。

passwd=1234' where if(length(database())=8, sleep(3), 1)#

网站会长时间无响应。

原因：如果不考虑sleep()，那么这条update语句会修改所有数据。对于每条数据，每次修改都要sleep(3)。

因此修改数据所需要的时间等于数据条数*3。

所以网站会长时间无响应。

passwd=1234' where username='admin' and if(length(database())=8, sleep(3), 1)#

这样就可以，但是需要提前知道字段名。

尝试从uname注入。uname作了过滤，无法注入。

报错型盲注。

passwd=123' and extractvalue(1,concat(0x7e,(select @@version),0x7e))#

数据会回显在错误信息里。

extractvalue(目标xml文档，路径) 对xml文档进行查询的函数

路径是可控的。

利用concat()函数将错误语法拼接起来，是extractvalue()报错。

在报错信息中，会显示错误语法和select的结果。

updatexml(目标xml文档，路径，要更新的内容) 

注入方法和extractvalue类似。

## 0x01 分析

有两步。第一步用select语句先确认表中有这个名字。

第二步，用update语句。

无法从unam注入，uname没有传到update语句。update语句中的username用的是select语句的结果。

```php
function check_input($value)
        {
        if(!empty($value))
                {
                // truncation (see comments) 限制长度
                $value = substr($value,0,15);
                }

                // Stripslashes if magic quotes enabled 转义
                if (get_magic_quotes_gpc())
                        {
                        $value = stripslashes($value);
                        }

                // Quote if not a number 
                if (!ctype_digit($value))
                        {
                        $value = "'" . mysql_real_escape_string($value) . "'";
                        }

        else
                {
                $value = intval($value);
                }
        return $value;
        }

$uname=check_input($_POST['uname']);

$passwd=$_POST['passwd'];


@$sql="SELECT username, password FROM users WHERE username= $uname LIMIT 0,1";

$result=mysql_query($sql);
$row = mysql_fetch_array($result);
//echo $row;
        if($row)
        {
                //echo '<font color= "#0000ff">';
                $row1 = $row['username'];
                //echo 'Your Login name:'. $row1;
                $update="UPDATE users SET password = '$passwd' WHERE username='$row1'";
                mysql_query($update);
                echo "<br>";
                if (mysql_error())
                {
                        echo '<font color= "#FFFF00" font size = 3 >';
                        print_r(mysql_error());
                        echo "</br></br>";
                        echo "</font>";
                }
                else
                {
                        echo '<font color= "#FFFF00" font size = 3 >';
                        //echo " You password has been successfully updated " ;
                        echo "<br>";
                        echo "</font>";
                }

                echo '<img src="../images/flag1.jpg"   />';
                //echo 'Your Password:' .$row['password'];
                echo "</font>";
        }
        else
        {
                echo '<font size="4.5" color="#FFFF00">';
                //echo "Bug off you Silly Dumb hacker";
                echo "</br>";
                echo '<img src="../images/slap1.jpg"   />';

                echo "</font>";
        }
```



​    