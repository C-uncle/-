# Less-19

## 0x00 注入

这次注入点在Referer中。

和less-18类似，使用extractvaluea进行报错注入。

![image-20210115175318474](D:\WEB安全\笔记\image\image-20210115175318474.png)

## 0x01 分析

```php
<?php
//including the Mysql connect parameters.
include("../sql-connections/sql-connect.php");
error_reporting(0);

function check_input($value)
        {
        if(!empty($value))
                {
                // truncation (see comments)
                $value = substr($value,0,20);
                }

                // Stripslashes if magic quotes enabled
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
        $uagent = $_SERVER['HTTP_REFERER'];
        $IP = $_SERVER['REMOTE_ADDR'];
        echo "<br>";
        echo 'Your IP ADDRESS is: ' .$IP;
        echo "<br>";
        //echo 'Your User Agent is: ' .$uagent;
// take the variables
if(isset($_POST['uname']) && isset($_POST['passwd']))
        {
        $uname = check_input($_POST['uname']);
        $passwd = check_input($_POST['passwd']);
        /*
        echo 'Your Your User name:'. $uname;
        echo "<br>";
        echo 'Your Password:'. $passwd;
        echo "<br>";
        echo 'Your User Agent String:'. $uagent;
        echo "<br>";
        echo 'Your User Agent String:'. $IP;
        */
        //logging the connection parameters to a file for analysis.
        $fp=fopen('result.txt','a');
        fwrite($fp,'Referer:'.$uname."\n");
        fclose($fp);

        $sql="SELECT  users.username, users.password FROM users WHERE users.username=$uname and users.password=$passwd O
RDER BY users.id DESC LIMIT 0,1";
        $result1 = mysql_query($sql);
        $row1 = mysql_fetch_array($result1);
                if($row1)
                        {
                        echo '<font color= "#FFFF00" font size = 3 >';
                        $insert="INSERT INTO `security`.`referers` (`referer`, `ip_address`) VALUES ('$uagent', '$IP')";
                        mysql_query($insert);
                        //echo 'Your IP ADDRESS is: ' .$IP;
                        echo "</font>";
                        //echo "<br>";
                        echo '<font color= "#0000ff" font size = 3 >';
                        echo 'Your Referer is: ' .$uagent;
                        echo "</font>";
                        echo "<br>";
                        print_r(mysql_error());
                        echo "<br><br>";
                        echo '<img src="../images/flag.jpg" />';
                        echo "<br>";
                        }
                else
                        {
                        echo '<font color= "#0000ff" font size="3">';
                        //echo "Try again looser";
                        print_r(mysql_error());
                        echo "</br>";
                        echo "</br>";
                        echo '<img src="../images/slap.jpg"  />';
                        echo "</font>";
                        }
        }
?>
```










