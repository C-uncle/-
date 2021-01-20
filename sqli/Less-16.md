# Less-16

## 0x00 注入

先尝试加单引号+井号，返回错误

加双引号+井号，错误

双引号+右括号+井号，正确

## 0x01 分析

$uname='"'.$uname.'"'; // 拼接了双引号
$passwd='"'.$passwd.'"';
@$sql="SELECT username, password FROM users WHERE username=($uname) and password=($passwd) LIMIT 0,1"; // sql语句内有括号