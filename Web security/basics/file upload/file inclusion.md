# 文件包含

##文件包含寻找本地文件

![[Pasted image 20211202154941.png]]
filename可控，php源码是直接使用了include，这就造成了文件包含的漏洞


目标是寻找etc/passwd文件
经过若干查找上级目录：
![[Pasted image 20211202155314.png]]


## 文件包含一句话
先找到上传点，利用文件上传漏洞上传个jpg格式的一句话：shell.jpg

找到它的路径：
``` ../../unsafeupload/uploads/shell.jpg ```
利用 ``` include ``` 的文件包含漏洞
![[Pasted image 20211202160445.png]]



## 文件包含利用日志文件

例： Apache server的日志

burp访问：
![[Pasted image 20211202165903.png]]
![[Pasted image 20211202165939.png]] 留下了日志

就像包含一句话文件一样，利用文件包含的漏洞访问这个日志文件，include会自动解析php....
但是PHP的权限低于系统日志文件的权限，所以在网站上无法访问日志文件，无法利用这个文件包含漏洞。


## 文件包含利用环境变量
和利用日志文件类似，文件包含也可以利用环境变量文件

/proc/self/environ 这个文件里保存了系统的一些变量。


![[Pasted image 20211202170738.png]]

![[Pasted image 20211202171220.png]]
Apache可能会将user-agent写入环境变量。 但是PHP的权限低于系统文件（/proc/self/environ），无法写入，所以不能实际看效果如何。

如果权限足够，可以得到phpinfo






## 伪协议利用

远程文件包含漏洞即include可以读取远程文件（url文件），只有``` allow_url_open ``` 和 ``` allow_url_include ``` 都开启才可以利用。

1. file:// 访问本地文件
![[Pasted image 20211204161139.png]]
2. php://input 利用post上传php恶意代码  : ![[Pasted image 20211204163543.png]]
3. php://filter 可以用来读取文件/源码：例如读取flag.txt里面的东西，再通过decoder decode base64得到结果
![[Pasted image 20211204164720.png]]
参考链接： https://www.php.net/manual/en/wrappers.php.php#wrappers.php.filter


## 远程文件包含
只有``` allow_url_open ``` 和 ``` allow_url_include ``` 都开启才可以利用该漏洞

例：![[Pasted image 20211204171625.png]] 



## 文件包含文件名截断



### %00文件名截断
要利用%00截断这个漏洞，要求PHP版本<5.3.0, 并且 ``` magic_quotes_gpc  ``` 需要关闭。

例：文件包含漏洞
```php
<?php 
	include $_GET['file'].'.php';
?>
```

需要查看flag.txt的文件，就需要把```.php```给截断掉。

![[Pasted image 20211204180501.png]]




## 远程文件包含截断
![[Pasted image 20211204204913.png]]




