# IIS 7.0解析漏洞

IIS 7.0 + PHP FASTCGI开启的时候，会出现该漏洞。
	任意文件后加`/.php`，就会按照php file解析

![[Pasted image 20211213175316.png]]
![[Pasted image 20211213175512.png]]

测试：
创建`1.txt`，`1.jpg` 里面有phpinfo，测试漏洞。
![[Pasted image 20211213175655.png]]
![[Pasted image 20211213175736.png]]


请求限制打上勾 修复该漏洞
![[Pasted image 20211213180019.png]]![[Pasted image 20211213180038.png]]